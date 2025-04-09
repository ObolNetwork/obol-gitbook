# Obol Splits

Obol develops and maintains a suite of smart contracts for use with Distributed Validators and their surrounding ecosystem of decentralised infrastructure. These contracts include:

* Validator Managers: Contracts used for a validator's withdrawal address, enabling ownership transfer, partial withdrawals, full exits, and operator rotation.
* Reward Splitting contracts: Contracts to split ether (and tokens) across multiple entities. Developed by [Splits.org](https://splits.org/)

Key Design Principles the Obol Smart Contract suite include are:

- That they are secure. All [released](https://github.com/ObolNetwork/obol-splits/releases/) Obol Splits products are [audited by high quality security teams](../../advanced-and-troubleshooting/security/overview.md#list-of-security-audits-and-assessments).
- They are not upgradeable.
- They are self-soverign. Any permissioned actions, such as withdrawal, exit, or operator rotation, are controlled by the user, not an unaccountable set of third parties with the ability to upgrade your contracts behaviour.
- They do not require a token to function.
- They are oracle-free. (Unless you intend to leverage a [swapper](https://docs.splits.org/core/swapper)).
- They divide the reward ether from principal ether such that staking providers can be paid a percentage of the _reward_ they accrue for the principal provider rather than a percentage of _principal and reward_.
- That rewards can be withdrawn in an ongoing manner without exiting the validator. (Some conditions apply)

## Obol Validator Managers[​](https://docs.obol.org/learn/intro/obol-splits#withdrawal-recipients)

An Obol Validator Manager (OVM) is a smart contract which manages the deposit, withdrawal, exit, and public key rotation of one or more Ethereum validators. It is deployed as the withdrawal address for a validator and supports 0x01 and 0x02 validator types.

### Creation

You create a new Validator Manager contract using the [factory](#obol-validator-manager-factory-deployment-​httpsdocsobolorglearnintroobol-splitsovm-factory-deployment) by calling the `ObolValidatorManagerFactory.createObolValidatorManager()` function, passing:
- `owner` - The address that is the ultimate administrator of this Validator Manager deployment, it manages the assignment of roles for the contract and **can call all privileged methods**. This address is best suited to being a multi-sig (such as a [SAFE](https://safe.global)) with a large number of signers, used only as a fallback, or it can be owned temporarily, fine grained roles can be assigned to addresses, and then the [`renounceOwnership()`](https://github.com/vectorized/solady/blob/main/src/auth/Ownable.sol#L186) or [`transferOwnership()`](https://github.com/vectorized/solady/blob/main/src/auth/Ownable.sol#L174) methods can be called.
- `principalRecipient` - This is the **address where the principal will be returned** to when validators exit or a withdrawal above the principalThreshold is made. This can be changed later by the `owner` or addresses with the `SET_PRINCIPAL_ROLE`.
- `rewardRecipient` - This is the **address where the accrued ether reward will be sent** when `distributeFunds()` is called. Usually it is a [Pull Split](https://docs.splits.org/core/split-v2#how-it-works) from [splits.org](https://splits.org). This address cannot be updated, instead you can specify an owner of the split, to modify the distribution percentages in future.
- `principalThreshold` - This is a configurable amount of Ether which dictates at what amount of value in the contract should we consider it to be principal being returned rather than reward accrued. A sensible default here is 16 ether, the threshold used in Obol's earlier [Optimistic Withdrawal Recipients](#optimistic-withdrawal-recipient​httpsdocsobolorglearnintroobol-splitsoptimistic-withdrawal-recipient-a-hrefwithdrawal-recipients-idwithdrawal-recipientsa-a-hrefoptimistic-withdrawal-recipient-idoptimistic-withdrawal-recipienta). Further detail in the [FAQ](#faq) section.

### Roles

Obol Validator Managers implement standard Role Based Access Control. The OVM has the following roles, that can be granted by the OVM owner.

- `WITHDRAWAL_ROLE`: Permits an address to trigger a partial withdrawal, or full exit of all validators managed by this contract using [EIP7002](https://eips.ethereum.org/EIPS/eip-7002).
- `CONSOLIDATION_ROLE`: Permits an address to initate a consolidation between one or more source validators and a target validator, all managed by this contract. All source and target validators must be active with a balance greater than 32 ether.
- `SET_PRINCIPAL_ROLE`: Permits an address to change the recipient of the principal returned when validators exit, or a withdrawal above the principalThreshold is initiated.
- `RECOVER_FUNDS_ROLE`: Permits an address to initiate `ERC20.transfer()` calls to arbitrary external addresses, with the intent to recover otherwise stuck tokens.

### Partial Withdrawals & Full Exits

Obol Validator Managers support [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) smart contract based withdrawals. The `owner` address, or any address with the `WITHDRAWAL_ROLE` can call the `requestWithdrawal()` method, to initiate a partial (or full) withdrawal of the balance of a validator managed by this contract.

{% hint style="info" %}
If you request to withdraw an amount that would leave a validator with less than a 32 ETH balance, only the amount that would leave the validator with 32 ETH will be withdrawn.
{% endhint %}

{% hint style="info" %}
If you request to partial withdraw a validators balance, the funds will be available in the OVM contract at the end of the exit queue. (Usually ~27 hours)

However if you withdraw the full balance of the validator, triggering its complete exit, the Ether will be available to to the OVM contract once the validator is through the exit queue, **and** the skimming process has completed. (Average of ~5 Days) This could add a number of days to the wait for validator funds, and full exiting at an optimal moment could significantly shorten the duration.
{% endhint %}

```solidity
function requestWithdrawal(
    bytes[] calldata pubKeys,
    uint64[] calldata amounts
  ) external payable onlyOwnerOrRoles(WITHDRAWAL_ROLE) {}
```

{% code title="Event" overflow="wrap" lineNumbers="false" %}
```solidity
  /// Emitted when a Pectra withdrawal request is done
  /// @param requester Address of the requester
  /// @param pubKey Validator public key
  /// @param amount Withdrawal amount
  event WithdrawalRequested(address indexed requester, bytes indexed pubKey, uint256 amount);
```
{% endcode %}

### Validator Consolidations

Obol Validator Managers support [EIP-7251](https://eips.ethereum.org/EIPS/eip-7251) smart contract based validator consolidations. This is an important feature for rotating the private keys for the validators managed by this contract. The rotation of private keys allows for the secure re-distribution of validation duties amongst new operators, without a significant period of inactivity in a normal exit and recreate flow.

The `owner` address, or any address with the `CONSOLIDATION_ROLE` can call the `requestConsolidation()` method, to initiate a consolidation between one or more source validators and a target validator, all managed by this contract.

{% hint style="info" %}
All source and target validators must be active with a balance greater than 32 ether for the consolidation to succeed. The target validator must be an 0x02 type validator, 0x01 type validators can become 0x02 type through a self consolidation, where the public key is the `source` and `target`.
{% endhint %}

```solidity
  /// Request validators consolidation with the EIP7251 system contract
  /// @dev all source validators will be consolidated into the target validator
  ///      the caller must compute the fee before calling and send a sufficient msg.value amount
  ///      excess amount will be refunded
  /// @param sourcePubKeys validator public keys to be consolidated
  /// @param targetPubKey target validator public key
  function requestConsolidation(
    bytes[] calldata sourcePubKeys,
    bytes calldata targetPubKey
  ) external payable onlyOwnerOrRoles(CONSOLIDATION_ROLE) {}
```

{% code title="Event" overflow="wrap" lineNumbers="false" %}
```solidity
  /// Emitted when a Pectra consolidation request is done
  /// @param requester Address of the requester
  /// @param source Source validator public key
  /// @param target Target validator public key
  event ConsolidationRequested(address indexed requester, bytes indexed source, bytes indexed target);
```
{% endcode %}

### Token Recovery

The `owner` address, or any address with the `RECOVER_FUNDS_ROLE` can call the `recoverFunds()` method, to send an ERC20 token balance on the ObolValidatorManager contract to an arbitrary `recipient` address.

{% hint style="warning" %}
Be cautious when interacting with unknown ERC20 addresses, they may not behave as anticipated.
{% endhint %}

```solidity
  /// Recover non-OVM tokens to a recipient
  /// @param nonOVMToken Token to recover
  /// @param recipient Address to receive recovered token
  function recoverFunds(address nonOVMToken, address recipient) external onlyOwnerOrRoles(RECOVER_FUNDS_ROLE) {}
```

{% code title="Event" overflow="wrap" lineNumbers="false" %}
```
  /// Emitted after tokens are recovered to a recipient
  /// @param nonOVMToken Recovered token (cannot be ETH)
  /// @param recipient Address receiving recovered token
  /// @param amount Amount of recovered token
  event RecoverNonOVMFunds(address indexed nonOVMToken, address indexed recipient, uint256 amount);
```
{% endcode %}

## Optimistic Withdrawal Recipient[​](https://docs.obol.org/learn/intro/obol-splits#optimistic-withdrawal-recipient) <a href="#withdrawal-recipients" id="withdrawal-recipients"></a> <a href="#optimistic-withdrawal-recipient" id="optimistic-withdrawal-recipient"></a>

<figure><img src="../../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption></figcaption></figure>

Optimistic Withdrawal Recipients (OWRs) **are the predecessor to Obol Validator Managers**. The primary addition with Validator Managers is the role-based control over validator withdrawals, exits and consolidations.

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



## Split Contracts[​](https://docs.obol.org/learn/intro/obol-splits#split-contracts) <a href="#split-contracts" id="split-contracts"></a>

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


### OWR Factory Deployment [**​**](https://docs.obol.org/learn/intro/obol-splits#owr-factory-deployment)

The `OptimisticWithdrawalRecipient` contract is deployed via a [factory contract](https://github.com/ObolNetwork/obol-splits/blob/main/src/owr/OptimisticWithdrawalRecipientFactory.sol). The factory is deployed at the following addresses on the following chains.

| Chain   | Address                                                                                                                       |
| ------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Mainnet | [0x119acd7844cbdd5fc09b1c6a4408f490c8f7f522](https://etherscan.io/address/0x119acd7844cbdd5fc09b1c6a4408f490c8f7f522)         |
| Holesky | [0x7fec4add6b5ee2b6c1cba232bc6db754794cb6df](https://holesky.etherscan.io/address/0x7fec4add6b5ee2b6c1cba232bc6db754794cb6df) |
| Sepolia | [0xca78f8fda7ec13ae246e4d4cd38b9ce25a12e64a](https://sepolia.etherscan.io/address/0xca78f8fda7ec13ae246e4d4cd38b9ce25a12e64a) |


## FAQ

### What are Obol Splits?

Obol Splits refers to a collection of composable smart contracts that enable the splitting of validator rewards and/or principal in a non-custodial, trust-minimised manner. Obol Splits contains integrations to enable DVs within Lido with Obol Lido Splits, and Native Staking with Obol Validator Managers.

### Are Obol Splits non-custodial?

Yes. Unless you were to decide to [deploy an editable splitter contract](#can-i-change-the-percentages-in-a-split), Obol Splits are immutable, non-upgradeable, non-custodial, and oracle-free. Obol Validator Managers have owners and Role Based Access Control, but these are self-sovereign and up to the deployer to set and manage. There is no third party with access to or control of your validator unless you grant them such access.

### Obol Validator Managers

#### What happens if I deposit to an OVM managed validator directly with the Ethereum deposit contract instead of through the dedicated OVM.deposit() method? 

In this case, the OVM contract will not have recorded the deposit as principal to be returned, so when the validator exits, it will be sent to the reward address. Consider editing the reward address to pay 100% to the principal recipient, exiting the validator, claiming the 'rewards', and editing the reward split back to normal, before depositing through the OVM for **a new validator private key** that exits to the same OVM contract.

#### What is the principal threshold for?

Determining if Ether returned from a validator is principal deposited or rewards accrued is difficult. Rather than introducing an off-chain proof system, or trusted oracle, Obol Splits adopt an assumption that a mass slashing so severe that the principal returned is less than 16 eth is very rare, and the outcome that would happen in that case is the rewards would be sent to the reward rather than principal address, an accepted risk. This however does impact reward claiming on very large 0x02 validators. A validator could have earned 20 ether in rewards, and if a request for withdrawal of 20 ether is processed, it would be subtracted from principal and disbursed to the principal recipient, and upon a full exit, the remaining eth beyond the principal would be sent to the rewards address. To avoid this, entities with the `WITHDRAWAL_ROLE` should withdraw increments less than the `principalThreshold` if they want it treated as reward, and more than principalThreshold if they want to process it as a direct exit.

### Can I change the percentages in a split?

Generally Obol Splits are deployed in an immutable fashion, meaning you cannot edit the percentages after deployment. However, if you were to choose to deploy a *controllable* splitter contract when creating your Split, then yes, the address you select as controller can update the split percentages arbitrarily. A common pattern for this use case is to use a Gnosis SAFE as the controller address for the split, giving a group of entities (usually the operators and principal provider) the ability to update the percentages if need be. A well known example of this pattern is the [Protocol Guild](https://protocol-guild.readthedocs.io/en/latest/03-onchain-architecture.html#).

### Are Obol Splits open source?

Yes, Obol Splits are licensed under GPLv3 and the source code is available [here](https://github.com/ObolNetwork/obol-splits).

### Are Obol Splits audited?

The Obol Splits contracts have been audited, though further development has continued on the contracts since. Consult the audit results [here](../../advanced-and-troubleshooting/security/overview.md#list-of-security-audits-and-assessments) and always deploy contracts only from published [releases](https://github.com/ObolNetwork/obol-splits/releases).

### Are the Obol Splits contracts verified on Etherscan?

Yes, you can view the verified contracts on Etherscan. A list of the contract deployments can be found [here](https://github.com/ObolNetwork/obol-splits?#deployment).

### Does my cold wallet have to call the Obol Splits contracts?

No. Any address can trigger the contracts to distribute the withdrawn/skimmed ether, they do not need to be a member of the Split either. You can set your cold wallet/custodian address as the recipient of the principal and rewards, and use any hot wallet to pay the gas fees to push the ether into the recipient address.

### Are there any edge cases I should be aware of when using Obol Splits?

The most important decision is to be aware of whether or not the Split contract you are using has been set up with editability
The most important thing to be aware of is what address is the owner of the Obol Validator Manager, whether it has assigned any other addresses any roles, and whether or not the Split contract you are using has been set up with editability and by which address. If a splitter is editable, you should understand what the address that can edit the split does. Is the editor an EOA? Who controls that address? How secure is their seed phrase? Is it a smart contract? What can that contract do? Can the controller contract be upgraded? etc. Generally, the safest thing in Obol's perspective is to use a high threshold multi-sign like a SAFE as the `owner`/`controller`, or to renounce ownership and control entirely, and if in future you are unhappy with the configuration, that you exit the validator and create a fresh cluster with new settings that fit your needs.

Another aspect to be aware of is how the splitting of principal from rewards works using the Optimistic Withdrawal Recipient contract. There are edge cases relating to not calling the contracts periodically or ahead of a withdrawal, activating more validators than the contract was configured for, and a worst case mass slashing on the network. Consult the documentation on the contract [here](../../learn/intro/obol-splits.md#optimistic-withdrawal-recipient), its audit [here](../../adv/security/smart_contract_audit.mdx), and follow up with the core team if you have further questions.
