[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterPayload

> **ClusterPayload** = `object`

Defined in: [types.ts:109](https://github.com/ObolNetwork/obol-sdk/blob/e7fc737767265d3063c4e96d045f725fadd20e1e/src/types.ts#L109)

Cluster configuration

## Extended by

- [`ClusterDefinition`](../interfaces/ClusterDefinition.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | [types.ts:111](https://github.com/ObolNetwork/obol-sdk/blob/e7fc737767265d3063c4e96d045f725fadd20e1e/src/types.ts#L111) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](ClusterOperator.md)[] | The cluster nodes operators addresses. | [types.ts:114](https://github.com/ObolNetwork/obol-sdk/blob/e7fc737767265d3063c4e96d045f725fadd20e1e/src/types.ts#L114) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](ClusterValidator.md)[] | The cluster validators information. | [types.ts:117](https://github.com/ObolNetwork/obol-sdk/blob/e7fc737767265d3063c4e96d045f725fadd20e1e/src/types.ts#L117) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `string`[] \| `null` | The cluster partial deposits in gwei or 32000000000. | [types.ts:120](https://github.com/ObolNetwork/obol-sdk/blob/e7fc737767265d3063c4e96d045f725fadd20e1e/src/types.ts#L120) |
