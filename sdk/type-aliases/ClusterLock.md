[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterLock

> **ClusterLock** = `object`

Defined in: [types.ts:294](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L294)

Cluster Details after DKG is complete

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="cluster_definition"></a> `cluster_definition` | [`ClusterDefinition`](../interfaces/ClusterDefinition.md) | The cluster definition. | [types.ts:296](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L296) |
| <a id="distributed_validators"></a> `distributed_validators` | [`DistributedValidator`](DistributedValidator.md)[] | The cluster distributed validators. | [types.ts:299](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L299) |
| <a id="signature_aggregate"></a> `signature_aggregate` | `string` | The cluster bls signature aggregate. | [types.ts:302](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L302) |
| <a id="lock_hash"></a> `lock_hash` | `string` | The hash of the cluster lock. | [types.ts:305](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L305) |
| <a id="node_signatures"></a> `node_signatures?` | `string`[] | Node Signature for the lock hash by the node secp256k1 key. | [types.ts:308](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L308) |
