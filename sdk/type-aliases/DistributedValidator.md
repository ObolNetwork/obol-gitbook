[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DistributedValidator

> **DistributedValidator** = `object`

Defined in: [types.ts:261](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L261)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="distributed_public_key"></a> `distributed_public_key` | `string` | The public key of the distributed validator. | [types.ts:263](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L263) |
| <a id="public_shares"></a> `public_shares` | `string`[] | The public key of the node distributed validator share. | [types.ts:266](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L266) |
| <a id="deposit_data"></a> `deposit_data?` | `Partial`\<[`DepositData`](DepositData.md)\> | The deposit data for activating the DV. | [types.ts:269](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L269) |
| <a id="partial_deposit_data"></a> `partial_deposit_data?` | `Partial`\<[`DepositData`](DepositData.md)\>[] | The deposit data with partial amounts or full amount for activating the DV. | [types.ts:272](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L272) |
| <a id="builder_registration"></a> `builder_registration?` | [`BuilderRegistration`](BuilderRegistration.md) | pre-generated signed validator builder registration to be sent to builder network. | [types.ts:275](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L275) |
