# Enable MEV

This quickstart guide focuses on configuring the builder API for Charon and supported validator and consensus clients.

### Getting started with Charon & the Builder API[​](https://docs.obol.org/next/adv/advanced/quickstart-builder-api#getting-started-with-charon--the-builder-api) <a href="#getting-started-with-charon--the-builder-api" id="getting-started-with-charon--the-builder-api"></a>

Running a distributed validator cluster with the builder API enabled will give the validators in the cluster access to the builder network. This builder network is a network of "Block Builders" who work with MEV searchers to produce the most valuable blocks a validator can propose.

[MEV-Boost](https://boost.flashbots.net/) is one such product from Flashbots that enables you to ask multiple block relays (who communicate with the "Block Builders") for blocks to propose. The block that pays the largest reward to the validator will be signed and returned to the relay for broadcasting to the wider network. The end result for the validator is generally an increased APR as they receive some share of the MEV.

{% hint style="info" %}
Before completing this guide, please check your cluster version, which can be found inside the `cluster-lock.json` file. If you are using cluster-lock version `1.7.0` or higher, Charon seamlessly accommodates all validator client implementations within a MEV-enabled distributed validator cluster.

For clusters with a `cluster-lock.json` version `1.6.0` and below, Charon is compatible only with [Teku](https://github.com/ConsenSys/teku). Use the version history feature of this documentation to see the instructions for configuring a cluster in that manner (`v0.16.0`).
{% endhint %}

### Client configuration[​](https://docs.obol.org/next/adv/advanced/quickstart-builder-api#client-configuration) <a href="#client-configuration" id="client-configuration"></a>

{% hint style="info" %}
You need to add CLI flags to your consensus client, Charon client, and validator client, to enable the builder API.

You need all operators in the cluster to have their nodes properly configured to use the builder API, or you risk missing a proposal.
{% endhint %}

#### Charon[​](https://docs.obol.org/next/adv/advanced/quickstart-builder-api#charon) <a href="#charon" id="charon"></a>

Charon supports builder API with the `--builder-api` flag. To use builder API, one simply needs to add this flag to the `charon run` command:

```
charon run --builder-api
```

#### Consensus Clients[​](https://docs.obol.org/next/adv/advanced/quickstart-builder-api#consensus-clients) <a href="#consensus-clients" id="consensus-clients"></a>

The following flags need to be configured on your chosen consensus client. A Flashbots relay URL is provided for example purposes, you should use the [charon test mev command](https://docs.obol.org/next/run/prepare/test-command#test-mev-relay) and select the two or three relays with the lowest latency to your node that also conform to your block building preferences. A public list of MEV relays is available [here](https://github.com/eth-educators/ethstaker-guides/blob/main/MEV-relay-list.md#mev-relay-list-for-mainnet).

{% tabs %}
{% tab title="Teku" %}
Teku can communicate with a single relay directly:

```
teku --builder-endpoint="https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net"
```

Or you can configure it to communicate with a local [MEV-boost](https://github.com/flashbots/mev-boost) sidecar to configure multiple relays:

```
teku --builder-endpoint=http://mev-boost:18550
```
{% endtab %}

{% tab title="Lighthouse" %}
Lighthouse can communicate with a single relay directly:

```
lighthouse bn --builder="https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net"
```

Or you can configure it to communicate with a local [MEV-boost](https://github.com/flashbots/mev-boost) sidecar to configure multiple relays:

```
lighthouse bn --builder="http://mev-boost:18550"
```
{% endtab %}

{% tab title="Prysm" %}
Prysm can communicate with a single relay directly:

```
prysm beacon-chain --http-mev-relay="https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net"
```
{% endtab %}

{% tab title="Nimbus" %}
Nimbus can communicate with a single relay directly:

```
nimbus_beacon_node \
      --payload-builder=true \
      --payload-builder-url="https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net"
```

You should also consider adding `--local-block-value-boost=3` as a flag, to favour locally built blocks if they are withing 3% in value of the relay block, to improve the chances of a successful proposal.
{% endtab %}

{% tab title="Lodestar" %}
Lodestar can communicate with a single relay directly:

```
node ./lodestar --builder --builder.urls="https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net"
```
{% endtab %}
{% endtabs %}

#### Validator Clients[​](https://docs.obol.org/next/adv/advanced/quickstart-builder-api#validator-clients) <a href="#validator-clients" id="validator-clients"></a>

The following flags need to be configured on your chosen validator client

{% tabs %}
{% tab title="Teku" %}
```
teku validator-client --validators-builder-registration-default-enabled=true
```
{% endtab %}

{% tab title="Lighthouse" %}
```
lighthouse vc --builder-proposals
```
{% endtab %}

{% tab title="Prysm" %}
```
prysm validator --enable-builder
```
{% endtab %}

{% tab title="Nimbus" %}
```
nimbus_validator_client --payload-builder=true
```
{% endtab %}

{% tab title="Lodestar" %}
```
node ./lodestar validator --builder="true" --builder.selection="builderonly"
```
{% endtab %}
{% endtabs %}

### Verify your cluster is correctly configured[​](https://docs.obol.org/next/adv/advanced/quickstart-builder-api#verify-your-cluster-is-correctly-configured) <a href="#verify-your-cluster-is-correctly-configured" id="verify-your-cluster-is-correctly-configured"></a>

It can be difficult to confirm everything is configured correctly with your cluster until a proposal opportunity arrives, but here are some things you can check.

When your cluster is running, you should see if Charon is logging something like this each epoch:

```
13:10:47.094 INFO bcast      Successfully submitted validator registration to beacon node {"delay": "24913h10m12.094667699s", "pubkey": "84b_713", "duty": "1/builder_registration"}
```

This indicates that your Charon node is successfully registering with the relay for a blinded block when the time comes.

If you are using the [Ultrasound Relay](https://relay.ultrasound.money/), you can enter your cluster's distributed validator public key(s) into their website, to confirm they also see the validator as correctly registered. If you are using [Titan Relay](https://titanrelay.xyz), you can check their API by running `curl https://titanrelay.xyz/relay/v1/data/validator_registration?pubkey=0x123..456` with the public key of a validator in your cluster.

You should check that your validator client's logs look healthy, and ensure that you haven't added a `fee-recipient` address that conflicts with what has been selected by your cluster in your `cluster-lock.json` file, as that may prevent your validator from producing a signature for the block when the opportunity arises. You should also confirm the same for all of the other peers in your cluster.

Once a proposal has been made, you should look at the `Block Extra Data` field under `Execution Payload` for the block on [Beaconcha.in](https://beaconcha.in/block/18450364), and confirm there is text present, this generally suggests the block came from a builder, and was not a locally constructed block.
