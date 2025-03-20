[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / Incentives

Defined in: [incentives.ts:24](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/incentives.ts#L24)

**`Internal`**

Incentives can be used for fetching and claiming Obol incentives.

 Access it through Client.incentives.

## Example

```ts
const obolClient = new Client(config);
await obolClient.incentives.claimIncentives(address);
```

## Methods

### claimIncentives()

> **claimIncentives**(`address`): `Promise`\<\{ `txHash`: `string`; \} \| \{ `alreadyClaimed`: `true`; \}\>

Defined in: [incentives.ts:61](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/incentives.ts#L61)

Claims obol incentives from a Merkle Distributor contract using just an address.
The method automatically fetches incentives data and checks if already claimed.

#### Parameters

| Parameter | Type | Description |
| ------ | ------ | ------ |
| `address` | `string` | The address to claim incentives for |

#### Returns

`Promise`\<\{ `txHash`: `string`; \} \| \{ `alreadyClaimed`: `true`; \}\>

The transaction hash or already claimed status

#### Remarks

**⚠️ Important:**  If you're storing the private key in an `.env` file, ensure it is securely managed
and not pushed to version control.

#### Throws

Will throw an error if the incentives data is not found or the claim fails

An example of how to use claimIncentives:
[obolClient](https://github.com/ObolNetwork/obol-sdk-examples/blob/main/TS-Example/index.ts#L281)

***

### isClaimed()

> **isClaimed**(`contractAddress`, `index`): `Promise`\<`boolean`\>

Defined in: [incentives.ts:122](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/incentives.ts#L122)

Read isClaimed.

#### Parameters

| Parameter | Type | Description |
| ------ | ------ | ------ |
| `contractAddress` | `string` | Address of the Merkle Distributor Contract |
| `index` | `number` | operator index in merkle tree |

#### Returns

`Promise`\<`boolean`\>

true if incentives are already claime

An example of how to use isClaimed:
[obolClient](https://github.com/ObolNetwork/obol-sdk-examples/blob/main/TS-Example/index.ts#L266)

***

### getIncentivesByAddress()

> **getIncentivesByAddress**(`address`): `Promise`\<`Incentives`\>

Defined in: [incentives.ts:142](https://github.com/ObolNetwork/obol-sdk/blob/920730d3a8bf5554dc69a4ed8703da68e999e989/src/incentives.ts#L142)

#### Parameters

| Parameter | Type | Description |
| ------ | ------ | ------ |
| `address` | `string` | Operator address |

#### Returns

`Promise`\<`Incentives`\>

The matched incentives from DB

#### Throws

On not found if address not found.

An example of how to use getIncentivesByAddress:
[obolClient](https://github.com/ObolNetwork/obol-sdk-examples/blob/main/TS-Example/index.ts#L250)
