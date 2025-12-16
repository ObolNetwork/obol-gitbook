# Frequently Asked Questions

## Frequently Asked Questions

### General[​](#general) <a href="#general" id="general"></a>

#### Does Obol have a token?[​](#does-obol-have-a-token) <a href="#does-obol-have-a-token" id="does-obol-have-a-token"></a>

Yes, please see the [token page](../../community-and-governance/obol-token/) for details about the OBOL Token and our [announcement](https://blog.obol.org/airdrop/) for details about the community airdrop that took place in January 2025. The official token contract address is 0x0B010000b7624eb9B3DfBC279673C76E9D29D5F7.

#### Where can I learn more about Distributed Validators?[​](#where-can-i-learn-more-about-distributed-validators) <a href="#where-can-i-learn-more-about-distributed-validators" id="where-can-i-learn-more-about-distributed-validators"></a>

Have you checked out our [blog site](https://blog.obol.tech/) and [twitter](https://twitter.com/ObolNetwork) yet? Maybe join our [discord](https://discord.gg/n6ebKsX46w) too.

#### Where does the name Charon come from?[​](#where-does-the-name-charon-come-from) <a href="#where-does-the-name-charon-come-from" id="where-does-the-name-charon-come-from"></a>

[Charon](https://www.theoi.com/Khthonios/Kharon.html) \[kharon] is the Ancient Greek Ferryman of the Dead. He was tasked with bringing people across the Acheron river to the underworld. His fee was one Obol coin, placed in the mouth of the deceased. This tradition of placing a coin or Obol in the mouth of the deceased continues to this day across the Greek world.

#### What are the hardware requirements for running a Charon node?[​](#what-are-the-hardware-requirements-for-running-a-charon-node) <a href="#what-are-the-hardware-requirements-for-running-a-charon-node" id="what-are-the-hardware-requirements-for-running-a-charon-node"></a>

Charon alone uses negligible disk space of not more than a few MBs. However, if you are running your consensus client and execution client on the same server as Charon, then you will typically need the same hardware as running a full Ethereum node:

{% tabs %}
{% tab title="Minimum" %}
|                        | Charon + VC | Beacon Node |
| ---------------------- | ----------- | ----------- |
| **CPU\***              | 1           | 2           |
| **RAM**                | 2           | 16          |
| **Storage**            | 100 MB      | 2 TB        |
| **Internet Bandwidth** | 10 Mb/s     | 10 Mb/s     |
{% endtab %}

{% tab title="Recommended" %}
|                        | Charon + VC | Beacon Node |
| ---------------------- | ----------- | ----------- |
| **CPU\***              | 2           | 4           |
| **RAM**                | 3           | 24          |
| **Storage**            | 100 MB      | 2 TB        |
| **Internet Bandwidth** | 25 Mb/s     | 25 Mb/s     |
{% endtab %}

{% tab title="High # of Validators (>200)" %}
|                        | Charon + VC | Beacon Node |
| ---------------------- | ----------- | ----------- |
| **CPU\***              | 2           | 8           |
| **RAM**                | 4           | 32          |
| **Storage**            | 100 MB      | 2 TB        |
| **Internet Bandwidth** | 100 Mb/s    | 100 Mb/s    |
{% endtab %}
{% endtabs %}

\*if using vCPU, aim for 2x the above amounts

For more hardware considerations, check out the [ethereum.org guides](https://ethereum.org/en/developers/docs/nodes-and-clients/run-a-node/#environment-and-hardware) which explores various setups and trade-offs, such as running the node locally or in the cloud.

For now, Geth, Teku & Lighthouse clients are packaged within the docker compose file provided in the [quickstart guides](../../run-a-dv/start/quickstart_overview.md), so you don't have to install anything else to run a cluster. Just make sure you give them some time to sync once you start running your node.

#### What is the difference between a node, a validator and a cluster?[​](#what-is-the-difference-between-a-node-a-validator-and-a-cluster) <a href="#what-is-the-difference-between-a-node-a-validator-and-a-cluster" id="what-is-the-difference-between-a-node-a-validator-and-a-cluster"></a>

A node is a single instance of Ethereum EL+CL clients that can communicate with other nodes to maintain the Ethereum blockchain.

A validator is a node that participates in the consensus process by verifying transactions and creating new blocks. Multiple validators can run from the same node.

A cluster is a group of nodes that act together as one or several validators which allows for a more efficient use of resources, reduces operational costs, and provides better reliability and fault tolerance.

#### Can I migrate an existing Charon node to a new machine?[​](#can-i-migrate-an-existing-charon-node-to-a-new-machine) <a href="#can-i-migrate-an-existing-charon-node-to-a-new-machine" id="can-i-migrate-an-existing-charon-node-to-a-new-machine"></a>

It is possible to migrate your Charon node to another machine running the same config by moving the `.charon` folder with its contents to your new machine. Make sure the EL and CL on the new machine are synced before proceeding to the move to minimize downtime.

### Distributed Key Generation[​](#distributed-key-generation) <a href="#distributed-key-generation" id="distributed-key-generation"></a>

#### What are the min and max numbers of operators for a Distributed Validator?[​](#what-are-the-min-and-max-numbers-of-operators-for-a-distributed-validator) <a href="#what-are-the-min-and-max-numbers-of-operators-for-a-distributed-validator" id="what-are-the-min-and-max-numbers-of-operators-for-a-distributed-validator"></a>

Currently, the minimum is 4 operators with a threshold of 3.

The threshold (aka quorum) corresponds to the minimum number of operators that need to be active for the validator(s) to be able to perform its duties. It is defined by the following formula `n-(ceil(n/3)-1)`. We strongly recommend using this default threshold in your DKG as it maximises liveness while maintaining BFT safety. Setting a 4 out of 4 cluster for example, would make your validator more vulnerable to going offline instead of less vulnerable. You can check the recommended threshold values for a cluster [here](key-concepts.md#distributed-validator-threshold).

### Obol Splits[​](#obol-splits) <a href="#obol-splits" id="obol-splits"></a>

#### What are Obol Splits?[​](#what-are-obol-splits) <a href="#what-are-obol-splits" id="what-are-obol-splits"></a>

Obol Splits refers to a collection of composable smart contracts that enable the splitting of validator rewards and/or principal in a non-custodial, trust-minimised manner. Obol Splits contains integrations to enable DVs within Lido, Eigenlayer, and in the future a number of other LSPs.

#### Are Obol Splits non-custodial?[​](#are-obol-splits-non-custodial) <a href="#are-obol-splits-non-custodial" id="are-obol-splits-non-custodial"></a>

Yes. Unless you were to decide to [deploy an editable splitter contract](#can-i-change-the-percentages-in-a-split), Obol Splits are immutable, non-upgradeable, non-custodial, and oracle-free.

#### Can I change the percentages in a split?[​](#can-i-change-the-percentages-in-a-split) <a href="#can-i-change-the-percentages-in-a-split" id="can-i-change-the-percentages-in-a-split"></a>

Generally, Obol Splits are deployed in an immutable fashion, meaning you cannot edit the percentages after deployment. However, if you were to choose to deploy a _controllable_ splitter contract when creating your Split, then yes, the address you select as controller can update the split percentages arbitrarily. A common pattern for this use case is to use a Gnosis SAFE as the controller address for the split, giving a group of entities (usually the operators and principal provider) the ability to update the percentages if need be. A well-known example of this pattern is the [Protocol Guild](https://protocol-guild.readthedocs.io/en/latest/03-onchain-architecture.html).

#### How do Obol Splits work?[​](#how-do-obol-splits-work) <a href="#how-do-obol-splits-work" id="how-do-obol-splits-work"></a>

You can read more about how Obol Splits work [here](obol-splits.md).

#### Are Obol Splits open source?[​](#are-obol-splits-open-source) <a href="#are-obol-splits-open-source" id="are-obol-splits-open-source"></a>

Yes, Obol Splits are licensed under GPLv3 and the source code is available [here](https://github.com/ObolNetwork/obol-splits).

#### Are Obol Splits audited?[​](#are-obol-splits-audited) <a href="#are-obol-splits-audited" id="are-obol-splits-audited"></a>

The Obol Splits contracts have been audited, though further development has continued on the contracts since. Consult the audit results [here](../../advanced-and-troubleshooting/security/smart-contract-audit.md).

#### Are the Obol Splits contracts verified on Etherscan?[​](#are-the-obol-splits-contracts-verified-on-etherscan) <a href="#are-the-obol-splits-contracts-verified-on-etherscan" id="are-the-obol-splits-contracts-verified-on-etherscan"></a>

Yes, you can view the verified contracts on Etherscan. A list of the contract deployments can be found [here](https://github.com/ObolNetwork/obol-splits?#deployment).

#### Does my cold wallet have to call the Obol Splits contracts?[​](#does-my-cold-wallet-have-to-call-the-obol-splits-contracts) <a href="#does-my-cold-wallet-have-to-call-the-obol-splits-contracts" id="does-my-cold-wallet-have-to-call-the-obol-splits-contracts"></a>

No. Any address can trigger the contracts to move the funds, they do not need to be a member of the Split either. You can set your cold wallet/custodian address as the recipient of the principal and rewards, and use any hot wallet to pay the gas fees to push the ether into the recipient address.

#### Are there any edge cases I should be aware of when using Obol Splits?[​](#are-there-any-edge-cases-i-should-be-aware-of-when-using-obol-splits) <a href="#are-there-any-edge-cases-i-should-be-aware-of-when-using-obol-splits" id="are-there-any-edge-cases-i-should-be-aware-of-when-using-obol-splits"></a>

The most important decision is to be aware of whether or not the Split contract you are using has been set up with editability. If a splitter is editable, you should understand what the address that can edit the split does. Is the editor an EOA? Who controls that address? How secure is their seed phrase? Is it a smart contract? What can that contract do? Can the controller contract be upgraded? etc. Generally, the safest thing in Obol's perspective is not to have an editable splitter, and if in the future you are unhappy with the configuration, that you exit the validator and create a fresh cluster with new settings that fit your needs.

Another aspect to be aware of is how the splitting of principal from rewards works using the Optimistic Withdrawal Recipient contract. There are edge cases relating to not calling the contracts periodically or ahead of a withdrawal, activating more validators than the contract was configured for, and a worst-case mass slashing on the network. Consult the documentation on the contract [here](obol-splits.md#optimistic-withdrawal-recipient), its audit [here](../../advanced-and-troubleshooting/security/smart-contract-audit.md), and follow up with the core team if you have further questions.

### Debugging Errors in Logs[​](#debugging-errors-in-logs) <a href="#debugging-errors-in-logs" id="debugging-errors-in-logs"></a>

You can check if the containers on your node are outputting errors by running `docker compose logs` on a machine with a running cluster.

Diagnose some common errors and view their resolutions [here](../../advanced-and-troubleshooting/troubleshooting/errors.md).
