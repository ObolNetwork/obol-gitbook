[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / TotalSplitPayload

> **TotalSplitPayload** = `object`

Defined in: [types.ts:177](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L177)

Split Proxy Params

## Extended by

- [`RewardsSplitPayload`](../interfaces/RewardsSplitPayload.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="splitrecipients"></a> `splitRecipients` | [`SplitRecipient`](SplitRecipient.md)[] | The split recipients addresses and splits. | [types.ts:179](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L179) |
| <a id="obolrafsplit"></a> `ObolRAFSplit?` | `number` | Split percentageNumber allocated for obol retroactive funding, minimum is 1%. | [types.ts:182](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L182) |
| <a id="distributorfee"></a> `distributorFee?` | `number` | The percentageNumber of accrued rewards that is paid to the caller of the distribution function to compensate them for the gas costs of doing so. Cannot be greater than 10%. For example, 5 represents 5%. | [types.ts:185](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L185) |
| <a id="controlleraddress"></a> `controllerAddress?` | `string` | Address that can mutate the split, should be ZeroAddress for immutable split. | [types.ts:188](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L188) |
