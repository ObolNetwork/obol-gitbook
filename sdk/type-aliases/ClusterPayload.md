[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterPayload

> **ClusterPayload** = `object`

Defined in: [types.ts:96](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L96)

Cluster configuration

## Extended by

- [`ClusterDefinition`](../interfaces/ClusterDefinition.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | [types.ts:98](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L98) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](ClusterOperator.md)[] | The cluster nodes operators addresses. | [types.ts:101](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L101) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](ClusterValidator.md)[] | The cluster validators information. | [types.ts:104](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L104) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `string`[] \| `null` | The cluster partial deposits in gwei or 32000000000. | [types.ts:107](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L107) |
| <a id="compounding"></a> `compounding?` | `boolean` | A withdrawal mechanism with 0x02 withdrawal credentials. | [types.ts:110](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L110) |
| <a id="target_gas_limit"></a> `target_gas_limit?` | `number` | The target gas limit where default is 36M. | [types.ts:113](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L113) |
| <a id="consensus_protocol"></a> `consensus_protocol?` | `string` | The consensus protocol e.g qbft. | [types.ts:116](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L116) |
