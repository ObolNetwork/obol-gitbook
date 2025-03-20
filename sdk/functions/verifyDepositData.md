[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / verifyDepositData

> **verifyDepositData**(`distributedPublicKey`, `depositData`, `withdrawalAddress`, `forkVersion`, `compounding`?): `object`

Defined in: [verification/common.ts:352](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/verification/common.ts#L352)

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
| `isValidDepositData` | `boolean` | [verification/common.ts:358](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/verification/common.ts#L358) |
| `depositDataMsg` | `Uint8Array` | [verification/common.ts:358](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/verification/common.ts#L358) |
