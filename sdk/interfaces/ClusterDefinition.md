[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / ClusterDefinition

Defined in: [types.ts:122](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L122)

Cluster definition data needed for dkg

## Extends

- [`ClusterPayload`](../type-aliases/ClusterPayload.md)

## Properties

| Property | Type | Description | Overrides | Inherited from | Defined in |
| ------ | ------ | ------ | ------ | ------ | ------ |
| <a id="name"></a> `name` | `string` | The cluster name. | - | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`name`](../type-aliases/ClusterPayload.md#name) | [types.ts:98](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L98) |
| <a id="operators"></a> `operators` | [`ClusterOperator`](../type-aliases/ClusterOperator.md)[] | The cluster nodes operators addresses. | - | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`operators`](../type-aliases/ClusterPayload.md#operators) | [types.ts:101](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L101) |
| <a id="validators"></a> `validators` | [`ClusterValidator`](../type-aliases/ClusterValidator.md)[] | The cluster validators information. | - | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`validators`](../type-aliases/ClusterPayload.md#validators) | [types.ts:104](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L104) |
| <a id="deposit_amounts"></a> `deposit_amounts?` | `null` \| `string`[] | The cluster partial deposits in gwei or 32000000000. | - | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`deposit_amounts`](../type-aliases/ClusterPayload.md#deposit_amounts) | [types.ts:107](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L107) |
| <a id="creator"></a> `creator` | [`ClusterCreator`](../type-aliases/ClusterCreator.md) | The creator of the cluster. | - | - | [types.ts:124](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L124) |
| <a id="version"></a> `version` | `string` | The cluster configuration version. | - | - | [types.ts:127](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L127) |
| <a id="dkg_algorithm"></a> `dkg_algorithm` | `string` | The cluster dkg algorithm. | - | - | [types.ts:130](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L130) |
| <a id="fork_version"></a> `fork_version` | `string` | The cluster fork version. | - | - | [types.ts:133](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L133) |
| <a id="uuid"></a> `uuid` | `string` | The cluster uuid. | - | - | [types.ts:136](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L136) |
| <a id="timestamp"></a> `timestamp` | `string` | The cluster creation timestamp. | - | - | [types.ts:139](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L139) |
| <a id="config_hash"></a> `config_hash` | `string` | The cluster configuration hash. | - | - | [types.ts:142](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L142) |
| <a id="threshold"></a> `threshold` | `number` | The distributed validator threshold. | - | - | [types.ts:145](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L145) |
| <a id="num_validators"></a> `num_validators` | `number` | The number of distributed validators in the cluster. | - | - | [types.ts:148](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L148) |
| <a id="definition_hash"></a> `definition_hash?` | `string` | The hash of the cluster definition. | - | - | [types.ts:151](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L151) |
| <a id="consensus_protocol"></a> `consensus_protocol?` | `string` | The consensus protocol e.g qbft. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`consensus_protocol`](../type-aliases/ClusterPayload.md#consensus_protocol) | - | [types.ts:154](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L154) |
| <a id="target_gas_limit"></a> `target_gas_limit?` | `number` | The target gas limit where default is 36M. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`target_gas_limit`](../type-aliases/ClusterPayload.md#target_gas_limit) | - | [types.ts:157](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L157) |
| <a id="compounding"></a> `compounding?` | `boolean` | A withdrawal mechanism with 0x02 withdrawal credentials. | [`ClusterPayload`](../type-aliases/ClusterPayload.md).[`compounding`](../type-aliases/ClusterPayload.md#compounding) | - | [types.ts:160](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L160) |
