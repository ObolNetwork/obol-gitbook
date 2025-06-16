[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / RewardsSplitPayload

Defined in: [types.ts:194](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L194)

OWR and Split Proxy Params

## Extends

- [`TotalSplitPayload`](../type-aliases/TotalSplitPayload.md)

## Properties

| Property | Type | Description | Inherited from | Defined in |
| ------ | ------ | ------ | ------ | ------ |
| <a id="splitrecipients"></a> `splitRecipients` | [`SplitRecipient`](../type-aliases/SplitRecipient.md)[] | The split recipients addresses and splits. | [`TotalSplitPayload`](../type-aliases/TotalSplitPayload.md).[`splitRecipients`](../type-aliases/TotalSplitPayload.md#splitrecipients) | [types.ts:179](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L179) |
| <a id="obolrafsplit"></a> `ObolRAFSplit?` | `number` | Split percentageNumber allocated for obol retroactive funding, minimum is 1%. | [`TotalSplitPayload`](../type-aliases/TotalSplitPayload.md).[`ObolRAFSplit`](../type-aliases/TotalSplitPayload.md#obolrafsplit) | [types.ts:182](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L182) |
| <a id="distributorfee"></a> `distributorFee?` | `number` | The percentageNumber of accrued rewards that is paid to the caller of the distribution function to compensate them for the gas costs of doing so. Cannot be greater than 10%. For example, 5 represents 5%. | [`TotalSplitPayload`](../type-aliases/TotalSplitPayload.md).[`distributorFee`](../type-aliases/TotalSplitPayload.md#distributorfee) | [types.ts:185](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L185) |
| <a id="controlleraddress"></a> `controllerAddress?` | `string` | Address that can mutate the split, should be ZeroAddress for immutable split. | [`TotalSplitPayload`](../type-aliases/TotalSplitPayload.md).[`controllerAddress`](../type-aliases/TotalSplitPayload.md#controlleraddress) | [types.ts:188](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L188) |
| <a id="principalrecipient"></a> `principalRecipient` | `string` | Address that will reclaim validator principal after exit. | - | [types.ts:196](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L196) |
| <a id="etheramount"></a> `etherAmount` | `number` | Amount needed to deploy all validators expected for the OWR/Splitter configuration. | - | [types.ts:199](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L199) |
| <a id="recoveryaddress"></a> `recoveryAddress?` | `string` | Address that can control where the owr erc-20 tokens can be pushed, if set to zero it goes to splitter or principal address. | - | [types.ts:202](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L202) |
