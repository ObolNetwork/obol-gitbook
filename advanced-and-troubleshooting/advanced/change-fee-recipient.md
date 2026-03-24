---
description: Change the fee recipient address for validators in a distributed validator cluster.
---

# Change Fee Recipient

The fee recipient address determines where transaction tips and MEV rewards are sent when a validator proposes a block. In a distributed validator cluster, the fee recipient and gas limit are set at cluster creation time and stored in the `cluster-lock.json` file. The `charon feerecipient` commands allow a threshold of operators to collaboratively update the fee recipient address and optionally the gas limit for one or more validators after the cluster has been created.

{% hint style="info" %}
The updated fee recipient address applies to both MEV (builder API) and non-MEV block proposals.
{% endhint %}

## Prerequisites

- A running distributed validator cluster with Charon `v1.10.0` or later.
- Access to validator private key shares on each operator's node.
- Agreement among a threshold of operators on the new fee recipient address and which validator public keys to update.

## Overview

The workflow involves three steps:

1. **Sign** — A threshold of operators each sign new builder registration messages specifying the new fee recipient address (and optionally a new gas limit).
2. **Fetch** — Any operator fetches the aggregated registrations from the remote API once enough partial signatures have been submitted.
3. **Apply** — Charon automatically detects and applies the updated overrides file. No restart is required.

## 1. Check current fee recipients

Before making changes, list the current fee recipient details for your validators:

```sh
docker run -u $(id -u):$(id -g) --rm -v "$(pwd)/:/opt/charon" obolnetwork/charon:v1.10.0 feerecipient list
```

To check specific validators only:

```sh
docker run -u $(id -u):$(id -g) --rm -v "$(pwd)/:/opt/charon" obolnetwork/charon:v1.10.0 feerecipient list \
  --validator-public-keys="0xYOUR_VALIDATOR_PUBKEY"
```

This displays the most recent builder registration for each validator, selecting the entry with the highest timestamp from either the cluster lock file or the overrides file.

## 2. Sign new fee recipient registrations

A threshold of operators must each run the `feerecipient sign` command with matching parameters. For example, in a 4-node cluster, at least 3 operators must sign.

Each participating operator runs:

```sh
docker run -u $(id -u):$(id -g) --rm -v "$(pwd)/:/opt/charon" obolnetwork/charon:v1.10.0 feerecipient sign \
  --fee-recipient="0xNEW_FEE_RECIPIENT_ADDRESS" \
  --validator-public-keys="0xVALIDATOR_PUBKEY_1,0xVALIDATOR_PUBKEY_2"
```

{% hint style="info" %}
`validator-public-keys` are the distributed validator public keys for which the fee recipient should be updated (find them in your `cluster-lock.json` file or on the DV Launchpad). `fee-recipient` is the new Ethereum address that will receive transaction tips and MEV rewards for the specified validators.
{% endhint %}

{% hint style="info" %}
Operators do not need to sign simultaneously. The first operator to sign sets the timestamp to the current time. Subsequent operators automatically adopt the first signer's timestamp and registration data from the remote API, ensuring all partial signatures are compatible. If operators prefer to coordinate explicitly, they can agree on a Unix timestamp beforehand and pass it with the `--timestamp` flag.
{% endhint %}

### Updating the gas limit

Besides the fee recipient address, the `sign` command also allows you to modify the gas limit for builder registrations by passing the `--gas-limit` flag. If not set, the existing gas limit from the cluster lock or overrides file is used.

```sh
docker run -u $(id -u):$(id -g) --rm -v "$(pwd)/:/opt/charon" obolnetwork/charon:v1.10.0 feerecipient sign \
  --fee-recipient="0xNEW_FEE_RECIPIENT_ADDRESS" \
  --gas-limit=36000000 \
  --validator-public-keys="0xVALIDATOR_PUBKEY_1"
```

## 3. Fetch the aggregated registrations

Once a threshold of operators have submitted their partial signatures, any operator can fetch the fully aggregated builder registrations:

```sh
docker run -u $(id -u):$(id -g) --rm -v "$(pwd)/:/opt/charon" obolnetwork/charon:v1.10.0 feerecipient fetch \
  --validator-public-keys="0xVALIDATOR_PUBKEY_1,0xVALIDATOR_PUBKEY_2"
```

This writes the aggregated registrations to the overrides file at `.charon/builder_registrations_overrides.json`.

{% hint style="info" %}
If not enough operators have signed yet, the command will return an error. Coordinate with your cluster peers to ensure the threshold is met.
{% endhint %}

{% hint style="info" %}
The `--validator-public-keys` flag is optional for the `fetch` command. If omitted, it fetches registrations for all validators in the cluster.
{% endhint %}

## 4. Verify the change

After fetching, confirm the updated fee recipients are in place:

```sh
docker run -u $(id -u):$(id -g) --rm -v "$(pwd)/:/opt/charon" obolnetwork/charon:v1.10.0 feerecipient list
```

You should see the new fee recipient address reflected for the updated validators.

## Automatic application by Charon

A running `charon run` process watches the overrides file for changes using filesystem events and automatically reloads it — **no restart is required**.

Additionally, `charon run` can periodically fetch updated builder registrations from the remote API automatically. To enable this, set the flag `--fetch-feerecipient-updates`:

```sh
charon run --fetch-feerecipient-updates ...
```

When enabled, the background fetch runs on the following schedule:

- Every **24 hours** under normal conditions.
- Every **1 hour** if partial (not yet fully aggregated) entries are detected.
- On every **restart**.

This means that in most cases, once a threshold of operators have signed, a running Charon node with `--fetch-feerecipient-updates` enabled will automatically pick up the new fee recipient without any manual `fetch` step. The manual `charon feerecipient fetch` command is useful for applying the change immediately or for verifying the result before relying on the automatic background fetch.

## Further reading

- [CLI Reference](../../learn/charon/charon-cli-reference.md#the-feerecipient-command) for the full list of `feerecipient` command flags.
- [Enable MEV](enable-mev.md) for configuring the builder API in your cluster.
