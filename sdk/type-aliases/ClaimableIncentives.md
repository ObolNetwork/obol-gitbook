[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClaimableIncentives

> **ClaimableIncentives** = `object`

Defined in: [types.ts:310](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L310)

Claimable Obol Incentives

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="operator_address"></a> `operator_address` | `string` | Operator Address. | [types.ts:312](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L312) |
| <a id="amount"></a> `amount` | `string` | The amount the recipient is entitled to. | [types.ts:315](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L315) |
| <a id="index"></a> `index` | `number` | The recipient's index in the Merkle tree. | [types.ts:318](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L318) |
| <a id="merkle_proof"></a> `merkle_proof` | `string`[] | The Merkle proof (an array of hashes) generated for the recipient. | [types.ts:321](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L321) |
| <a id="contract_address"></a> `contract_address` | `string` | The MerkleDistributor contract address. | [types.ts:324](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L324) |
