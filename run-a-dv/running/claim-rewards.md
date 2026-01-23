---
description: >-
  Learn how to claim rewards from your distributed validator cluster, including the new 3-step process for 0x02 validators and legacy OWR flow.
---

# Claim Rewards

For every epoch, active validators earn ETH rewards from both the consensus layer and the execution layer. The consensus layer rewards are derived from validator duties such as attestation, proposals, and sync committees. These rewards are accumulated in the validator's withdrawal address. Execution rewards, which are earned from MEV and transaction priority tips, are accumulated in the fee recipient address.

---

## New Claim Rewards Process (0x02 Validators)

Whether your cluster is created with OVMs as withdrawal address or EOA as withdrawal address, the process is similar. There are 3 steps:

### 1. Withdraw Rewards / Principal

With the support of compounding, `0x02` type validators stopped supporting automatic withdrawal sweeps to compound rewards. As a result, rewards are not sent directly to the withdrawal address. Instead, the withdrawal address has to send a withdrawal request.

1. You can refer to this documentation on how to withdraw rewards:
   - If the withdrawal address is an **EOA**: See the [Request Withdrawal guide](./request-withdrawal.md) for EOA-specific instructions.
   - If the withdrawal address is an **OVM**: See the [Request Withdrawal guide](./request-withdrawal.md) for OVM-specific instructions.

{% hint style="warning" %}
Be very careful about the amount of ETH you are withdrawing as it will govern whether the amount will be treated as principal or rewards. Read more about this in the [withdrawal request FAQ](./request-withdrawal.md#3-how-to-decide-initial-withdrawal-amount).
{% endhint %}

2. If there are already undistributed rewards, make sure to distribute them before the withdrawal is processed (unless you want to send them to the principal recipient). Because there is a possibility that after withdrawal is processed, the new withdrawal amount and old undistributed amount together add up to cross the principal threshold.

{% hint style="info" %}
💡 In case of `0x01` validators, no withdrawal is required. Withdrawal skimming happens on a regular basis and will be sent to OVM balance. If you are using legacy Obol splits contracts with `0x01` validators, also called as OWRs, then you can jump to the distribute stage (see [Legacy OWR Flow](#legacy-owr-flow-deprecated) below).
{% endhint %}

### 2. Distribute the Rewards

If you are using OVM, distribute the ETH that has been withdrawn from the validators. Upon distribution, it will be sent to either the principal recipient or will be sent as rewards and ready to be claimed on the operator page.

{% hint style="info" %}
💡 The amount of ETH withdrawn in the previous step will be sent to OVM. If the OVM balance > principal threshold, upon distribution the ETH from OVM balance will be sent to the principal address. If OVM balance < principal threshold, it will be sent to the fee recipient splitter. If rewards are sent to the fee recipient splitter, they will be distributed again according to split configuration. But all of these multiple distributions are batched together under a single distribute action.
{% endhint %}

{% hint style="info" %}
💡 For legacy `0x01` validators with OWR withdrawal addresses, distribute works just like before. Click the distribute icon to trigger the distribution.
{% endhint %}

### 3. Claim

Once the funds are distributed, go to the [Operator Dashboard](https://launchpad.obol.org/) to claim the rewards.

---

## Legacy OWR Flow (Deprecated)

{% hint style="warning" %}
This section documents the legacy OWR (Optimistic Withdrawal Recipient) flow for older `0x01` validators. For new clusters with `0x02` validators, please use the [New Claim Rewards Process](#new-claim-rewards-process-0x02-validators) above.
{% endhint %}

The method for claiming rewards depends on the Cluster's withdrawal configuration, whether it's an [**OWR**](../../learn/intro/obol-splits.md#optimistic-withdrawal-recipient) or an [**Exitable Withdrawal Configuration**](../../learn/intro/obol-splits.md#exitable-withdrawal-recipient). The table below outlines the latest details on how and where to claim rewards.

### Claim Status <a href="#claim-status" id="claim-status"></a>

| Withdrawal Configuration                                                | Subcategory Description                                                                   | Is supported on Launchpad? | Where to claim?                                                                                                                                                                                       |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Claim principal + rewards without splits - Exit and get rewards      | To claim principal or rewards without splits, users currently have to exit the validator. | ✅                          | Cluster details page in the Operator Dashboard. For example, [here](https://hoodi.launchpad.obol.org/cluster/details/?lockHash=0x42833298f3c767b866615814dd9f86ce35ed2f89bf3d397d5f353a0ad5a38013). |
| 2. Splits only rewards using OWR - ETH                                  | For all clusters with ETH rewards. Requires two steps: (1) Distribute in cluster details page, (2) Claim at operator page. | ✅                          | Step 1: Cluster details page - Click **Distribute** button for each OWR. Step 2: Operator page - Claim your share. For example, [here](https://hoodi.launchpad.obol.org/cluster/details/?lockHash=0x42833298f3c767b866615814dd9f86ce35ed2f89bf3d397d5f353a0ad5a38013). |
| 3. Split principal + rewards - ETH                                      | For clusters configured to split both principal and rewards.                              | ✅                          | Operators currently need to use the UI provided by Splits.org. For example, a [Lido Split](https://app.splits.org/accounts/0x845aF36663a9908D9E46101e3CC658FbCEB783a8/?chainId=1).                    |
| 4. Splits non-ETH rewards using any withdrawal config - wstETH or weETH | For Lido and EtherFi clusters earning rewards in protocol-specific tokens.                | In Progress ➡️             | Operators currently need to use the UI provided by Splits.org. For example, a [Lido Split](https://app.splits.org/accounts/0x845aF36663a9908D9E46101e3CC658FbCEB783a8/?chainId=1).                    |
| 5. Lido CSM rewards - wstETH                                            | For all Lido CSM clusters earning wstETH rewards.                                         | In Progress ➡️             | Similar to row number 3, use the Splits UI. More details can be found at the bottom of [this page](../integrations/lido-csm.md).                                                  |

### Claim Flow <a href="#claim-flow" id="claim-flow"></a>

To understand how claims via Launchpad work, it is highly recommended to first understand how splits work. More details are available [**here**](../../learn/intro/obol-splits.md). The flowchart below summarizes how an operator and a non-operator can interact with split contracts to facilitate claims:

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

**Updated OWR Process:**

1. All validator rewards are accumulated in the validator's withdrawal address, which is an OWR (Optimistic Withdrawal Recipient) contract.
2. **Distribute in Cluster Details Page**: On the cluster details page, you will see a **Distribute** button for each OWR. Click the Distribute button to send rewards from the OWR to the Split Main contract. This step must be completed before claiming.
3. **Claim at Operator Page**: After distribution is complete, navigate to the operator page to claim your share of the rewards. The rewards will be distributed according to the split configuration set at cluster creation.
4. The Split Main contract sends proportional rewards to each operator based on their configured split percentage.

{% hint style="info" %}
Note: There is no longer a "Claim All" button in the cluster details page. The process now requires two steps: first distribute in the cluster details page, then claim at the operator page.
{% endhint %}

### Launchpad Edge Cases <a href="#launchpad-edge-cases" id="launchpad-edge-cases"></a>

We are constantly improving the user experience. Below are some edge cases to avoid confusion:

#### Case 1: You need to distribute before claiming

Make sure you have clicked the **Distribute** button for each OWR in the cluster details page before attempting to claim rewards at the operator page. If you try to claim before distributing, there may be no rewards available to claim.

#### Case 2: Multiple OWRs in a cluster

If your cluster has multiple OWRs (one per validator), you will see a separate **Distribute** button for each OWR. You need to distribute from each OWR individually before claiming your rewards. After all distributions are complete, you can claim your total share from the operator page.

#### Case 3: You just created a new cluster with no active validators or rewards, but it shows claimable amounts

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

The amount shown here is from a previous case and is now sitting in the Split Main, ready to be claimed. Unfortunately, it is not possible to associate the Split Main balance of an address with a specific cluster. If you are part of multiple clusters, your Split Main balance will appear next to all claim buttons, regardless of the cluster. We are working on a fix to avoid this confusion.
