# Obol Splits

Obol develops and maintains a suite of smart contracts for use with Distributed Validators and their surrounding ecosystem of decentralised infrastructure. These contracts include:

* Validator Managers: Contracts used for a validator's withdrawal address, enabling ownership transfer, partial withdrawals, full exits, and operator rotation.
* Reward Splitting contracts: Contracts to split ether (and tokens) across multiple entities. Developed by [Splits.org](https://splits.org/)

Key Design Principles the Obol Smart Contract suite include are:

- That they are secure. All [released](https://github.com/ObolNetwork/obol-splits/releases/) Obol Splits products are [audited by high quality security teams](../../advanced-and-troubleshooting/security/overview.md#list-of-security-audits-and-assessments).
- They are not upgradeable.
- They are self-soverign. Any permissioned actions, such as withdrawal, exit, or operator rotation, are controlled by the user, not an unaccountable set of third parties with the ability to upgrade your contracts behaviour.
- They do not require a token to function. These contracts do not have a price-feed oracle.
- They are oracle-free. (Unless you intend to leverage a [swapper](https://docs.splits.org/core/swapper)).
- They divide the reward ether from principal ether such that staking providers can be paid a percentage of the _reward_ they accrue for the principal provider rather than a percentage of _principal and reward_.
- That rewards can be withdrawn in an ongoing manner without exiting the validator.

### Validator Managers[​](https://docs.obol.org/learn/intro/obol-splits#withdrawal-recipients)

An Obol Validator Manager is a smart contract which manages the deposit, withdrawal, exit, and public key rotation of one or more Ethereum validators.


#### Obol Validator Managers

An Obol Validator Manager (OVM) is a smart contract that serves as the withdrawal address for your validator. It supports 0x01 and 0x02 validator types.

##### Creation

You create a new Validator Manager contract using the [factory](#obol-validator-manager-factory-deployment-​httpsdocsobolorglearnintroobol-splitsovm-factory-deployment) by calling the `ObolValidatorManagerFactory.createObolValidatorManager()` function, passing:
- `owner` - The address that is the ultimate administrator of this Validator Manager deployment, it manages the assignment of roles for the contract and can call all priviledged methods. This address is best suited to being a multi-sig with a large number of signers, used only as a fallback, or it can be owned temporarily, fine grained roles can be assigned to addresses, and the [`renounceOwnership()`](https://github.com/vectorized/solady/blob/main/src/auth/Ownable.sol#L186) can be called.
- `principalRecipient` - This is the address where the principal will be returned to when validators exit or a withdrawal above the principalThreshold is made. This can be changed later by the `owner` or addresses with the `SET_PRINCIPAL_ROLE`.
- `rewardRecipient` - This is the address where the accrued ether reward will be sent when `distributeFunds()` is called. Usually it is a [Pull Split](https://docs.splits.org/core/split-v2#how-it-works) from [splits.org](https://splits.org). This address cannot be updated, instead you can specify an owner of the split, to modify the distribution percentages in future.
- `principalThreshold` - This is a configurable amount of Ether which dictates at what amount of value in the contract should we consider it to be principal being returned rather than reward accrued. A sensible default here is 16 ether, the threshold used in Obol's earlier [Optimistic Withdrawal Recipients](#optimistic-withdrawal-recipient​httpsdocsobolorglearnintroobol-splitsoptimistic-withdrawal-recipient-a-hrefwithdrawal-recipients-idwithdrawal-recipientsa-a-hrefoptimistic-withdrawal-recipient-idoptimistic-withdrawal-recipienta). Further detail in the [considerations](#considerations) section.

##### Roles

Obol Validator Managers implement standard Role Based Access Control. The OVM has the following roles, that can be granted by the OVM owner.

- `WITHDRAWAL_ROLE`: Permits an address to trigger a partial withdrawal, or full exit of all validators managed by this contract using [EIP7002](https://eips.ethereum.org/EIPS/eip-7002).
- `CONSOLIDATION_ROLE`: Permits an address to initate a consolidation between one or more source validators and a target validator, all managed by this contract. All source and target validators must be active with a balance greater than 32 ether.
- `SET_PRINCIPAL_ROLE`: Permits an address to change the recipient of the principal returned when validators exit, or a withdrawal above the principalThreshold is initiated.
- `RECOVER_FUNDS_ROLE`: Permits an address to initiate `ERC20.transfer()` calls to arbitrary external addresses, with the intent to recover otherwise stuck tokens.

##### Partial Withdrawals


##### Full Exits


##### Validator Consolidations


##### Token Recovery

The `owner` address, or any address with the `RECOVER_FUNDS_ROLE` can call the `recoverFunds()` method, to send an ERC20 token balance on the ObolValidatorManager contract to an arbitrary `recipient` address.


{% hint style="warning" %}
Be cautious when interacting with unknown
{% endhint %}

```solidity
function recoverFunds(address nonOVMToken, address recipient) external onlyOwnerOrRoles(RECOVER_FUNDS_ROLE) {}
```

{% code title="Event" overflow="wrap" lineNumbers="false" %}
```
event RecoverNonOVMFunds(address indexed nonOVMToken, address indexed recipient, uint256 amount);
```
{% endcode %}

##### Considerations

(Talk about edge cases like the threshold, activating a validator not through the deposit method here, unforeseen slashings above principal threshold, )

#### Optimistic Withdrawal Recipient[​](https://docs.obol.org/learn/intro/obol-splits#optimistic-withdrawal-recipient) <a href="#withdrawal-recipients" id="withdrawal-recipients"></a> <a href="#optimistic-withdrawal-recipient" id="optimistic-withdrawal-recipient"></a>

<figure><img src="../../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption></figcaption></figure>

Optimistic Withdrawal Recipients (OWRs) are the prior version of Obol Validator Managers. The primary addition with Validator Managers is the role-based control over validator withdrawals, exits and consolidations.

Optimistic Withdrawal Recipients allow for the separation of reward from principal, as well as permitting the ongoing withdrawal of accruing rewards.

An Optimistic Withdrawal Recipient [contract](https://github.com/ObolNetwork/obol-splits/blob/main/src/owr/OptimisticWithdrawalRecipient.sol) takes three inputs when deployed:

* A _principal_ address: The address that controls where the principal ether will be transferred post-exit.
* A _reward_ address: The address where the accruing reward ether is transferred to.
* The amount of ether that makes up the principal.

This contract **assumes that any ether that has appeared in its address since it was last able to do balance accounting is skimming reward from an ongoing validator** (or number of validators) unless the change is > 16 ether. This means balance skimming is immediately claimable as reward, while an inflow of e.g. 31 ether is tracked as a return of principal (despite being slashed in this example).

{% hint style="danger" %}
Worst-case mass slashings can theoretically exceed 16 ether, if this were to occur, the returned principal would be misclassified as a reward, and distributed to the wrong address. This risk is the drawback that makes this contract variant 'optimistic'. If you intend to use this contract type, **it is important you fully understand and accept this risk**.

The alternative is to use an splits.org [waterfall contract](https://docs.splits.org/core/waterfall), which won't allow the claiming of rewards until all principal ether has been returned, meaning validators need to be exited for operators to claim their CL rewards.
{% endhint %}

This contract fits both design goals and can be used with thousands of validators. It is safe to deploy an Optimistic Withdrawal Recipient with a principal higher than you actually end up using, though you should process the accrued rewards before exiting a validator or the reward recipients will be short-changed as that balance may be counted as principal instead of reward the next time the contract is updated. If you activate more validators than you specified in your contract deployment, you will record too much ether as reward and will overpay your reward address with ether that was principal ether, not earned ether. Current iterations of this contract are not designed for editing the amount of principal set.



### Split Contracts[​](https://docs.obol.org/learn/intro/obol-splits#split-contracts) <a href="#split-contracts" id="split-contracts"></a>

Validators have two streams of revenue, the consensus layer rewards and the execution layer rewards. Validator Managers focus on the former, split contracts focus on the latter. They are best used in tandem.

<figure><img src="../../.gitbook/assets/ovm_splits_overview.png" alt="Obol Validator Manager in Tandem with an Execution Layer Fee recipient splitter contract"><figcaption></figcaption></figure>

A split, or splitter, is a set of contracts that can divide ether or an ERC20 across a number of addresses. Splits are often used in conjunction with withdrawal recipients. Execution Layer rewards for a DV are directed to a split address through the use of a `fee recipient` address. Splits can be either immutable, or mutable by way of an admin address capable of updating them.

Further information about splits can be found on the splits.org team's [docs site](https://docs.splits.org/). The addresses of their deployments can be found [here](https://docs.splits.org/core/split#addresses).

### Split Controllers[​](https://docs.obol.org/learn/intro/obol-splits#split-controllers) <a href="#split-controllers" id="split-controllers"></a>

Splits can be completely edited through the use of the `controller` address, however, total editability of a split is not always wanted. We recommend using a [SAFE wallet](https://safe.global) to manage the Split.

#### (Gnosis) SAFE wallet[​](https://docs.obol.org/learn/intro/obol-splits#gnosis-safe-wallet) <a href="#gnosis-safe-wallet" id="gnosis-safe-wallet"></a>

A [SAFE](https://safe.global/) is a common method to administrate an editable split. The most well-known deployment of this pattern is the [protocol guild](https://protocol-guild.readthedocs.io/en/latest/3-smart-contract.html). The SAFE can arbitrarily update the split to any set of addresses with any valid set of percentages.


## Deployments

### Obol Validator Manager Factory Deployment [**​**](https://docs.obol.org/learn/intro/obol-splits#ovm-factory-deployment)

The `ObolValidatorManager` contract is deployed via a [factory contract](https://github.com/ObolNetwork/obol-splits/blob/main/src/ovm/ObolValidatorManagerFactory.sol). The factory is deployed at the following addresses on the following chains.

| Chain   | Address                                                                                                                       |
| ------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Mainnet | [](https://etherscan.io/address/)         |
| Hoodi  | [0xb1E1f5e90f4190F78182A8d5cbed774893Dd1558](https://hoodi.etherscan.io/address/0xb1E1f5e90f4190F78182A8d5cbed774893Dd1558)  |
| Holesky | [](https://holesky.etherscan.io/address/) |
| Sepolia | [](https://sepolia.etherscan.io/address/) |

### Obol Lido Split Factory Deployment [**​**](https://docs.obol.org/learn/intro/obol-splits#ols-factory-deployment)

The `ObolLidoSplit` contract is deployed via a [factory contract](https://github.com/ObolNetwork/obol-splits/blob/main/src/lido/ObolLidoSplitFactory.sol). The factory is deployed at the following addresses on the following chains.

| Chain   | Address                                                                                                                       |
| ------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Mainnet | [](https://etherscan.io/address/)         |
| Hoodi  | [0xb633CD420aF83E8A5172e299104842b63dd97ab7](https://hoodi.etherscan.io/address/0xb633CD420aF83E8A5172e299104842b63dd97ab7)  |
| Holesky | [](https://holesky.etherscan.io/address/) |
| Sepolia | [](https://sepolia.etherscan.io/address/) |


**OWR Factory Deployment**[**​**](https://docs.obol.org/learn/intro/obol-splits#owr-factory-deployment)

The `OptimisticWithdrawalRecipient` contract is deployed via a [factory contract](https://github.com/ObolNetwork/obol-splits/blob/main/src/owr/OptimisticWithdrawalRecipientFactory.sol). The factory is deployed at the following addresses on the following chains.

| Chain   | Address                                                                                                                       |
| ------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Mainnet | [0x119acd7844cbdd5fc09b1c6a4408f490c8f7f522](https://etherscan.io/address/0x119acd7844cbdd5fc09b1c6a4408f490c8f7f522)         |
| Holesky | [0x7fec4add6b5ee2b6c1cba232bc6db754794cb6df](https://holesky.etherscan.io/address/0x7fec4add6b5ee2b6c1cba232bc6db754794cb6df) |
| Sepolia | [0xca78f8fda7ec13ae246e4d4cd38b9ce25a12e64a](https://sepolia.etherscan.io/address/0xca78f8fda7ec13ae246e4d4cd38b9ce25a12e64a) |


## FAQ

