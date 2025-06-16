[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ExistingExitValidationBlobData

Defined in: [types.ts:450](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L450)

Represents the data structure for an already existing exit blob for exit validation.

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="public_key"></a> `public_key` | `string` | The BLS public key of the validator in hex format | [types.ts:454](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L454) |
| <a id="epoch"></a> `epoch` | `string` | The epoch number when the exit is scheduled to occur | [types.ts:458](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L458) |
| <a id="validator_index"></a> `validator_index` | `string` | The unique index of the validator in the beacon chain | [types.ts:462](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L462) |
| <a id="shares_exit_data"></a> `shares_exit_data` | `Record`\<`string`, \{ `partial_exit_signature`: `string`; \}\>[] | Array of distributed validator shares exit data, where each share contains the partial exit signature from each operator in the cluster | [types.ts:467](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L467) |
