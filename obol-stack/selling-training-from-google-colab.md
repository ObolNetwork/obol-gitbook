---
description: Attempted walkthrough for selling autoresearch training from a Google Colab T4 GPU through Obol Stack
---

# Selling training from Google Colab (T4) through Obol Stack

This guide walks through the current best-effort path for selling GPU-backed autoresearch experiment execution from a Google Colab T4 runtime through Obol Stack.

It is written from an actual implementation attempt around the current `feat/autoresearch-integration` work in `obol-stack`.

## What this guide is

This is a practical guide for the **GPU contributor / seller** persona:

- you have access to a Google Colab T4 GPU
- you want to run the `autoresearch-worker` there
- you want Obol Stack to publish a paid x402-gated endpoint for that worker
- you want the worker to be discoverable through ERC-8004 registration metadata

## What was validated while preparing this guide

Validated in the `obol-stack` PR branch behind this guide:

- `autoresearch-worker` exists as a real worker API
- the worker runs one experiment at a time and stores results on disk
- `obol sell http` can attach richer registration metadata
- canonical provenance support is wired through the stack
- the autoresearch coordinator now works against the current 8004scan and remote-signer APIs
- local persona tests cover:
  - GPU seller / worker flow
  - researcher / coordinator flow
  - service-builder + consumer flow using a resume/CV enhancer example
- the x402 BDD integration harness was extended toward richer registry-backed discovery coverage

## What was NOT fully executed from the autonomous environment

The following still require an interactive Google session or real GPU runtime access:

- signing into Google Colab and attaching a T4 runtime
- running a public tunnel from the Colab notebook for a long-lived seller session
- keeping that Colab runtime alive long enough to act as a stable marketplace worker
- a full end-to-end paid training sale from a live Colab notebook through the production stack

So this guide is:
- **implementation-backed**,
- **honest about current limitations**,
- and the best current path to try this on a real machine.

---

## Architecture

At the moment, the most reliable setup is:

```text
Google Colab T4
  └── autoresearch-worker API on :8080
       └── exposed with ngrok
            └── public Colab worker URL

Local machine running Obol Stack
  └── small in-cluster relay service
       └── proxies HTTP inside the cluster to the Colab HTTPS tunnel URL
            └── monetized with `obol sell http`
                 └── x402 gate + route + ERC-8004 registration
```

### Why use an in-cluster relay?

Today, `obol sell http` expects an in-cluster Kubernetes Service and the built-in upstream health check uses:

```text
http://<service>.<namespace>.svc.cluster.local:<port><healthPath>
```

That means the cleanest current path is not pointing Obol directly at an arbitrary external HTTPS URL.
Instead, you run a tiny relay in the cluster and let Obol monetize that relay.

This is a current implementation constraint, not a fundamental product requirement.

---

## Prerequisites

### On your local machine

- Obol Stack running
- OpenClaw agent initialized
- Docker available
- a wallet you want to receive USDC to
- ngrok account and authtoken

### In Google Colab

- a notebook with GPU runtime enabled
- a T4 runtime (or equivalent Colab GPU)
- enough credits to keep the notebook alive during the test

---

## Step 1: Prepare your local Obol Stack machine

Start the stack and the agent:

```bash
obol stack init
obol stack up
obol agent init
```

Confirm the cluster is healthy:

```bash
obol kubectl get pods -A
obol tunnel status
```

If you plan to register the worker publicly, make sure the stack has a reachable public base URL or tunnel.

---

## Step 2: Start the worker in Google Colab

Open a Colab notebook, switch to a GPU runtime, and use a T4 if available.

### 2.1 Install the worker dependencies

In a Colab cell:

```python
!apt-get update -qq
!apt-get install -y git curl python3-venv > /dev/null
!curl -LsSf https://astral.sh/uv/install.sh | sh
!pip -q install pyngrok
```

### 2.2 Clone `obol-stack`

```python
!git clone https://github.com/ObolNetwork/obol-stack.git
%cd obol-stack
```

Use a version that includes the `autoresearch-worker` files.

### 2.3 Prepare or copy your autoresearch workspace

The worker expects a repo/workdir mounted at `/data/autoresearch` in the containerized form, but in Colab you can simply pick a working directory.

For example:

```python
!mkdir -p /content/data/autoresearch
```

If you already have an autoresearch workspace, sync it into that directory.

### 2.4 Start the worker API

```python
import subprocess

proc = subprocess.Popen([
    "python3",
    "internal/embed/skills/autoresearch-worker/scripts/worker_api.py",
    "serve",
    "--repo", "/content/data/autoresearch",
    "--data-dir", "/content/data",
    "--host", "0.0.0.0",
    "--port", "8080",
    "--timeout", "300",
    "--train-command", "python3 train.py",
])
print("worker started", proc.pid)
```

### 2.5 Verify the worker in the notebook

```python
import json, urllib.request
print(json.loads(urllib.request.urlopen("http://127.0.0.1:8080/health").read()))
```

You should see an `ok` health payload.

---

## Step 3: Expose the Colab worker with ngrok

ngrok has a documented Google Colab flow and is the easiest current option here.

### 3.1 Add your ngrok authtoken

```python
from pyngrok import ngrok
ngrok.set_auth_token("YOUR_NGROK_AUTHTOKEN")
```

### 3.2 Start a tunnel to the worker

```python
from pyngrok import ngrok

tunnel = ngrok.connect(8080)
print(tunnel.public_url)
```

Record the URL. Example:

```text
https://example.ngrok-free.app
```

### 3.3 Probe the worker through ngrok

```python
import json, urllib.request
req = urllib.request.Request(
    tunnel.public_url + "/experiment",
    data=json.dumps({"probe": True}).encode(),
    headers={"Content-Type": "application/json"},
    method="POST",
)
print(json.loads(urllib.request.urlopen(req).read()))
```

If that works, the Colab worker is reachable publicly.

---

## Step 4: Create an in-cluster relay to the Colab worker

Because `obol sell http` expects an in-cluster Service, create a tiny relay in the cluster that forwards to the ngrok URL.

Replace `example.ngrok-free.app` below with your actual ngrok hostname.

### 4.1 Create the relay ConfigMap

```bash
cat <<'EOF' | obol kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: colab-worker-relay
  namespace: llm
data:
  app.py: |
    import os
    from http.server import BaseHTTPRequestHandler, HTTPServer
    from urllib.request import Request, urlopen
    from urllib.error import HTTPError

    TARGET = os.environ["TARGET_BASE"]

    class Handler(BaseHTTPRequestHandler):
        def do_GET(self):
            target = TARGET + self.path
            req = Request(target, method="GET")
            try:
                with urlopen(req, timeout=30) as resp:
                    body = resp.read()
                    self.send_response(resp.status)
                    for k, v in resp.headers.items():
                        if k.lower() != "transfer-encoding":
                            self.send_header(k, v)
                    self.end_headers()
                    self.wfile.write(body)
            except HTTPError as e:
                body = e.read()
                self.send_response(e.code)
                self.end_headers()
                self.wfile.write(body)

        def do_POST(self):
            length = int(self.headers.get("Content-Length", "0") or "0")
            body = self.rfile.read(length)
            target = TARGET + self.path
            headers = {k: v for k, v in self.headers.items() if k.lower() != "host"}
            req = Request(target, data=body, headers=headers, method="POST")
            try:
                with urlopen(req, timeout=300) as resp:
                    out = resp.read()
                    self.send_response(resp.status)
                    for k, v in resp.headers.items():
                        if k.lower() != "transfer-encoding":
                            self.send_header(k, v)
                    self.end_headers()
                    self.wfile.write(out)
            except HTTPError as e:
                out = e.read()
                self.send_response(e.code)
                self.end_headers()
                self.wfile.write(out)

    HTTPServer(("0.0.0.0", 8080), Handler).serve_forever()
EOF
```

### 4.2 Create the relay Deployment + Service

```bash
cat <<'EOF' | obol kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: colab-worker-relay
  namespace: llm
spec:
  replicas: 1
  selector:
    matchLabels:
      app: colab-worker-relay
  template:
    metadata:
      labels:
        app: colab-worker-relay
    spec:
      containers:
        - name: relay
          image: python:3.12-slim
          command: ["python", "/app/app.py"]
          env:
            - name: TARGET_BASE
              value: "https://example.ngrok-free.app"
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: app
              mountPath: /app
      volumes:
        - name: app
          configMap:
            name: colab-worker-relay
---
apiVersion: v1
kind: Service
metadata:
  name: colab-worker-relay
  namespace: llm
spec:
  selector:
    app: colab-worker-relay
  ports:
    - name: http
      port: 8080
      targetPort: 8080
EOF
```

### 4.3 Verify the relay

```bash
obol kubectl get pods -n llm -l app=colab-worker-relay
obol kubectl exec -n llm deploy/colab-worker-relay -- python - <<'PY'
import json, urllib.request
print(json.loads(urllib.request.urlopen('http://127.0.0.1:8080/health').read()))
PY
```

If this works, the cluster can reach the Colab worker.

---

## Step 5: Monetize the training worker

Once the relay is healthy, sell it through Obol Stack:

```bash
obol sell http autoresearch-worker \
  --namespace llm \
  --upstream colab-worker-relay \
  --port 8080 \
  --health-path /health \
  --wallet 0xYOUR_WALLET \
  --chain base-sepolia \
  --per-hour 0.50 \
  --path /services/autoresearch-worker \
  --register \
  --register-name "Colab T4 Worker" \
  --register-description "Google Colab T4 worker for paid autoresearch experiments" \
  --register-skills machine_learning/model_optimization \
  --register-domains technology/artificial_intelligence/research \
  --register-metadata gpu=T4 \
  --register-metadata framework=autoresearch \
  --register-metadata runtime=google-colab
```

Then check status:

```bash
obol sell status autoresearch-worker -n llm
obol kubectl get serviceoffers.obol.org autoresearch-worker -n llm -o yaml
```

You want the conditions to progress to `Ready=True`.

---

## Step 6: Probe the paid endpoint

Once the offer is ready, probe it from the local machine or from inside the agent:

```bash
curl -i -X POST http://obol.stack:8080/services/autoresearch-worker/experiment \
  -H 'Content-Type: application/json' \
  -d '{"probe": true}'
```

Expected:
- `402 Payment Required`
- x402 pricing fields in the response body/headers

That confirms the seller path is live.

---

## Step 7: Submit a real experiment

Once the coordinator or a buyer is wired to the endpoint, the worker expects a payload like:

```json
{
  "train_py": "print('training script here')",
  "config": {
    "batch_size": 64,
    "learning_rate": 0.001
  }
}
```

In a full researcher flow, the autoresearch coordinator does this for you and attaches payment.

---

## Operational caveats

### 1. Colab sessions are not stable seller infrastructure

Google Colab runtimes can:
- stop unexpectedly
- time out
- disconnect tunnels
- rotate public URLs

This makes Colab good for:
- prototyping
- validating the worker UX
- testing the marketplace path

It is not ideal for long-lived sellers.

### 2. The relay is a current workaround

Today, the relay exists because `obol sell http` expects an in-cluster Service.
A future improvement would be first-class support for external HTTPS upstreams.

### 3. Pricing is approximate

Current `--per-hour` pricing is converted into a request price using the 5-minute experiment budget assumption.
That is good enough for the current autoresearch worker model, but it is still an approximation.

---

## Recommended next step after the seller works

Once the Colab worker is sellable, the next downstream path is:

1. run remote autoresearch experiments through the paid worker
2. publish the best result with canonical provenance
3. expose optimized inference
4. build an x402-gated application on top (for example the resume/CV enhancer flow)

This is where the resume example becomes useful as the **service-builder + consumer** persona path.

---

## Troubleshooting

### The relay health check fails

Check:

```bash
obol kubectl logs -n llm deploy/colab-worker-relay
```

Most common causes:
- the ngrok URL changed
- the Colab notebook went idle
- the worker process crashed in Colab

### `obol sell http` never becomes Ready

Inspect:

```bash
obol kubectl get serviceoffers.obol.org autoresearch-worker -n llm -o yaml
obol kubectl get events -n llm
```

### The Colab worker is reachable directly but not through Obol

Verify each hop separately:

1. Colab localhost → worker health
2. ngrok URL → worker health
3. relay pod → ngrok health
4. `obol stack` route → x402 402 response

---

## Final note

This guide reflects the current state of the implementation work.
It is the best practical path today, but not yet the final product experience.

The core seller pieces are now in place. The remaining work is mostly around packaging, smoother external-upstream support, and more end-to-end production validation.
