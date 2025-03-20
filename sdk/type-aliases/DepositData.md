[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DepositData

> **DepositData** = `object`

Defined in: [types.ts:240](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L240)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="pubkey"></a> `pubkey` | `string` | The public key of the distributed validator. | [types.ts:242](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L242) |
| <a id="withdrawal_credentials"></a> `withdrawal_credentials` | `string` | The 0x01 withdrawal address of the DV. | [types.ts:245](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L245) |
| <a id="amount"></a> `amount` | `string` | 32 ethers. | [types.ts:248](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L248) |
| <a id="deposit_data_root"></a> `deposit_data_root` | `string` | A checksum for DepositData fields . | [types.ts:251](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L251) |
| <a id="signature"></a> `signature` | `string` | BLS signature of the deposit message. | [types.ts:254](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L254) |
