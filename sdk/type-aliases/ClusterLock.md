[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterLock

> **ClusterLock** = `object`

Defined in: [types.ts:280](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L280)

Cluster Details after DKG is complete

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="cluster_definition"></a> `cluster_definition` | [`ClusterDefinition`](../interfaces/ClusterDefinition.md) | The cluster definition. | [types.ts:282](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L282) |
| <a id="distributed_validators"></a> `distributed_validators` | [`DistributedValidator`](DistributedValidator.md)[] | The cluster distributed validators. | [types.ts:285](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L285) |
| <a id="signature_aggregate"></a> `signature_aggregate` | `string` | The cluster bls signature aggregate. | [types.ts:288](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L288) |
| <a id="lock_hash"></a> `lock_hash` | `string` | The hash of the cluster lock. | [types.ts:291](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L291) |
| <a id="node_signatures"></a> `node_signatures?` | `string`[] | Node Signature for the lock hash by the node secp256k1 key. | [types.ts:294](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L294) |
