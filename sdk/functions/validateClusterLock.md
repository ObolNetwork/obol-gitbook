[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / validateClusterLock

> **validateClusterLock**(`lock`): `Promise`\<`boolean`\>

Defined in: [services.ts:13](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/services.ts#L13)

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
