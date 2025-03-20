[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / TotalSplitPayload

> **TotalSplitPayload** = `object`

Defined in: [types.ts:167](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L167)

Split Proxy Params

## Extended by

- [`RewardsSplitPayload`](../interfaces/RewardsSplitPayload.md)

## Properties

| Property | Type | Description | Defined in |
| ------ | ------ | ------ | ------ |
| <a id="splitrecipients"></a> `splitRecipients` | [`SplitRecipient`](SplitRecipient.md)[] | The split recipients addresses and splits. | [types.ts:169](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L169) |
| <a id="obolrafsplit"></a> `ObolRAFSplit?` | `number` | Split percentageNumber allocated for obol retroactive funding, minimum is 1%. | [types.ts:172](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L172) |
| <a id="distributorfee"></a> `distributorFee?` | `number` | The percentageNumber of accrued rewards that is paid to the caller of the distribution function to compensate them for the gas costs of doing so. Cannot be greater than 10%. For example, 5 represents 5%. | [types.ts:175](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L175) |
| <a id="controlleraddress"></a> `controllerAddress?` | `string` | Address that can mutate the split, should be ZeroAddress for immutable split. | [types.ts:178](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L178) |
