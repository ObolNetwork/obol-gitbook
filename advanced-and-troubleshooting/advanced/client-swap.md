---
sidebar_position: 7
description: Client swap
---

# Client swap

If you are using CDVN, the default stack is:

| Execution layer | Consensus layer | Distributed validator | Validator client | MEV       |
| --------------- | --------------- | --------------------- | ---------------- | --------- |
| Geth            | Lighthouse      | Charon                | Lodestar         | MEV boost |

However, the more users there are of CDVN, the more centralisation this creates.
Since v1.6, CDVN provides the option for swapping clinets in the stack.

Currently supported clients are:

| Execution layer | Consensus layer | Distributed validator | Validator client | MEV          |
| --------------- | --------------- | --------------------- | ---------------- | ------------ |
| Nethermind.     | Lighthouse      | Charon                | Lodestar         | MEV boost    |
|                 | Grandine        |                       | Nimbus           | Commit boost |

For support between different combinations, refer to Charon's compatibility matrix, found under release notes for each release.

## Swapping clients in an already running cluster

To achieve this, the currently existing `.env` file has to be replaced with the new `.env` file introduced in v1.6.

1. Copy the new `.env.sample.<NETWORK>` file to `.env`. Make sure custom environment variables are not lost in the process.

> [!IMPORTANT]
> Some environment variables were renamed in order to be client-agnostic. If you have set those environment variables to custom values, after migrating to the multi client setup, use the new ones. They serve the same purpose.
>
> | Old                           | New                              |
> |-------------------------------|--------------------------------- |
> | NETHERMIND_PORT_P2P           | EL_PORT_P2P                      |
> | NETHERMIND_IP_HTTP            | EL_IP_HTTP                       |
> | NETHERMIND_PORT_HTTP          | EL_PORT_HTTP                     |
> | NETHERMIND_IP_ENGINE          | EL_IP_ENGINE                     |
> | NETHERMIND_PORT_ENGINE        | EL_PORT_ENGINE                   |
> | LIGHTHOUSE_PORT_P2P           | CL_PORT_P2P                      |
> | LODESTAR_PORT_METRICS         | VC_PORT_METRICS                  |
> | MEVBOOST_TIMEOUT_GETHEADER    | MEV_TIMEOUT_GETHEADER            |
> | MEVBOOST_TIMEOUT_GETPAYLOAD   | MEV_TIMEOUT_GETPAYLOAD           |
> | MEVBOOST_TIMEOUT_REGVAL       | MEV_TIMEOUT_REGVAL               |
> | MEVBOOST_RELAYS               | MEV_RELAYS                       |
> | NETHERMIND_PROMTAIL_MONITORED | EL_NETHERMIND_PROMTAIL_MONITORED |
> | LIGHTHOUSE_PROMTAIL_MONITORED | CL_LIGHTHOUSE_PROMTAIL_MONITORED |
> | LODESTAR_PROMTAIL_MONITORED   | VC_LODESTAR_PROMTAIL_MONITORED   |
> | MEV_BOOST_PROMTAIL_MONITORED  | MEV_MEV_BOOST_PROMTAIL_MONITORED |

2. Stop the existing cluster that uses the old environment file.

```sh
docker compose --profile "" down
```

3. Start the new cluster that will use the new environment file.

```sh
docker compose up -d
```

At this stage, your CDVN is set to easily swap components of its stack, but is running the same stack currently.

### Swap Consensus layer

> [!NOTE]
> The code snippets under those steps are assuming you are swapping from Lighthouse CL to Grandine CL and you.

1. Stop the existing consensus layer client container.

> [!TIP]
> If you do not want to experience downtime while the new beacon node is syncing, you can set a fallback beacon node for Charon (`CHARON_FALLBACK_BEACON_NODE_ENDPOINTS` env variable) that will be used while the new BN is syncing.
> Note that you need to restart Charon as well in order for it to take effect.

```sh
docker compose down cl-lighthouse
```

2. Update the `CL` environment variable in `.env` to a different supported consensus layer client (i.e.: `cl-grandine`). Supported CL clients are listed on top of this page.

3. Start the new consensus layer client container.

```sh
docker compose up cl-grandine -d
```

4. Restart Charon in order to update the CL client it's querying.

```sh
docker compose down charon
docker compose up charon -d
```

5. After the new consensus layer client is synced and you are assured the new setup is working, you can delete the previous CL client's data in order to save resources.

```sh
rm -rf ./data/lighthouse
```

### Swap Validator client

> [!NOTE]
> The code snippets under those steps are assuming you are swapping from Lodestar VC to Nimbus VC.

1. Stop the existing validator client container.

```sh
docker compose down vc-lodestar
```

2. Update the `VC` environment variable to a different supported validator client (i.e.: `vc-nimbus`). Supported CL clients are listed on top of this page.

3. Start the new validator client container.

```sh
docker compose up vc-nimbus -d
```

4. After the new validator client is started and you are assured the new setup is working, you can delete the previous VC's data in order to save resources

```sh
rm -rf ./data/lodestar
```

### SWAP MEV client

> [!NOTE]
> The code snippets under those steps are assuming you are swapping from MEV Boost to Commit Boost and you are using Lighthouse CL.

> [!NOTE]
> If switching to Commit Boost, you will need to copy a Commit Boost TOML config `commit-boost/config.toml.sample.<NETWORK>` to `commit-boost/config.toml`, as it does not support `.env` configurations yet. Make sure the configuration matches what you have had set for mev-boost, in terms of relays and timeouts.

1. Stop the existing MEV client container.

```sh
docker compose down mev-mevboost
```

2. Update the `MEV` environment variable to a different supported MEV client (i.e.: `mev-commitboost`). Supported MEV clients are listed on top of this page.

3. Start the new MEV client container.

```sh
docker compose up mev-commitboost -d
```

4. Restart the beacon node in order to update the MEV it's querying.

```sh
docker compose down cl-lighthouse
docker compose up cl-lighthouse -d
```

## Choosing clients in fresh cluster

In order to choose which clients to use in a new cluster, simply change the `EL`, `CL`, `MEV` or `VC` variables in the `.env` file. The cluster will use the respective client for each component.
