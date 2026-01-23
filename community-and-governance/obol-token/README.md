---
sidebar_position: 4
---

# The OBOL Token

The OBOL Token is central to the governance and operation of the Obol Collective. It serves multiple purposes that are essential to its functioning.

**OBOL Token Transferability:** With the approval of[ OIP-2](https://community.obol.org/t/oip-2-unlock-obol-token/317/30), the OBOL Token is set for a strategic and well-planned unlock. The tokens will be unlocked on the 7th May 2025 at 11:00 UTC, following the completion of centralized exchange listings. Please go to [claim.obol.org](https://claim.obol.org) to unlock airdrop tokens.

## Token Contract

The official token contract address of the OBOL Token is [0x0B010000b7624eb9B3DfBC279673C76E9D29D5F7](https://etherscan.io/address/0x0B010000b7624eb9B3DfBC279673C76E9D29D5F7)

## Official Uniswap Pool

The official Uniswap Pool for the OBOL Token is [https://app.uniswap.org/explore/pools/ethereum/0x57F52C9faa6D40c5163D76b8D7dD81ddB7c95434](https://app.uniswap.org/explore/pools/ethereum/0x57F52C9faa6D40c5163D76b8D7dD81ddB7c95434)

## Verified Contract Addresses <a href="#verified-contract-addresses" id="verified-contract-addresses"></a>

The following smart contracts power OBOL staking, governance, and reward distribution on Ethereum mainnet.

<details>

<summary>Governor Contract</summary>

* **Address:** [`0xcB1622185A0c62A80494bEde05Ba95ef29Fbf85c`](https://etherscan.io/address/0xcB1622185A0c62A80494bEde05Ba95ef29Fbf85c)
* **Purpose:** Manages onchain proposal lifecycle and voting logic for Token House governance.
* **What you can find onchain:**
  * Voting thresholds
  * Quorum settings,
  * Delay/period configs,
  * Proposal and vote history.

</details>

<details>

<summary>Auto Delegate (Overwhelming Support Strategy)</summary>

* **Address:** [`0xCa28852B6Fc15EbD95b17c875D5Eb14b08579158`](https://etherscan.io/address/0xCa28852B6Fc15EbD95b17c875D5Eb14b08579158)
* **Purpose:** Implements the “Overwhelming Support” auto-delegation strategy. This contract casts votes on behalf of un-delegated or transferred stOBOL when proposals receive strong community support. This mechanism uses tokens that would otherwise not be available in governance to ensure uncontroversial proposals will meet quorum.
* **What you can find onchain:**
  * Parameters like `supportThreshold`, `subQuorumBips`, and `votingWindow`
  * Proposals voted on by the strategy
  * Vote power amounts cast
  * Event logs showing execution activity
  * Wallets or protocols interacting with auto-delegation logic

</details>

## Learn more about...

{% content-ref url="token-distribution-and-liquidity.md" %}
[token-distribution-and-liquidity.md](token-distribution-and-liquidity.md)
{% endcontent-ref %}

{% content-ref url="token-holders-faq.md" %}
[token-holders-faq.md](token-holders-faq.md)
{% endcontent-ref %}

{% content-ref url="tge-faq.md" %}
[tge-faq.md](tge-faq.md)
{% endcontent-ref %}
