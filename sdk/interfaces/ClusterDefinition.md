[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterDefinition

Defined in: [types.ts:112](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L112)

Cluster definition data needed for dkg

## Extends

- [`ClusterPayload`](../type-aliases/ClusterPayload.md)

## Properties

| Property | Type | Description | Inherited from | Defined in |
| ------ | ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`name`](../type-aliases/ClusterPayload.md#name) | [types.ts:97](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L97) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](../type-aliases/ClusterOperator.md)[] | The cluster nodes operators addresses. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`operators`](../type-aliases/ClusterPayload.md#operators) | [types.ts:100](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L100) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](../type-aliases/ClusterValidator.md)[] | The cluster validators information. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`validators`](../type-aliases/ClusterPayload.md#validators) | [types.ts:103](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L103) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `null` \| `string`[] | The cluster partial deposits in gwei or 32000000000. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`deposit_amounts`](../type-aliases/ClusterPayload.md#deposit_amounts) | [types.ts:106](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L106) |
| <a id="creator"></a> `creator` | [`ClusterCreator`](../type-aliases/ClusterCreator.md) | The creator of the cluster. | - | [types.ts:114](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L114) |
| <a id="version"></a> `version` | `string` | The cluster configuration version. | - | [types.ts:117](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L117) |
| <a id="dkg_algorithm"></a> `dkg_algorithm` | `string` | The cluster dkg algorithm. | - | [types.ts:120](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L120) |
| <a id="fork_version"></a> `fork_version` | `string` | The cluster fork version. | - | [types.ts:123](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L123) |
| <a id="uuid"></a> `uuid` | `string` | The cluster uuid. | - | [types.ts:126](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L126) |
| <a id="timestamp"></a> `timestamp` | `string` | The cluster creation timestamp. | - | [types.ts:129](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L129) |
| <a id="config_hash"></a> `config_hash` | `string` | The cluster configuration hash. | - | [types.ts:132](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L132) |
| <a id="threshold"></a> `threshold` | `number` | The distributed validator threshold. | - | [types.ts:135](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L135) |
| <a id="num_validators"></a> `num_validators` | `number` | The number of distributed validators in the cluster. | - | [types.ts:138](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L138) |
| <a id="definition_hash"></a> `definition_hash?` | `string` | The hash of the cluster definition. | - | [types.ts:141](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L141) |
| <a id="consensus_protocol"></a> `consensus_protocol?` | `string` | The consensus protocol e.g qbft. | - | [types.ts:144](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L144) |
| <a id="target_gas_limit"></a> `target_gas_limit?` | `number` | The target gas limit where default is 30M. | - | [types.ts:147](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L147) |
| <a id="compounding"></a> `compounding?` | `boolean` | A withdrawal mechanism with 0x02 withdrawal credentials. | - | [types.ts:150](https://github.com/ObolNetwork/obol-sdk/blob/719eeaf64437833b733de7c3e76fdb5a3bef243a/src/types.ts#L150) |
