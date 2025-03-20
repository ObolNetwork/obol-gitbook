[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterPayload

> **ClusterPayload** = `object`

Defined in: [types.ts:95](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L95)

Cluster configuration

## Extended by

- [`ClusterDefinition`](../interfaces/ClusterDefinition.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | [types.ts:97](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L97) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](ClusterOperator.md)[] | The cluster nodes operators addresses. | [types.ts:100](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L100) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](ClusterValidator.md)[] | The cluster validators information. | [types.ts:103](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L103) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `string`[] \| `null` | The cluster partial deposits in gwei or 32000000000. | [types.ts:106](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L106) |
