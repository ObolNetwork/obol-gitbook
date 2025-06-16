[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterLock

> **ClusterLock** = `object`

Defined in: [types.ts:290](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L290)

Cluster Details after DKG is complete

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="cluster_definition"></a> `cluster_definition` | [`ClusterDefinition`](../interfaces/ClusterDefinition.md) | The cluster definition. | [types.ts:292](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L292) |
| <a id="distributed_validators"></a> `distributed_validators` | [`DistributedValidator`](DistributedValidator.md)[] | The cluster distributed validators. | [types.ts:295](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L295) |
| <a id="signature_aggregate"></a> `signature_aggregate` | `string` | The cluster bls signature aggregate. | [types.ts:298](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L298) |
| <a id="lock_hash"></a> `lock_hash` | `string` | The hash of the cluster lock. | [types.ts:301](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L301) |
| <a id="node_signatures"></a> `node_signatures?` | `string`[] | Node Signature for the lock hash by the node secp256k1 key. | [types.ts:304](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L304) |
