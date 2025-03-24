[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / verifyDepositData

> **verifyDepositData**(`distributedPublicKey`, `depositData`, `withdrawalAddress`, `forkVersion`, `compounding`?): `object`

Defined in: [verification/common.ts:362](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/verification/common.ts#L362)

Verify deposit data withdrawal credintials and signature

## Parameters

| Parameter | Type | Description |
| ------ | ------ | ------ |
| `distributedPublicKey` | `string` | - |
| `depositData` | `Partial`\<[`DepositData`](../type-aliases/DepositData.md)\> | - |
| `withdrawalAddress` | `string` | withdrawal address in definition file. |
| `forkVersion` | `string` | fork version in definition file. |
| `compounding`? | `boolean` | - |

## Returns

`object`

- return if deposit data is valid.

| Name | Type | Defined in |
| ------ | ------ | ------ |
| `isValidDepositData` | `boolean` | [verification/common.ts:368](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/verification/common.ts#L368) |
| `depositDataMsg` | `Uint8Array` | [verification/common.ts:368](https://github.com/ObolNetwork/obol-sdk/blob/02533ab878b3f13dbe6c0029828624f75ecbe185/src/verification/common.ts#L368) |
