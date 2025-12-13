# Lido V3 stVault Integration Kit

{% hint style="info" %}
💡

Lido V3 introduces **stVaults**—customizable staking vaults that unlock stETH liquidity for institutional stakers and asset managers. **Obol Distributed Validators offer the most capital-efficient way to deploy a stVault**, unlocking the **highest minting capacity** and the end game staking configuration.
{% endhint %}

## Lido V3 stVault Integration Kit

**Target audience:** Node operators and capital allocators looking to deploy stVaults with **maximum capital efficiency** and institutional-grade security.

{% columns %}
{% column %}
{% hint style="info" %}
⚙️

**I'm a Node Operator**\
Show me how to implement a DV-backed stVault<br>
{% endhint %}
{% endcolumn %}

{% column %}
{% hint style="info" %}
💼

**I'm a Capital Allocator**\
Show me the design and risk story<br>


{% endhint %}
{% endcolumn %}

{% column %}
{% hint style="info" %}
🤝

**I Want Help from Obol**\
Talk to a human or explore Cluster-as-a-Service<br>
{% endhint %}
{% endcolumn %}
{% endcolumns %}

### Who This Kit Is For

**Node operators** who are:

* Being asked to **deploy, and operate** a stVault for a fund, DAO, protocol, or other allocator
* Building that vault on top of **multi-operator Obol DVT** rather than a single operator / single client setup

**Capital allocators** who:

* Need to understand **why** a DVT-backed stVault can support more favorable risk assessments and reserve ratios
* Want a clear set of questions to ask their operators and a sense of what "good" looks like for a DV cluster

{% hint style="info" %}
ℹ️

**Note:** If you're looking for "how do I run a DV cluster with my own ETH," this is the wrong place. This kit is specifically about **Lido V3 stVaults operated on behalf of others**.
{% endhint %}

### Why Obol DVs Are the Best Way to Deploy a stVault

Lido V3's stVault design unlocks new levels of **capital efficiency** for node operators and institutional stakers. Obol Distributed Validators are **the most capital-efficient way to run a vault on Lido V3**.

One of Lido V3's key innovations is the **Reserve Ratio (RR)**, which determines how much ETH must be kept as a reserve buffer relative to minted stETH. Lido is proposing a **2% Reserve Ratio tier for verified multi-operator DVT vaults**, allowing up to **98% stETH minting capacity**. This is the most favorable tier—by comparison, the next-highest default tier for identified operators proposes a 5% RR with only 95% minting capacity.

**Obol Distributed Validators unlock the highest minting capacity available on Lido V3.**

#### Key Benefits

* Unlock the highest capital efficiency\
  Multi-operator DVT vaults qualify for the 2% Reserve Ratio tier, offering 98% stETH minting capacity—the most capital-efficient configuration on Lido V3.
* Distribute responsibility across multiple operators\
  Reduce reliance on any single infrastructure provider, company, or jurisdiction. Multi-operator setups distribute key shares across many entities.
* Maximize client and implementation diversity\
  Run multiple consensus and execution client combinations inside the same vault, strengthening the vaults resilience and reducing correlated risks.
* Superior liveness and fault tolerance\
  Obol's fault-tolerant infrastructure means your validators keep performing even if individual operators experience downtime or failures.
* Enterprise-grade security\
  Leverage best-in-class security that institutional stakers and asset managers require for managing significant stake.

### 📚 How This Kit Is Organized

{% stepper %}
{% step %}
### For Node Operators – Implementation Guide

A practical guide to designing, deploying, and operating a stVault using Obol DVs, including:

* What to collect from the vault owner
* How to design your DV cluster (operators, clients, geos)
* How to rehearse on testnet and prepare for mainnet launch
* How this integrates into the Lido V3 stVault flow

👉 **[Read the Node Operator Guide →](lido-v3-stvault-for-node-operator.md)**
{% endstep %}

{% step %}
### For Capital Allocators – Design & Risk Overview

A higher-level walkthrough of:

* What you are optimizing for (safety, yield, minting capacity, counterparties)
* Why multi-operator DVT is different from "just another node operator"
* How Obol's **Cluster-as-a-Service** offering can help guide your decisions
* What to ask from your operators or from Obol directly
{% endstep %}

{% step %}
### Support & Services for stVault Builders

How to:

* Get in touch with the Obol team for reviews and design help
* Use Obol's **Cluster-as-a-Service** or curated operator networks
* Access additional resources, diagrams, and checklists
{% endstep %}
{% endstepper %}

### 🔗 Quick Links

* [**Default risk assessment framework**](https://research.lido.fi/t/default-risk-assessment-framework-and-fees-parameters-for-lido-v3-stvaults/10504) — Explore how Tiers within Identified Node Operators effect the reserve ratio of the vault
* [**DVT Cluster identification and assessment**](https://docs.lido.fi/run-on-lido/stvaults/node-operators-identification/#dvt-cluster-identification-and-assessment) — See the identification requirements for each node operator in a cluster in order to qualify for improved Tier&#x20;
* [**stVaults Doc Center**](https://docs.lido.fi/run-on-lido/stvaults/) — View Lido's comprehensive guides which detail how to create any product powered by stVaults

### What to Expect Next

This initial version focuses on **DV + stVault design and operations**.

#### Future Extensions

As Lido's [**DeFi wrapper** ](https://hackmd.io/@lido/lido-v3-wrapper-design)and more advanced strategies roll out, we will extend this kit with:

* End-to-end reference architectures that combine stVaults, wrappers, and multi-operator DV clusters
* Config and deployment examples taken from real-world vaults&#x20;
* Case studies, including the "client team vault", once it is live

**For now, this page gives you the map. The linked sections show you how to actually build and run the vaults behind it.**

