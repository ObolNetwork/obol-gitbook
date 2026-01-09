# Sharing Operator Logs

## Why Sharing Operator Logs Is Crucial for Validator Performance

As an operator running one or more validator nodes, it’s essential that every component in your stack Execution Layer (EL), Consensus Layer (CL), Charon, and your Validator Client (VC) is operating smoothly. Any degradation in these components can directly impact validator duties, leading to missed attestations, lost proposals, and ultimately, reduced rewards.

Even small issues, if unnoticed, can compound over time and cause significant reward losses.

Sharing your operator logs with our support team allows us to quickly diagnose performance issues, spot early warning signs, and guide you toward restoring and optimizing your setup. Logs give us the visibility needed to help you maintain a healthy, high-performing validator

---

## Why Sharing Logs Helps You (Benefits)

### **1. Access to a team of node-operations experts**

You get direct support from engineers who specialize in EL, CL, Charon, and validator performance tuning. This expertise is immediately available whenever you run into problems.

We provide insights and recommendations to improve your setup, including hardware, networking, client configurations, and monitoring strategies. As we develop advanced analysis capabilities, we'll be able to offer even more architecture optimization recommendations

### **2. Faster issue diagnosis and resolution**

Logs allow us to diagnose bottlenecks, misconfigurations, sync issues, or network faults much faster than manual troubleshooting. Typically, we aim to provide resolution within **24 hours** for operators streaming logs to us.

### **3. Reduced reward loss through faster diagnosis**

When issues occur, logs help us quickly diagnose the root cause so you can resolve problems faster and minimize missed attestations, missed sync committee duties, or outages that impact your rewards.

### **4. Future operator scoring and reputation**

As the network evolves, sharing logs can help identify which operator is acting as the leader and which ones are following in Charon’s internal consensus. This information may also be useful in the future for building operator reputation or scoring systems based on performance and availability.

### **5. Contribution to ecosystem improvements**

By sharing logs, you help us improve tooling, documentation, client implementations, and the overall reliability of the distributed validator ecosystem.

---

## Concerns (and why they are minimal)

### **1. Some manual effort is required to gather and share logs**

Yes, collecting logs takes a few minutes only in case of manual, but the reward savings and stability improvements far outweigh the effort.

### **2. Security Concerns**

Logs contain only operational information such as timestamps, connection states, performance metrics, and error messages. **No private keys, validator secrets, or sensitive credentials are ever included in logs.** The information we receive is purely diagnostic and cannot be used to access your validator or funds

Examples:

```jsx
- EL logs:
2025-12-12T12:19:36.510555762Z Dec 12 12:19:36.510 INFO  New block received                            slot: 13226496, root: 0xf7f43a510bc6019cad42f08c0f0321250e33dae22fa605375426f60f9d4b3e72
2025-12-12T12:19:41.001534562Z Dec 12 12:19:41.001 INFO  Synced                                        peers: "208", exec_hash: "0xe404cb12920e2c6a251099fae517b01b99602cf25884c44a372bc36b6e964592 (verified)", finalized_root: 0x73b63e11c4cf6c8ddd7414629eeeb60cd6c2b3c0c92fdb35d1766c1c029f35b0, finalized_epoch: 413326, epoch: 413328, block: "0xf7f43a510bc6019cad42f08c0f0321250e33dae22fa605375426f60f9d4b3e72", slot: 13226496
2025-12-12T12:19:47.056939365Z Dec 12 12:19:47.056 INFO  Forwarding register validator request to connected builder  count: 2
2025-12-12T12:19:49.570712132Z Dec 12 12:19:49.570 INFO  New block received                            slot: 13226497, root: 0x5c42859eb459685654e12585d1a77603bcd54f20598549aad2b6012ce639b508
2025-12-12T12:19:51.166908365Z Dec 12 12:19:51.166 WARN  Error processing HTTP API request             elapsed_ms: 0.18668800000000002, status: 404 Not Found, path: /eth/v1/beacon/headers/13226494, method: GET
2025-12-12T12:19:51.743691872Z Dec 12 12:19:51.741 WARN  Error processing HTTP API request             elapsed_ms: 3.286889, status: 404 Not Found, path: /eth/v2/beacon/blocks/13226494/attestations, method: GET
```

```jsx
- CL logs:
2025-12-12T12:19:01.254636135Z INFO [12-12|12:19:01.254] Imported new potential chain segment     number=23,996,499 hash=74c829..f4aac9 blocks=1   txs=244   mgas=19.683   elapsed=78.280ms     mgasps=251.441  triediffs=272.46MiB triedirty=9.29MiB
2025-12-12T12:19:01.435859152Z INFO [12-12|12:19:01.435] Chain head was updated                   number=23,996,499 hash=74c829..f4aac9 root=0dadea..44b52a elapsed=4.358711ms
2025-12-12T12:19:07.011185541Z INFO [12-12|12:19:07.011] Starting work on payload                 id=0x0309baf212d4da7f
2025-12-12T12:19:07.290211622Z INFO [12-12|12:19:07.290] Updated payload                          id=0x0309baf212d4da7f number=23,996,500 hash=09ec69..bd4c95 txs=76    withdrawals=16 gas=4,938,072  fees=0.003694953029  root=da93dd..f1aef1 elapsed=278.204ms
2025-12-12T12:19:09.334609001Z INFO [12-12|12:19:09.334] Updated payload                          id=0x0309baf212d4da7f number=23,996,500 hash=94e49a..3a4d17 txs=89    withdrawals=16 gas=5,572,818  fees=0.004453971092  root=59fd35..8df495 elapsed=44.168ms
2025-12-12T12:19:11.158297982Z INFO [12-12|12:19:11.158] Stopping work on payload                 id=0x0309baf212d4da7f reason=delivery
```

```jsx
- Charon logs:
11:46:03.002 DEBG qbft       QBFT consensus instance starting         {"peers": "[0:good-colors 1:faithful-park 2:engaging-pencil 3:nervous-idea 4:pleasant-result 5:average-road 6:clear-leg]", "timer": "eager_dlinear", "duty": "12311928/attester"}
11:46:03.080 DEBG qbft       QBFT upon rule triggered                 {"rule": "justified_pre_prepare", "round": 1, "duty": "12311928/attester"}
11:46:03.242 DEBG qbft       QBFT upon rule triggered                 {"rule": "quorum_prepares", "round": 1, "duty": "12311928/attester"}
11:46:03.769 DEBG qbft       QBFT upon rule triggered                 {"rule": "quorum_commits", "round": 1, "duty": "12311928/attester"}
11:46:03.770 DEBG qbft       QBFT consensus decided                   {"duty": "attester", "slot": 12311928,
```

```jsx
- MEV Logs:
2025-08-07T17:15:11.010215026+05:30 stdout F time="2025-08-07T11:45:11.009Z" level=debug msg="handling request" method=registerValidator version=v1.9.0
2025-08-07T17:15:11.010227589+05:30 stdout F time="2025-08-07T11:45:11.009Z" level=info msg="calling registerValidator on relays" method=registerValidator numRelays=2 regBytes=900 timeout=3s ua=Lodestar/v1.31.0/64823d4 version=v1.9.0
2025-08-07T17:15:11.010229487+05:30 stdout F time="2025-08-07T11:45:11.009Z" level=debug msg="sending the registerValidator request" method=registerValidator request="&{POST [https://global.titanrelay.xyz/eth/v1/builder/validators](https://global.titanrelay.xyz/eth/v1/builder/validators) HTTP/1.1 1 1 map[Accept:[*/*] Accept-Encoding:[gzip, deflate] Accept-Language:[*] Connection:[keep-alive] Content-Type:[application/octet-stream] Date-Milliseconds:[1754567111009] Sec-Fetch-Mode:[cors] User-Agent:[mev-boost/v1.9.0 Lodestar/v1.31.0/64823d4]] {0xc000396930} 0x750520 900 [] false global.titanrelay.xyz map[] map[] <nil> map[]   <nil> <nil> <nil>  {{}} <nil> [] map[]}" ua=Lodestar/v1.31.0/64823d4 url="[https://global.titanrelay.xyz/eth/v1/builder/validators](https://global.titanrelay.xyz/eth/v1/builder/validators)" version=v1.9.0
```

```jsx
- VC logs:
Aug-07 11:46:03.833[]                [34mdebug[39m: Signed attestation slot=12311928, index=0, head=0x98c953e9f561b3a51d908bee580c6906e2cce2b3ee95b34d3de675771488e298, validatorIndex=1598288
Aug-07 11:46:03.833[]                [34mdebug[39m: API request routeId=submitPoolAttestationsV2, requestWireFormat=json, responseWireFormat=ssz
Aug-07 11:46:03.835[]                [34mdebug[39m: API response success routeId=submitPoolAttestationsV2, status=200, wireFormat=null
Aug-07 11:46:03.835[]                 [32minfo[39m: Published attestations slot=12311928, index=0,
```

## How to Share Logs

### **Sharing Charon Logs**

Charon logs can be shared via two methods:

**1. Manual sharing (Not Recommended)**

A cumbersome approach and much slower process, where you typically notice an issue, then fetch the logs and send them to us from your setup

To query and send logs manually:

- Head to the node where the distributed validator operates
- Use Docker commands to extract logs, some examples are given:

```bash
# Get last N lines with specific pattern
docker logs container_name --tail <lines> | grep "<required_pattern>"

# Follow logs in real-time
docker logs container_name -f

# From a particular timestamp
docker logs container_name --since "2026-01-01T00:00:00"
```

- Save the logs to a file and share them with the Obol support team

**Where to send manual logs:** Contact the Obol customer success team for the appropriate submission method.

**2. Automated streaming (Recommended)**

Using Prometheus remote write to stream logs continuously to our monitoring infrastructure.

To set up automated log streaming:

1. Contact the **Obol customer success team** to obtain your remote write token
2. Configure your Charon setup with the provided token
3. Logs will automatically stream to our monitoring infrastructure

In CDVN we set the value for this variable in .env file [here](https://github.com/ObolNetwork/charon-distributed-validator-node/blob/849eb9002611abce4a5d117123bbfe35602ac895/.env.sample.mainnet#L189C1-L191C1)

```jsx
# Prometheus remote write token used for accessing external prometheus.
#PROM_REMOTE_WRITE_TOKEN=
```

### **Sharing Beacon Node (BN) Logs**

### **How to manually share BN logs**

For most common setups with beacon nodes running in a cluster:

1. Access the node or cluster where your beacon node is running
2. Use the appropriate command for your BN client (Lighthouse, Prysm, Teku, Nimbus, Lodestar)
3. Extract and save the relevant log sections
4. Share with the Obol support team

```jsx
docker logs <container-name> | grep <slot# or timeframe>
```

- Save the logs to a file and share them with the Obol support team in the comm channel we requested them (TG, Discord, Airtable etc)

---

## Frequently Asked Questions

### **How do you address security concerns about sharing logs?**

Logs contain only operational information such as timestamps, connection states, performance metrics, and error messages. **No private keys, validator secrets, or sensitive credentials are ever included in logs.** The information we receive is purely diagnostic and cannot be used to access your validator or funds.

### **What's the SLA difference between manual and automated log sharing?**

**Manual log sharing:**

- **No guaranteed SLA** – Resolution time depends on when you notice an issue, when you share logs, and how many back-and-forth exchanges are needed
- Can take multiple rounds of log requests to identify the root cause
- Delayed log sharing may result in incomplete diagnostic information if the issue is no longer occurring
- Typical resolution time: **Variable**

**Automated log streaming:**

- **Target SLA: 24 hours for resolution**
- Our team actively monitors all clusters streaming logs
- We proactively detect issues and notify you immediately
- We can provide specific guidance or ask you to make changes as soon as problems are identified
- Much faster diagnosis since we have continuous visibility

### **How can I stop streaming logs?**

To stop streaming logs, simply remove the Prometheus remote write token from your node configuration. Once the token is removed, log streaming will cease immediately.