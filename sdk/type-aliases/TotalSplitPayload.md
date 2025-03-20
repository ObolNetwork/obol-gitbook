[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / TotalSplitPayload

> **TotalSplitPayload** = `object`

Defined in: [types.ts:168](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L168)

Split Proxy Params

## Extended by

- [`RewardsSplitPayload`](../interfaces/RewardsSplitPayload.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="splitrecipients"></a> `splitRecipients` | [`SplitRecipient`](SplitRecipient.md)[] | The split recipients addresses and splits. | [types.ts:170](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L170) |
| <a id="obolrafsplit"></a> `ObolRAFSplit?` | `number` | Split percentageNumber allocated for obol retroactive funding, minimum is 1%. | [types.ts:173](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L173) |
| <a id="distributorfee"></a> `distributorFee?` | `number` | The percentageNumber of accrued rewards that is paid to the caller of the distribution function to compensate them for the gas costs of doing so. Cannot be greater than 10%. For example, 5 represents 5%. | [types.ts:176](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L176) |
| <a id="controlleraddress"></a> `controllerAddress?` | `string` | Address that can mutate the split, should be ZeroAddress for immutable split. | [types.ts:179](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/types.ts#L179) |
