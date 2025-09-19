---
description: >-
  Validator consolidation workflow for rotating operators in an Obol distributed validator cluster.
---

# Operator Rotation

### Introduction to Operator Rotation using Pectra Validator Consolidation

Validator consolidation is a new feature for the Ethereum network, introduced with the last **Pectra** network upgrade. It allows a user to "consolidate" multiple validators into a single, new validator.

- Consolidations can only be performed when the source validator has 0x01 or 0x02 withdrawal credentials and the target validator must be 0x02. The consolidation transaction must be sent from the withdrawal address defined in the source credentials. The target withdrawal credentials can be any address of choice.
- This process transfers the staked ETH from the old validators to the new one while the stake never leaves the beacon chain. The only partial downtime for the source validator is the standard 27-hour waiting period on the beacon chain before the withdrawal. When compared to fully exiting and re-depositing, consolidation avoids the sweep delay required in that option.

{% hint style="info" %} **Future direction (in development):** We’re moving toward a simpler, UI guided operator rotation powered by a new DKG algorithm called Pedersen. When available, operator rotation will run as a coordinated action across the cluster, old and new operators complete a single process together. Instead of changing your live setup, the flow generates a new set of cluster files that you can review before switching over. Operationally, your current cluster can keep running up until you’re ready to cut over. More details will be released soon. {% endhint %}
---

### Guide: Operator Rotation in an Obol Cluster via Validator Consolidation

Validator consolidation enables a safe and efficient way to perform operator rotation in an Obol cluster. This is achieved by transferring staked ETH directly from a **source cluster** (source validators with its original operators) to a **target cluster** (target validators with a new set of operators). Operator rotation can be classified into several different scenarios:

- Source and target cluster validators have the same withdrawal address that is an EOA (wallet address) or a Safe contract.
- Source and target cluster validators have different withdrawal addresses but they are still EOAs.
- Source and target cluster validators have the same OVMs as withdrawal addresses.
- Source and target cluster validators have different OVMs as withdrawal addresses. Source can be an OVM and withdrawal can be an EigenPod/EOA/Safe or vice versa.

In this guide, we focus on source and target validators that have the same withdrawal address that is an EOA or Safe. The other scenarios are actively in development.

**Pros:**

- Compared to the Charon native operator rotation, which requires technical knowledge of node operation, must be executed by operators, and is risky for key security when not done properly, consolidation-based operator rotation can be performed by a withdrawal address (who can be a non-operator such as an ETH allocator).
- Very minimal downtime of ~27 hours (256 epochs) on the stake with source validators. Missed rewards are estimated to be around 0.00296 ETH per validator.

**Cons:**

- If the number of validators is very high, even a single day of downtime (even though small) can add up.
- Target validators need to be active. This requires an additional 32 ETH for each target validator the cluster wishes to set up.

This document outlines the step-by-step process for rotating operators in an Obol DVT cluster using the new validator consolidation feature. This guide assumes you are starting with a source cluster with four existing operators and want to consolidate their validators into a new target cluster with four new operators.

### 1. Prepare the Target Cluster

- **Create a New Cluster:** As the user, first create a new Obol cluster for four new operators of your choice. More details can be found [here](https://docs.obol.org/run-a-dv/start/create-a-dv-with-a-group).
- **Set Withdrawal Address:** Set the withdrawal address for this new cluster to be the same EOA address you used for the source cluster. In future this can be changed to a withdrawal address of your choice.
- **Deploy a New Splitter:** Deploy a new splitter contract dedicated to the new operators of the target cluster.
- **Configure Validators:** Ensure the validators in the new cluster are configured as **compounding validators** with the `0x02` credential type. To enable this make sure to turn the compound toggle on or use the `--compounding` flag if using the CLI directly.

<figure><img src="../../.gitbook/assets/operator-rotation-compounding.png" alt="Compounding validator configuration"><figcaption></figcaption></figure>

- **Run Nodes:** Start the Charon nodes for all operators in the new target cluster. Make sure all the nodes are healthy and ready for deposits. More details [here](https://docs.obol.org/run-a-dv/running/monitoring).
- **Activate Validators:** Activate the target validators by depositing 32 ETH for each. More details [here](https://docs.obol.org/run-a-dv/running/activate-a-dv). The image shows a new operator `0x493...9b1`.

<figure><img src="../../.gitbook/assets/operator-rotation-activate.png" alt="Target validator activation view"><figcaption></figcaption></figure>

### 2. Finalize the Source Cluster

- Have a source cluster ready. Make sure you are connected with the correct withdrawal address. In this case, the operator [`0x28eC4c075DF60535DDE5e2788C34B1961c99474c`](https://hoodi.launchpad.obol.org/operator/0x28eC4c075DF60535DDE5e2788C34B1961c99474c/) is also the withdrawal address.

<figure><img src="../../.gitbook/assets/operator-rotation-source-withdrawal.png" alt="Source cluster withdrawal operator"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/operator-rotation-source-dashboard.png" alt="Source cluster validator list"><figcaption></figcaption></figure>

- **Distribute Rewards:** Before proceeding, distribute all pending rewards from the source cluster's splitter contract to ensure all financial obligations are settled with the original operators. The rewards should be 0 after rewards are distributed and claimed.

<figure><img src="../../.gitbook/assets/operator-rotation-rewards.png" alt="Splitter rewards distribution"><figcaption></figcaption></figure>

### 3. Initiate the Consolidation

- **Access the Migration Tool:** Navigate to the Obol Launchpad migration page by using a URL such as `https://hoodi.launchpad.obol.org/migrate/?withdrawalAddress=your_withdrawal_address`. Alternatively, click the **Migrate** button on a target validator's page within the target cluster dashboard. This **Migrate** button is only clickable for validators where the connected address is the withdrawal address. Make sure the correct address is connected.

<figure><img src="../../.gitbook/assets/operator-rotation-migrate.png" alt="Launchpad migrate action"><figcaption></figcaption></figure>

- **Select Validators:** On the migration page, select the source validators from the original cluster that you wish to consolidate.

INCLUDE IMAGE HERE FOR WITHDRAWAL ADDRESS PAGE AFTER SOURCE VALIDATORS ARE ACTIVATED

- **Confirm and Consolidate:** Click the **Migrate** button to send the consolidation request.

INCLUDE IMAGE OF CONFIRMED MIGRATION

INCLUDE IMAGE HERE FOR WITHDRAWAL ADDRESS PAGE AFTER TARGET VALIDATORS ARE ACTIVATED

<figure><img src="../../.gitbook/assets/operator-rotation-target-withdrawal.png" alt="Target withdrawal address view"><figcaption></figcaption></figure>

### 4. Post-Consolidation Actions

- **Source Validator Exit:** Once the consolidation request is processed by the Ethereum network, the source validators will be set to exit automatically.

SHOW THE EXAMPLE LINKS AND IMAGES HERE

- **Waiting Period:** The validators will enter a waiting period of approximately 27 hours, which is standard for ETH withdrawals and exits.
- **ETH Transfer:** After the waiting period, the staked ETH from the source validators will be automatically consolidated and credited to the target validators in the new cluster.

SHOW IMAGE/LINK OF TARGET HERE

- **Wind Down Source Clusters:** Once the source clusters have fully exited and the ETH has been credited to the new validators, you can safely wind down the source cluster's nodes.

This process ensures a seamless and secure operator rotation, leveraging the efficiency of validator consolidation to minimize downtime and avoid a lengthy manual withdrawal process.
