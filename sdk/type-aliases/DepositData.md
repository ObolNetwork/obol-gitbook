[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DepositData

> **DepositData** = `object`

Defined in: [types.ts:250](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L250)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="pubkey"></a> `pubkey` | `string` | The public key of the distributed validator. | [types.ts:252](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L252) |
| <a id="withdrawal_credentials"></a> `withdrawal_credentials` | `string` | The 0x01 withdrawal address of the DV. | [types.ts:255](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L255) |
| <a id="amount"></a> `amount` | `string` | 32 ethers. | [types.ts:258](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L258) |
| <a id="deposit_data_root"></a> `deposit_data_root` | `string` | A checksum for DepositData fields . | [types.ts:261](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L261) |
| <a id="signature"></a> `signature` | `string` | BLS signature of the deposit message. | [types.ts:264](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L264) |
