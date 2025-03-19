[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterPayload

> **ClusterPayload** = `object`

Defined in: [types.ts:109](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L109)

Cluster configuration

## Extended by

- [`ClusterDefinition`](../interfaces/ClusterDefinition.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | [types.ts:111](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L111) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](ClusterOperator.md)[] | The cluster nodes operators addresses. | [types.ts:114](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L114) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](ClusterValidator.md)[] | The cluster validators information. | [types.ts:117](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L117) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `string`[] \| `null` | The cluster partial deposits in gwei or 32000000000. | [types.ts:120](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L120) |
