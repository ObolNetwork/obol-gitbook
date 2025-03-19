[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / DepositData

> **DepositData** = `object`

Defined in: [types.ts:254](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L254)

Required deposit data for validator activation

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="pubkey"></a> `pubkey` | `string` | The public key of the distributed validator. | [types.ts:256](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L256) |
| <a id="withdrawal_credentials"></a> `withdrawal_credentials` | `string` | The 0x01 withdrawal address of the DV. | [types.ts:259](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L259) |
| <a id="amount"></a> `amount` | `string` | 32 ethers. | [types.ts:262](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L262) |
| <a id="deposit_data_root"></a> `deposit_data_root` | `string` | A checksum for DepositData fields . | [types.ts:265](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L265) |
| <a id="signature"></a> `signature` | `string` | BLS signature of the deposit message. | [types.ts:268](https://github.com/ObolNetwork/obol-sdk/blob/df036c7bf14d70c2908019882b5bbd9b08a748fb/src/types.ts#L268) |
