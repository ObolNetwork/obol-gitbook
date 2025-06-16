[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ExitValidationPayload

Defined in: [types.ts:436](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L436)

Represents the overall exit payload structure for exit validation.

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="partial_exits"></a> `partial_exits` | [`ExitValidationBlob`](ExitValidationBlob.md)[] | Array of partial exits for validators. | [types.ts:438](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L438) |
| <a id="share_idx"></a> `share_idx` | `number` | Operator's share index (1-based). | [types.ts:441](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L441) |
| <a id="signature"></a> `signature` | `string` | Signature of the ExitValidationPayload by the operator. | [types.ts:444](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L444) |
