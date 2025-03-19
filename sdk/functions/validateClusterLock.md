[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / validateClusterLock

> **validateClusterLock**(`lock`): `Promise`\<`boolean`\>

Defined in: [services.ts:13](https://github.com/ObolNetwork/obol-sdk/blob/e7fc737767265d3063c4e96d045f725fadd20e1e/src/services.ts#L13)

Verifies Cluster Lock's validity.

## Parameters

| Parameter | Type | Description |
| ------ | ------ | ------ |
| `lock` | [`ClusterLock`](../type-aliases/ClusterLock.md) | cluster lock |

## Returns

`Promise`\<`boolean`\>

boolean result to indicate if lock is valid

## Throws

on missing keys or values.

An example of how to use validateClusterLock:
[validateClusterLock](https://github.com/ObolNetwork/obol-sdk-examples/blob/main/TS-Example/index.ts#L127)
