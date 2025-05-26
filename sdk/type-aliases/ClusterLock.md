[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterLock

> **ClusterLock** = `object`

Defined in: [types.ts:281](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L281)

Cluster Details after DKG is complete

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="cluster_definition"></a> `cluster_definition` | [`ClusterDefinition`](../interfaces/ClusterDefinition.md) | The cluster definition. | [types.ts:283](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L283) |
| <a id="distributed_validators"></a> `distributed_validators` | [`DistributedValidator`](DistributedValidator.md)[] | The cluster distributed validators. | [types.ts:286](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L286) |
| <a id="signature_aggregate"></a> `signature_aggregate` | `string` | The cluster bls signature aggregate. | [types.ts:289](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L289) |
| <a id="lock_hash"></a> `lock_hash` | `string` | The hash of the cluster lock. | [types.ts:292](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L292) |
| <a id="node_signatures"></a> `node_signatures?` | `string`[] | Node Signature for the lock hash by the node secp256k1 key. | [types.ts:295](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L295) |
