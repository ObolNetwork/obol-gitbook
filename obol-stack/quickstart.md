---
description: Get started with Obol Stack in under 5 minutes
---

# Quickstart

This guide walks you through installing the Obol Stack, setting up an AI agent, and optionally deploying a blockchain network.

## Prerequisites

* Docker installed and running on your machine.
* macOS or Linux operating system.
* At least 8 GB of RAM available.

{% hint style="info" %}
Ensure Docker is running before proceeding. You can verify this by running `docker info` in your terminal.
{% endhint %}

## Step 1: Install Obol Stack

Run the bootstrap installer:

```shell
bash <(curl -s https://stack.obol.org)
```

The installer will:

1. Validate that Docker is running.
2. Install the `obol` CLI binary and dependencies (kubectl, helm, k3d, helmfile, k9s).
3. Configure your PATH and add `obol.stack` to `/etc/hosts`.
4. Offer to start the cluster immediately.

{% tabs %}
{% tab title="Default installation" %}
```shell
bash <(curl -s https://stack.obol.org)
```

Files are installed to:

* Config: `~/.config/obol/`
* Data: `~/.local/share/obol/`
* Binaries: `~/.local/bin/`
{% endtab %}

{% tab title="Specific version" %}
```shell
OBOL_RELEASE=v0.5.0 bash <(curl -s https://stack.obol.org)
```
{% endtab %}

{% tab title="Development mode" %}
```shell
git clone https://github.com/ObolNetwork/obol-stack.git
cd obol-stack
OBOL_DEVELOPMENT=true ./obolup.sh
```

Development mode uses a local `.workspace/` directory and runs `go run` instead of a compiled binary.
{% endtab %}
{% endtabs %}

## Step 2: Start the stack

```shell
obol stack init
obol stack up
```

{% hint style="info" %}
The first startup may take a few minutes as Docker pulls the required images.
{% endhint %}

## Step 3: Set up the AI agent

Initialize an OpenClaw agent instance:

```shell
obol agent init
```

This walks you through choosing a model provider:

* **Ollama** (local, free) - if Ollama is detected on your machine
* **Anthropic** or **OpenAI** - routed through the in-cluster llmspy gateway

Once complete, open the agent dashboard:

```shell
obol openclaw dashboard
```

Each agent is automatically provisioned with an Ethereum signing wallet (via the remote-signer service). The wallet address is displayed during setup.

{% hint style="success" %}
You can reconfigure the model provider at any time with `obol openclaw setup`.
{% endhint %}

## Step 4: Deploy a blockchain network (optional)

The stack ships with built-in RPC access to Ethereum mainnet and Hoodi via Obol's RPC — no node required. However if you're agent is doing a lot of requests or running a distributed validator, you might want to run your own local node:

```shell
# Install an Ethereum node on Hoodi testnet
obol network install ethereum --network=hoodi

# Deploy to the cluster
obol network sync ethereum
```

This creates the deployment `ethereum/hoodi` and registers the local node as the primary RPC upstream, with the built-in public RPCs as automatic fallback.

Check the deployment:

```shell
obol kubectl get pods -n ethereum-hoodi
```

{% hint style="info" %}
You can also deploy mainnet (`obol network install ethereum`) or sepolia (`obol network install ethereum --network=sepolia`). Run multiple networks side by side — use `obol network sync ethereum` or `obol network sync ethereum/hoodi` to target a specific one.
{% endhint %}

## Step 5: Explore

```shell
# Interactive cluster UI. (Press '0' to view all)
obol k9s

# View all pods
obol kubectl get pods -A

# Check tunnel status (public URL)
obol tunnel status

# Configure model provider globally
obol model setup
```

## Stopping and cleaning up

```shell
# Stop the cluster (preserves data)
obol stack down

# Restart
obol stack up

# Remove everything including data
obol stack purge -f
```

{% hint style="warning" %}
`obol stack purge -f` is irreversible. It removes all cluster data and configuration.
{% endhint %}

## Next steps

* [Installing networks](installing-networks.md) - Deploy different blockchain networks.
* [Installing apps](installing-apps.md) - Deploy additional applications.
* [FAQ](faq.md) - Common questions and troubleshooting.
