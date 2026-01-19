---
description: >-
  Learn how to request withdrawals from validators with 0x02 withdrawal credentials using OVM, including batching and principal/rewards distinction.
---

# Request Withdrawal

Post-Pectra update, new kind of validators with `0x02` withdrawal credentials type are supported that can have more than 32 ETH of effective balance. Unlike the validators with `0x01` withdrawal credentials type, which go through periodic skimming of their consensus rewards through a withdrawal sweep, `0x02` rewards are added to the balance to enable auto-compounding. As a result, there is no automatic skimming of rewards. Users have to explicitly send a transaction to request a withdrawal of their rewards to the beacon chain. OVM simplifies requesting withdrawal:

1. By supporting batching of withdrawal requests across multiple validators
2. Helping users distinguish between rewards and principal as long as all deposits are done through OVMs and reward amounts are below principal threshold

The following steps guide through how to request the withdrawal from the validators:

## Step-by-Step Withdrawal Process

1. On the cluster details page, go to the validators table. In the actions column, click on the withdraw icon. It must be noted that the connected address must have `WITHDRAWAL_ROLE` to request withdrawal if the withdrawal address is OVM. **Read more about how to assign roles [here](../../advanced-and-troubleshooting/advanced/assign-ovm-roles.md).** If the withdrawal address is EOA, make sure to be connected with the correct EOA.

<figure><img src="../../.gitbook/assets/OVMWithdrawal1.png" alt=""><figcaption></figcaption></figure>

2. If the user has sufficient permissions, withdrawal address is EOA or OVM and the validator is with `0x02` withdrawal credentials, it will open up the modal to specify total withdrawal amount. The fields here mean the following:
   1. **Validator Balance:** Total balance of all validators under this OVM or EOA
   2. **Withdrawal Limit:** The maximum amount a user can withdraw without triggering an exit. For example, if the validator balance is 100 ETH and 2 validators are active, the withdrawal limit is 100 - (2 × 32) = 36 ETH

{% hint style="info" %}
💡 When deciding the amount to withdraw, users must pay attention to the amount of ETH in OVM's balance, pending withdrawals and OVM's principal threshold. These fields together help decide how much to withdraw to reach principal threshold. Read more about this in the [FAQ section](#3-how-to-decide-initial-withdrawal-amount).
{% endhint %}

<figure><img src="../../.gitbook/assets/OVMWithdrawal2.png" alt=""><figcaption></figcaption></figure>

3. Once an amount is selected, users are recommended a split for where to withdraw the amount from. Read more about the recommendation in the [FAQ section](#2-how-recommendation-for-requesting-withdrawal-works). If you would prefer to allocate manually according to some specific amounts, click on `Allocate Manually`.

<figure><img src="../../.gitbook/assets/OVMWithdrawal3.png" alt=""><figcaption></figcaption></figure>

4. For allocating manually, users must stay under the withdrawal limit. Users can also specify here if they want to exit a specific validator by withdrawing all the amounts.

<figure><img src="../../.gitbook/assets/OVMWithdrawal4.png" alt=""><figcaption></figcaption></figure>

5. In the confirmation stage, review the post-withdrawal balances of validators and if the validators will exit. Upon confirmation, transactions requesting withdrawal will be sent. Once the transaction is accepted, the validator will be in queue waiting for withdrawal sweep. The exact waiting period here depends on the index of the validator on the withdrawal sweep right now and the index of the requesting validator.

## FAQ

### 1. Why did ETH not arrive after I have sent withdrawal request?

If you have successfully sent a transaction to the Ethereum Withdrawal Contract to trigger a partial withdrawal or exit for your **0x02 validator**, but the ETH is not yet in your wallet, it is usually due to one of the following protocol-level steps.

#### 1. Is your request still in the "Partial Withdrawal Queue"?

Unlike older 0x01 validators that are automatically "swept" by the protocol, 0x02 withdrawals are **manually triggered** and enter a First-In-First-Out (FIFO) queue.

- **The Delay:** If many stakers are withdrawing at once, your request must wait its turn.
- **How to check:** Visit a block explorer like [beaconcha.in](https://beaconcha.in/) and look for the **"Partial Withdrawal Queue"** status.

#### 2. Are you in the "27-Hour Cooldown" period?

Every withdrawal request—even after it clears the initial queue—is subject to a mandatory security delay.

- **The Delay:** Approximately **27 hours and 50 minutes** (256 epochs).
- **Why?** This is a safety protocol to prevent rapid "stake-grinding" attacks and ensure network stability. Your ETH will remain on the validator and **continue earning rewards** during this specific window.

#### 3. Did you leave at least 32 ETH in the validator?

For 0x02 validators, you can only withdraw the "excess" balance.

- **The Rule:** You cannot partially withdraw a validator's balance below **32 ETH** while keeping it active. If you requested an amount that would drop your balance below 32 ETH, the protocol may reject the request or only process the amount available above the 32 ETH limit.

#### 4. Is the network experiencing a "Mass Exit" or High Churn?

If you are performing a **Full Exit** (not just a partial withdrawal), you are subject to the **Churn Limit**.

- **The Delay:** Ethereum only allows a certain amount of ETH (currently 256 ETH per epoch) to exit the network at once. During periods of high volatility or institutional exits, this queue can stretch from a few days to **several weeks**.
- **0x01 vs 0x02:** While 0x02 partial withdrawals are usually faster because they skip the "sweep cycle," full exits for both 0x01 and 0x02 validators wait in the same global exit line.

#### 5. Are you checking the right "Arrival" type?

Withdrawals do not arrive as a standard "Transfer" transaction.

- **What to look for:** ETH withdrawals are **"Balance Increases"** provided directly by the protocol. They will appear in the **"Withdrawals"** tab of your address on a block explorer, rather than the "Transactions" or "Internal Txns" tab. Your wallet balance will increase, but you may not see a "Received" notification in some apps.

### 2. How recommendation for requesting withdrawal works?

The recommended approach is to withdraw ETH from the active validator that has the **lowest current ETH balance** until that validator's withdrawal limit is reached.

| **Scenario** | **Example** | **Recommendation** |
| --- | --- | --- |
| **Goal** | Withdraw 10 ETH | Withdraw 10 ETH from the 100 ETH validator. |
| **Validators** | Validator A: 100 ETH | **NOT** the 200 ETH validator. |
|  | Validator B: 200 ETH |  |

While the difference in rewards is usually minimal, this strategy helps to **optimize the effective balance** of your highest-earning validator:

- **Effective Balance:** Validator rewards are calculated based on the "effective balance," which is rounded down to the nearest whole ETH, capped at 32 ETH. The protocol has a **0.5 ETH hysteresis limit** for rounding to the *next* whole integer. For example, a validator must reach **32.5 ETH** to be treated as having a 33 ETH effective balance (capped at 32 ETH).
- **The Goal:** By withdrawing from the validator with **less ETH**, you ensure the higher-balance validator (the **200 ETH** one in your example) keeps as much stake as possible. Since the 200 ETH validator is already earning more rewards, keeping its balance high gives it the best chance to quickly cross the next **0.5 ETH threshold** needed to potentially boost its effective balance (and therefore its rewards) sooner.

### 3. How to decide initial withdrawal amount?

When initiating a withdrawal, the OVM (Obol Validator Manager) uses the **Principal Threshold** (Tp) to determine whether the withdrawn ETH will be classified as **Principal** or **Rewards** upon the next distribution event.

The key calculation users must understand is the **Projected OVM Balance**—the total ETH that will reside in the OVM once all current and pending transactions are complete. The amount you input in the withdrawal field is the only variable you can control to govern this outcome. The UI provides warnings when your input will suggest where the withdrawal will go after it succeeds.

#### Core Withdrawal Principle

The decision is based on a comparison between the **Projected OVM Balance** and the **Principal Threshold** (Tp).

**Projected OVM Balance = B_ovm + W_pending + W_current**

Where:

- **B_ovm:** Current OVM Balance (ETH already in the contract).
- **W_pending:** Pending Withdrawals (ETH from previous successful transactions that are known to be en route to the OVM).
- **W_current:** Currently Withdrawing amount (the value the user is inputting).
- **Tp:** Principal Threshold (a fixed ETH value).

#### The Distribution Rules

The OVM classifies the withdrawal based on the **Projected OVM Balance**:

| **Condition** | **Formula (Plain Text)** | **Outcome (Upon Distribution)** | **Destination** |
| --- | --- | --- | --- |
| **Principal Rule** (Threshold Met/Exceeded) | B_ovm + W_pending + W_current >= Tp | The amount is treated as **Principal**. | Sent to the **Principal Recipient**. |
| **Rewards Rule** (Below Threshold) | B_ovm + W_pending + W_current < Tp | The amount is treated as **Rewards**. | Sent to the **Reward Recipient(s)**. |

#### Example

Assume **Tp = 16 ETH**, **B_ovm = 10 ETH**, and **W_pending = 2 ETH**. The current base is 10 + 2 = 12 ETH.

| **Withdrawal Amount (W_current)** | **Projected OVM Balance** | **Result** | **Destination** |
| --- | --- | --- | --- |
| **4 ETH** (or more) | 10 + 2 + 4 = 16 ETH | 16 ETH >= 16 ETH **(Principal Rule)** | Principal Recipient |
| **3 ETH** (or less) | 10 + 2 + 3 = 15 ETH | 15 ETH < 16 ETH **(Rewards Rule)** | Reward Recipient(s) |

#### Action for Rewards Recipients

If the withdrawal is classified as Rewards:

- **Splitter Contract:** The distribution will trigger a function in the splitter to share the rewards among the configured addresses.
- **EOA (Externally Owned Account):** The amount will be sent directly to the EOA address.
