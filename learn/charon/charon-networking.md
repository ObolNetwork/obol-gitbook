# Charon Networking

## Charon networking

### Overview[​](https://docs.obol.org/learn/charon/networking#overview) <a href="#overview" id="overview"></a>

This document describes Charon's networking model which can be divided into two parts: the [_internal validator stack_](https://docs.obol.org/learn/charon/networking#internal-validator-stack) and the [_external p2p network_](https://docs.obol.org/learn/charon/networking#external-p2p-network).

### Internal Validator Stack[​](https://docs.obol.org/learn/charon/networking#internal-validator-stack) <a href="#internal-validator-stack" id="internal-validator-stack"></a>

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Charon is a middleware DVT client and is therefore connected to an upstream beacon node and a downstream validator client is connected to it. Each operator should run the whole validator stack (all 4 client software types), either on the same machine or on different machines. The networking between the nodes should be private and not exposed to the public internet.

Related Charon configuration flags:

* `--beacon-node-endpoints`: Connects Charon to one or more beacon nodes.
* `--validator-api-address`: Address for Charon to listen on and serve requests from the validator client.

### External P2P Network[​](https://docs.obol.org/learn/charon/networking#external-p2p-network) <a href="#external-p2p-network" id="external-p2p-network"></a>

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

The Charon clients in a DV cluster are connected to each other via a small p2p network consisting of only the clients in the cluster. Peer IP addresses are discovered via an external "relay" server. The p2p connections are over the public internet so the Charon p2p port must be publicly accessible. Charon leverages the popular [libp2p](https://libp2p.io/) protocol.

Related [Charon configuration flags](https://docs.obol.org/learn/charon/charon-cli-reference):

* `--p2p-tcp-addresses`: Addresses for Charon to listen on and serve p2p requests.
* `--p2p-relays`: Connect Charon to one or more relay servers.
* `--private-key-file`: Private key identifying the Charon client.

#### LibP2P Authentication and Security[​](https://docs.obol.org/learn/charon/networking#libp2p-authentication-and-security) <a href="#libp2p-authentication-and-security" id="libp2p-authentication-and-security"></a>

Each Charon client has a secp256k1 private key. The associated public key is encoded into the [cluster lock file](https://docs.obol.org/learn/charon/cluster-configuration#cluster-lock-file) to identify the nodes in the cluster. For ease of use and to align with the Ethereum ecosystem, Charon encodes these public keys in the [ENR format](https://eips.ethereum.org/EIPS/eip-778), not in [libp2p’s Peer ID format](https://docs.libp2p.io/concepts/fundamentals/peers/).

{% hint style="warning" %}
Each Charon node's secp256k1 private key is critical for authentication and must be kept secure to prevent cluster compromise.

Do not use the same key across multiple clusters, as this can lead to security issues.

For more on p2p security, refer to [libp2p's article](https://docs.libp2p.io/concepts/security/security-considerations).
{% endhint %}

Charon currently only supports libp2p tcp connections with [noise](https://noiseprotocol.org/) security and only accepts incoming libp2p connections from peers defined in the cluster lock.

#### LibP2P Relays and Peer Discovery[​](https://docs.obol.org/learn/charon/networking#libp2p-relays-and-peer-discovery) <a href="#libp2p-relays-and-peer-discovery" id="libp2p-relays-and-peer-discovery"></a>

Relays are simple libp2p servers that are publicly accessible supporting the [circuit-relay](https://docs.libp2p.io/concepts/nat/circuit-relay/) protocol. Circuit-relay is a libp2p transport protocol that routes traffic between two peers over a third-party “relay” peer.

Obol hosts a publicly accessible relay at [https://0.relay.obol.tech](https://0.relay.obol.tech/) and will work with other organisations in the community to host alternatives Anyone can host their own relay server for their DV cluster.

Each Charon node knows which peers are in the cluster from the ENRs in the cluster lock file, but their IP addresses are unknown. By connecting to the same relay, nodes establish “relay connections” to each other. Once connected via relay they exchange their known public addresses via libp2p’s [identify](https://docs.libp2p.io/concepts/fundamentals/protocols/#identify) protocol. The relay connection is then upgraded to a direct connection. If a node’s public IP changes, nodes once again connect via relay, exchange the new IP, and then connect directly once again.

Note that in order for two peers to discover each other, they must connect to the same relay. Cluster operators should therefore coordinate which relays to use.

Libp2p’s [identify](https://docs.libp2p.io/concepts/fundamentals/protocols/#identify) protocol attempts to automatically detect the public IP address of a Charon client without the need to explicitly configure it. If this however fails, the following two configuration flags can be used to explicitly set the publicly advertised address:

* `--p2p-external-ip`: Explicitly sets the external IP address.
* `--p2p-external-hostname`: Explicitly sets the external DNS host name.

{% hint style="warning" %}
If a pair of Charon clients are not publicly accessible, due to being behind a NAT, they will not be able to upgrade their relay connections to a direct connection. Even though this is supported, it isn’t recommended as relay connections introduce additional latency and reduced throughput and will result in decreased validator effectiveness and possible missed block proposals and attestations.
{% endhint %}

Libp2p’s circuit-relay connections are end-to-end encrypted, even though relay servers accept connections between nodes from multiple different clusters, relays are merely routing opaque connections. And since Charon only accepts incoming connections from other peers in its cluster, the use of a relay doesn’t allow connections between clusters.

Only the following three libp2p protocols are established between a Charon node and a relay itself:

* [circuit-relay](https://docs.libp2p.io/concepts/nat/circuit-relay/): To establish relay e2e encrypted connections between two peers in a cluster.
* [identify](https://docs.libp2p.io/concepts/fundamentals/protocols/#identify): Auto-detection of public IP addresses to share with other peers in the cluster.
* [peerinfo](https://github.com/ObolNetwork/charon/blob/main/app/peerinfo/peerinfo.go): Exchanges basic application [metadata](https://github.com/ObolNetwork/charon/blob/main/app/peerinfo/peerinfopb/v1/peerinfo.proto) for improved operational metrics and observability.\


All other Charon protocols are only established between nodes in the same cluster.

#### Scalable Relay Clusters[​](https://docs.obol.org/learn/charon/networking#scalable-relay-clusters) <a href="#scalable-relay-clusters" id="scalable-relay-clusters"></a>

In order for a Charon client to connect to a relay, it needs the relay's [multiaddr](https://docs.libp2p.io/concepts/fundamentals/addressing/) (containing its public key and IP address). But a single multiaddr can only point to a single relay server which can easily be overloaded if too many clusters connect to it. Charon therefore supports resolving a relay’s multiaddr via HTTP GET request. Since Charon also includes the unique `cluster-hash` header in this request, the relay provider can use [consistent header-based load-balancing](https://cloud.google.com/load-balancing/docs/https/traffic-management-global#traffic_steering_header-based_routing) to map clusters to one of many relays using a single HTTP address.

The relay supports serving its runtime public multiaddrs via its `--http-address` flag.

E.g., [https://0.relay.obol.tech](https://0.relay.obol.tech/) is actually a load-balancer that routes HTTP requests to one of many relays based on the `cluster-hash` header returning the target relay’s multiaddr which the Charon client then uses to connect to that relay.

The charon `--p2p-relays` flag therefore supports both multiaddrs as well as HTTP URls.
