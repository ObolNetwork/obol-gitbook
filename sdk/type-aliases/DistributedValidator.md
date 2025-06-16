[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DistributedValidator

> **DistributedValidator** = `object`

Defined in: [types.ts:270](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L270)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="distributed_public_key"></a> `distributed_public_key` | `string` | The public key of the distributed validator. | [types.ts:272](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L272) |
| <a id="public_shares"></a> `public_shares` | `string`[] | The public key of the node distributed validator share. | [types.ts:275](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L275) |
| <a id="deposit_data"></a> `deposit_data?` | `Partial`\<[`DepositData`](DepositData.md)\> | The deposit data for activating the DV. | [types.ts:278](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L278) |
| <a id="partial_deposit_data"></a> `partial_deposit_data?` | `Partial`\<[`DepositData`](DepositData.md)\>[] | The deposit data with partial amounts or full amount for activating the DV. | [types.ts:281](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L281) |
| <a id="builder_registration"></a> `builder_registration?` | [`BuilderRegistration`](BuilderRegistration.md) | pre-generated signed validator builder registration to be sent to builder network. | [types.ts:284](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L284) |
