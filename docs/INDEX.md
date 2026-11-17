# Documentation Index

Complete documentation for **Universum Infinitum - Deposit Blockers** — a declarative Stellaris mod that adds seven planet deposit blockers and one society removal technology per blocker.

## Getting Started

| Document | Description |
|----------|-------------|
| [`quick-start.md`](quick-start.md) | Install the mod, enable it, and confirm the blockers appear |
| [`repository-overview.md`](repository-overview.md) | What the mod is, what it owns, and what the engine owns |
| [`repository-structure.md`](repository-structure.md) | File-by-file layout of the repository |
| [`faq.md`](faq.md) | Frequently asked questions |

## Architecture and Design

| Document | Description |
|----------|-------------|
| [`architecture.md`](architecture.md) | Engine integration model, composition, and boundary rules |
| [`design-decisions.md`](design-decisions.md) | Recorded design decisions with evidence and consequences |
| [`data-model.md`](data-model.md) | Complete deposit and technology field reference |
| [`invariants.md`](invariants.md) | Identifier coupling, weight baseline, and descriptor invariants |
| [`dependencies.md`](dependencies.md) | Vanilla script API surface and distribution dependencies |
| [`configuration.md`](configuration.md) | Descriptors, scripted variables, and tuning sources |
| [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) | Tracked unknowns, observed inconsistencies, and unverified engine behaviour |

## Components

| Document | Description |
|----------|-------------|
| [`components/deposit-definitions.md`](components/deposit-definitions.md) | `common/deposits/ui-deposit-blockers.txt` in depth |
| [`components/technology-definitions.md`](components/technology-definitions.md) | `common/technology/ui-deposit-blockers_technologies.txt` in depth |
| [`components/localisation.md`](components/localisation.md) | English localisation tables, encoding, and key conventions |
| [`components/presentation-assets.md`](components/presentation-assets.md) | Deposit and technology icons, thumbnail, and cover image |

## Flows

| Document | Description |
|----------|-------------|
| [`flows/startup-and-loading.md`](flows/startup-and-loading.md) | Launcher registration through script parsing |
| [`flows/planet-generation-and-blocker-placement.md`](flows/planet-generation-and-blocker-placement.md) | `potential` gating and weighted `drop_weight` resolution |
| [`flows/technology-research-and-blocker-clearing.md`](flows/technology-research-and-blocker-clearing.md) | Technology arbitration, clearing cost, and `on_cleared` effects |

## Behaviour

| Document | Description |
|----------|-------------|
| [`behaviour/blocker-appearance.md`](behaviour/blocker-appearance.md) | Which planets receive which blockers, and why |
| [`behaviour/research-and-clearing.md`](behaviour/research-and-clearing.md) | What a player observes when researching and clearing |

## Reference

| Document | Description |
|----------|-------------|
| [`api-reference/INDEX.md`](api-reference/INDEX.md) | Definition schema index and identifier catalogue |
| [`api-reference/deposit-definition-schema.md`](api-reference/deposit-definition-schema.md) | Every deposit field, constraint, and value |
| [`api-reference/technology-definition-schema.md`](api-reference/technology-definition-schema.md) | Every technology field, constraint, and value |
| [`api-usage-examples.md`](api-usage-examples.md) | Complete annotated definitions and tuning examples |

## Operations

| Document | Description |
|----------|-------------|
| [`build-and-deployment.md`](build-and-deployment.md) | Packaging, distribution targets, and release history |
| [`testing.md`](testing.md) | Cross-check scripts, manual verification, and coverage gaps |
| [`change-guide.md`](change-guide.md) | How to add, retune, rename, or remove a blocker safely |
| [`troubleshooting.md`](troubleshooting.md) | Symptoms, causes, and resolutions |
| [`error-handling.md`](error-handling.md) | Parser rejection semantics and detection strategy |
| [`logging.md`](logging.md) | Engine log signals available for diagnosis |
| [`security.md`](security.md) | Threat model, attack surface, and platform controls |
| [`state-and-persistence.md`](state-and-persistence.md) | Engine-owned save-game state and migration risk |
| [`concurrency-and-scheduling.md`](concurrency-and-scheduling.md) | Engine scheduling context and why the mod defines none |
| [`integrations.md`](integrations.md) | Engine, platform, and mod-to-mod integration surface |

## Maintenance

| Document | Description |
|----------|-------------|
| [`documentation-maintenance.md`](documentation-maintenance.md) | Documentation inventory, update triggers, and style rules |

## Root Documents

| Document | Description |
|----------|-------------|
| [`../README.md`](../README.md) | Player-facing description, installation, and compatibility |
| [`../ARCHITECTURE.md`](../ARCHITECTURE.md) | System context, components, flows, and contracts |
| [`../CONTRIBUTING.md`](../CONTRIBUTING.md) | Contribution process and standards |
| [`../SECURITY.md`](../SECURITY.md) | Supported versions, reporting, and disclosure policy |
| [`../CODE_OF_CONDUCT.md`](../CODE_OF_CONDUCT.md) | Conduct standards for project spaces |
| [`../PRIVACY.md`](../PRIVACY.md) | Personal-data handling statement |
| [`../LICENSE`](../LICENSE) | GNU General Public License version 3 |

## Navigation Aids

**Start here (newcomers):** [`quick-start.md`](quick-start.md) → [`repository-overview.md`](repository-overview.md) → [`repository-structure.md`](repository-structure.md).

**Deep dive (component owners):** [`components/deposit-definitions.md`](components/deposit-definitions.md) → [`components/technology-definitions.md`](components/technology-definitions.md) → [`invariants.md`](invariants.md) → [`change-guide.md`](change-guide.md).

**Flows (debuggers):** [`flows/startup-and-loading.md`](flows/startup-and-loading.md) → [`flows/planet-generation-and-blocker-placement.md`](flows/planet-generation-and-blocker-placement.md) → [`flows/technology-research-and-blocker-clearing.md`](flows/technology-research-and-blocker-clearing.md).

**Operations:** [`build-and-deployment.md`](build-and-deployment.md) → [`testing.md`](testing.md) → [`troubleshooting.md`](troubleshooting.md).

## Documentation Conventions

- **"Not applicable" pattern:** Behaviour owned by the Stellaris engine is documented as the engine's responsibility, with the mod-side parameter or absence of participation stated explicitly.
- **Evidence basis:** Every claim is derived from the repository files; engine behaviour that was not observed in play is labelled as a verification gap rather than asserted.
- **Heading case:** Sentence case for all `##` headings under `docs/`.
- **Cross-references:** Relative links from the `docs/` root.

## Maintenance Metadata

| Field | Value |
|-------|-------|
| Last reviewed | 2026-11-17 |
| Owner | Repository maintainer (`hmlendea`) |
| Coverage status | Complete for repository-owned content; engine internals documented as externally owned |
| Validated against | `ARCHITECTURE.md`, `common/deposits/ui-deposit-blockers.txt`, `common/technology/ui-deposit-blockers_technologies.txt`, both localisation files, all icon assets, both descriptors |
