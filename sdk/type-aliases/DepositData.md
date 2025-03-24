[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DepositData

> **DepositData** = `object`

Defined in: [types.ts:241](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L241)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="pubkey"></a> `pubkey` | `string` | The public key of the distributed validator. | [types.ts:243](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L243) |
| <a id="withdrawal_credentials"></a> `withdrawal_credentials` | `string` | The 0x01 withdrawal address of the DV. | [types.ts:246](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L246) |
| <a id="amount"></a> `amount` | `string` | 32 ethers. | [types.ts:249](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L249) |
| <a id="deposit_data_root"></a> `deposit_data_root` | `string` | A checksum for DepositData fields . | [types.ts:252](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L252) |
| <a id="signature"></a> `signature` | `string` | BLS signature of the deposit message. | [types.ts:255](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/types.ts#L255) |
