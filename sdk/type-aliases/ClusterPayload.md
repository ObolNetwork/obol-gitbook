[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterPayload

> **ClusterPayload** = `object`

Defined in: [types.ts:96](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L96)

Cluster configuration

## Extended by

- [`ClusterDefinition`](../interfaces/ClusterDefinition.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | [types.ts:98](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L98) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](ClusterOperator.md)[] | The cluster nodes operators addresses. | [types.ts:101](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L101) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](ClusterValidator.md)[] | The cluster validators information. | [types.ts:104](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L104) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `string`[] \| `null` | The cluster partial deposits in gwei or 32000000000. | [types.ts:107](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L107) |
