[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DistributedValidator

> **DistributedValidator** = `object`

Defined in: [types.ts:274](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L274)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="distributed_public_key"></a> `distributed_public_key` | `string` | The public key of the distributed validator. | [types.ts:276](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L276) |
| <a id="public_shares"></a> `public_shares` | `string`[] | The public key of the node distributed validator share. | [types.ts:279](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L279) |
| <a id="deposit_data"></a> `deposit_data?` | `Partial`\<[`DepositData`](DepositData.md)\> | The deposit data for activating the DV. | [types.ts:282](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L282) |
| <a id="partial_deposit_data"></a> `partial_deposit_data?` | `Partial`\<[`DepositData`](DepositData.md)\>[] | The deposit data with partial amounts or full amount for activating the DV. | [types.ts:285](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L285) |
| <a id="builder_registration"></a> `builder_registration?` | [`BuilderRegistration`](BuilderRegistration.md) | pre-generated signed validator builder registration to be sent to builder network. | [types.ts:288](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L288) |
