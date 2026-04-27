# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the Obol Network GitBook documentation repository containing comprehensive documentation for Distributed Validator Technology (DVT) and the Obol ecosystem. The content is structured around GitBook's markdown-based documentation system.

## Architecture & Structure

### Content Organization
- **`learn/`** - Educational content about DVT concepts, Charon client, and Ethereum staking
- **`run-a-dv/`** - Practical guides for running distributed validators (quickstarts, preparation, operations, cluster editing)
- **`advanced-and-troubleshooting/`** - Advanced configurations, security documentation, and troubleshooting guides
- **`community-and-governance/`** - Governance, token information, and community resources
- **`obol-stack/`** - Obol Stack product documentation (quickstart, installing networks/apps)
- **`api/`** - API documentation for Obol services
- **`sdk/`** - Auto-generated SDK documentation
- **`walkthrough-guides/`** - Step-by-step tutorials

### Duplicate Structure Pattern
The repository contains two parallel directory structures:
- **Long-form paths** (e.g., `run-a-dv/`, `advanced-and-troubleshooting/`) - Main content
- **Short-form paths** (`adv/`, `gov/`, `guides/`, `learn/`, `sdk/`) - Contains `_category_.json` files and `.mdx` versions

### GitBook Configuration
- Uses `_category_.json` files for navigation structure with properties: `label`, `position`, `collapsed`, `collapsible`, `className`
- Supports both `.md` and `.mdx` file formats
- Front matter with `description`, `sidebar_position`, and other metadata
- Image assets stored in `.gitbook/assets/` directory

## Key Concepts & Domain Knowledge

### Core Technologies
- **Distributed Validator Technology (DVT)** - Technology enabling Ethereum validators to run across multiple nodes
- **Charon** - The distributed validator client that enables DVT
- **Distributed Key Generation (DKG)** - Ceremony for creating validator key shares without a trusted dealer
- **Threshold Signatures** - Cryptographic technique allowing subset of operators to sign on behalf of validator

### Operational Patterns
- **Cluster Thresholds** - 4-node clusters require 3/4 nodes online, 7-node clusters require 5/7, etc.
- **Active/Active Redundancy** - Multiple nodes can go offline while validator remains operational
- **Non-custodial Architecture** - Private keys are never reconstructed in full

### Cluster Modification Commands
Documentation lives in `run-a-dv/editing/`. These map to `charon alpha edit` subcommands:
- `add-validators.md` - Generate and add new validators to existing cluster
- `add-operators.md` - Add new operators while keeping validator public keys unchanged
- `remove-operators.md` / `replace-operator.md` - Remove or replace operators
- `recreate-private-keys.md` - Create new private key shares while retaining operator identities
- **Ceremony Coordination** - Most edit commands require threshold or all operators to participate
- **Experimental Status** - These features should not be used in production (Mainnet) yet

### Testing & Validation
- **`charon alpha test` commands** - Comprehensive testing suite for cluster health
  - `charon alpha test beacon` - Test beacon node connectivity and performance
  - `charon alpha test validator` - Test validator client connectivity
  - `charon alpha test peers` - Test peer-to-peer connectivity within cluster
  - `charon alpha test infra` - Test infrastructure (disk, memory, network)
  - Add `--load-test` flag for performance testing

## Content Guidelines

### Documentation Standards
- Use American English throughout all documentation (per `community-and-governance/contribution/docs.md`)
- Use clear, technical language appropriate for both developers and stakers
- Include troubleshooting sections with specific error conditions and resolutions
- Provide hardware/infrastructure requirements where applicable
- Reference external tools and clients with proper links
- Include security warnings for sensitive operations (private keys, production environments)
- Use GitBook-specific syntax (e.g., `{% hint style="warning" %}`, `{% hint style="info" %}`, `{% hint style="danger" %}`)

### Content Types
Two distinct content types used throughout the documentation:
- **Walkthroughs** - How-to guides with concrete numbered steps. Short (2-10 min read), neutral tone. Structure: (1) context, (2) what we're about to do, (3) the steps, (4) summary and next steps.
- **Conceptual articles** - Explain _why_ something exists or works. No steps. Confident and friendly tone. Structure: (1) intro, (2) what it is, (3) why it's essential, (4) related topics, (5) summary.

### Formatting Rules
- **Titles** follow sentence structure — only names and places are capitalized, e.g., `## Distributed key generation` not `## Distributed Key Generation`
- **Bold** (`**text**`) for UI elements the reader interacts with (buttons, fields, window names)
- **Italics** (`_text_`) for names of things (products, documents, concepts)
- **Lists** use `-` for unordered items; each item ends with a period `.`
- **Acronyms** — spell out in full on first use per page, e.g., "Distributed Key Generation (DKG)"
- **Oxford comma** required in lists of three or more items
- Front matter must include `description:` field

### File Organization
- Keep related content together (e.g., all quickstart guides in `run-a-dv/start/`)
- Use descriptive filenames that match content purpose
- Maintain parallel structure between long-form and short-form directories
- Update `SUMMARY.md` table of contents when adding new content

### Cross-References
- Link between related concepts (e.g., DKG ceremony links to cluster configuration)
- Reference specific CLI commands with proper formatting
- Point to external resources (GitHub repos, Discord, API documentation)
- Maintain consistency in terminology across all documentation

## Common Operations

### Content Updates
- Edit markdown files directly in appropriate directory structure
- Update both `.md` and `.mdx` versions if both exist
- Refresh `SUMMARY.md` when adding new pages
- Update version references (e.g., Charon version in CLI reference)

### Link Validation
- Verify internal links use correct relative paths
- Check external links for accuracy (GitHub releases, Discord invites)
- Ensure image references point to correct `.gitbook/assets/` paths

### Technical Accuracy
- Validate CLI command examples against latest Charon releases (run `charon [command] --help` to get current flags and usage)
- Verify threshold mathematics in cluster configuration tables
- Cross-check API documentation with actual API specifications
- Ensure hardware requirements reflect current recommendations
- Update internal documentation links to use relative paths with proper anchor links (e.g., `../../learn/charon/charon-cli-reference.md#section-name`)
