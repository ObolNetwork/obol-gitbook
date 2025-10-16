---
description: >-
  Add operators to your existing distributed validator cluster using the charon alpha edit add-operators command.
---

# Adding Operators

{% hint style="warning" %}
This is an experimental feature and should not be used in production (Mainnet) yet.
{% endhint %}

You can add operators to your cluster using the `charon alpha edit add-operators` command. This operation keeps all validator public keys unchanged whilst adding new operators to the cluster.

## Prerequisites

1. Review the `edit add-operators` command [CLI reference](../../learn/charon/charon-cli-reference.md#add-operators-to-a-cluster).
2. **For existing operators**: Keep the DV node running during the process and ensure you have a copy of the current cluster lock file and validator private key shares.
3. **For new operators**: Obtain a copy of the existing cluster lock file from the existing operators and have your Charon ENR private key file ready.
4. Obtain the Charon ENR addresses of all new operators being added to the cluster.

{% hint style="info" %}
The ceremony uses a different p2p relay from your running cluster to avoid conflicts. The default relay address is already configured differently, so no special action is required.
{% endhint %}

## Adding Operators Process

The examples below demonstrate adding new operators to an existing cluster. All existing operators must run this command, along with the new operators being added.

### For Existing Operators

```bash
# If you prefer running a pre-built charon binary
charon alpha edit add-operators --new-operator-enrs=enr:-JG4QH... --output-dir=output

# Or, if you prefer running it in Docker
# (replace 'latest' with the most recent version if needed: https://hub.docker.com/r/obolnetwork/charon/tags)
docker run --rm -v "$(pwd):/opt/charon" obolnetwork/charon:latest alpha edit add-operators --new-operator-enrs=enr:-JG4QH... --private-key-file=/opt/charon/.charon/charon-enr-private-key --lock-file=/opt/charon/.charon/cluster-lock.json --validator-keys-dir=/opt/charon/.charon/validator_keys --output-dir=/opt/charon/output
```

### For New Operators

New operators being added should run the same command but only need to provide their private key file and the cluster lock file (they won't have validator keys yet):

```bash
# If you prefer running a pre-built charon binary
charon alpha edit add-operators --new-operator-enrs=enr:-JG4QH... --output-dir=output --lock-file=cluster-lock.json --private-key-file=charon-enr-private-key

# Or, if you prefer running it in Docker
docker run --rm -v "$(pwd):/opt/charon" obolnetwork/charon:latest alpha edit add-operators --new-operator-enrs=enr:-JG4QH... --private-key-file=/opt/charon/charon-enr-private-key --lock-file=/opt/charon/cluster-lock.json --output-dir=/opt/charon/output
```

{% hint style="info" %}
To add multiple operators at once, provide a comma-separated list: `--new-operator-enrs=enr:-JG4QH...,enr:-JG4QK...,enr:-JG4QL...`
{% endhint %}

This command will create a new cluster configuration with the additional operators whilst keeping all validator public keys unchanged. The new configuration will be saved in the `output` directory.

## Making the DV Stack Use the New Configuration

The example below is designed for the [CDVN repository](https://github.com/ObolNetwork/charon-distributed-validator-node), but the process is similar for other setups.

1. To start using the new configuration (with the added operators), stop the current Charon and validator client instances:

```bash
docker compose stop charon lodestar
```

2. Back up and remove the existing `.charon` directory, then move the `output` directory to `.charon`:

```bash
mv .charon .charon-backup
mv output .charon
```

3. Restart the Charon and validator client instances:

```bash
docker compose up -d charon lodestar
```

Lodestar's boot script (`lodestar/run.sh`) will automatically import all keys, removing any existing keys and cache. Charon will load the new `cluster-lock.json` and recognise all validators in the cluster with the updated operator set.

{% hint style="warning" %}
All existing operators must fully shut down their cluster nodes before starting with the new configuration. The old cluster must be completely stopped before the new cluster with the expanded operator set can begin operating. Unlike add-validators, this is not a gradual migration.
{% endhint %}

## Current Limitations

- The new cluster configuration will not be reflected on the Launchpad.
- The new cluster configuration will have a new cluster hash, so the observability stack will display new cluster data under a different identifier.
- All operators (both existing and new) must participate in the add-operators ceremony for it to complete successfully.
- The cluster's threshold value remains unchanged after adding operators because the existing set of operators already possesses enough shares to create full signatures.
