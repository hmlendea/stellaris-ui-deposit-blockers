# Documentation maintenance

## Purpose

This document records what exists under `docs/`, when each document must change, and the conventions that keep the tree navigable for future contributors and agents.

## Inventory

| Document | Answers | Change trigger |
|----------|---------|----------------|
| [`INDEX.md`](INDEX.md) | Where do I start? | Any document added, renamed, or removed |
| [`repository-overview.md`](repository-overview.md) | What is this mod and what does it own? | New capability, new distribution channel, changed ownership boundary |
| [`repository-structure.md`](repository-structure.md) | Where is everything? | New directory, new file category, renamed content file |
| [`quick-start.md`](quick-start.md) | How do I install and confirm it works? | Changed installation route, changed launcher behaviour |
| [`architecture.md`](architecture.md) | How does the engine consume this mod? | New engine subsystem used, changed composition |
| [`design-decisions.md`](design-decisions.md) | Why is it built this way? | A consequential decision is made or reversed |
| [`data-model.md`](data-model.md) | What fields exist and what do they mean? | New field, changed field semantics |
| [`invariants.md`](invariants.md) | What must stay true? | New coupling, changed baseline, changed contract |
| [`dependencies.md`](dependencies.md) | What does it rely on? | New vanilla identifier, vanilla rename, new channel |
| [`configuration.md`](configuration.md) | What is tunable and where? | New tunable, changed baseline, new tier |
| [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) | What is unresolved? | An item is resolved or a new one is discovered |
| [`components/deposit-definitions.md`](components/deposit-definitions.md) | How does the deposits file work? | Any change to the deposits file |
| [`components/technology-definitions.md`](components/technology-definitions.md) | How does the technology file work? | Any change to the technology file |
| [`components/localisation.md`](components/localisation.md) | How do display strings work? | New key, renamed identifier, new locale |
| [`components/presentation-assets.md`](components/presentation-assets.md) | How are icons resolved? | New asset, changed geometry or format |
| [`flows/startup-and-loading.md`](flows/startup-and-loading.md) | How does the mod load? | Changed descriptor, changed directory layout |
| [`flows/planet-generation-and-blocker-placement.md`](flows/planet-generation-and-blocker-placement.md) | How are blockers placed? | Changed `potential`, changed weight logic |
| [`flows/technology-research-and-blocker-clearing.md`](flows/technology-research-and-blocker-clearing.md) | How are technologies offered and blockers cleared? | Changed weights, costs, or `on_cleared` |
| [`behaviour/blocker-appearance.md`](behaviour/blocker-appearance.md) | What does a player observe about placement? | Changed eligibility or weights |
| [`behaviour/research-and-clearing.md`](behaviour/research-and-clearing.md) | What does a player observe about research and clearing? | Changed cost, duration, tier, or effect |
| [`api-reference/INDEX.md`](api-reference/INDEX.md) | What is the definition schema map? | New schema, new identifier, changed line numbers |
| [`api-reference/deposit-definition-schema.md`](api-reference/deposit-definition-schema.md) | What fields does a deposit accept? | New field or changed constraints |
| [`api-reference/technology-definition-schema.md`](api-reference/technology-definition-schema.md) | What fields does a technology accept? | New field or changed constraints |
| [`api-usage-examples.md`](api-usage-examples.md) | What do complete declarations look like? | A representative example changes |
| [`build-and-deployment.md`](build-and-deployment.md) | How is it released? | New channel, new release process, new release tag |
| [`testing.md`](testing.md) | How is it verified? | New check added or an existing check changes |
| [`change-guide.md`](change-guide.md) | How do I edit safely? | New edit class, new invariant, new coupling surface |
| [`troubleshooting.md`](troubleshooting.md) | What does this symptom mean? | A new recurring problem is identified |
| [`error-handling.md`](error-handling.md) | What happens when something fails? | Changed failure semantics |
| [`logging.md`](logging.md) | What diagnostics exist? | Engine log behaviour changes |
| [`security.md`](security.md) | What is the threat model? | New attack surface or mitigation |
| [`state-and-persistence.md`](state-and-persistence.md) | What state exists and who owns it? | Changed persistence behaviour |
| [`concurrency-and-scheduling.md`](concurrency-and-scheduling.md) | What runs concurrently? | Engine scheduling behaviour changes |
| [`integrations.md`](integrations.md) | What does it integrate with? | New integration, changed contract |
| [`faq.md`](faq.md) | What do people ask? | A question recurs |
| [`documentation-maintenance.md`](documentation-maintenance.md) | How is this tree maintained? | Any document added or removed |

## Update triggers by change type

| Change to content | Documents to revise |
|-------------------|---------------------|
| Add a blocker and technology | `repository-overview.md`, `data-model.md`, `configuration.md`, `api-reference/INDEX.md`, `behaviour/blocker-appearance.md`, `behaviour/research-and-clearing.md`, both component documents, `api-usage-examples.md` |
| Rename an identifier | `invariants.md`, `components/localisation.md`, `components/presentation-assets.md`, `api-reference/INDEX.md`, `design-decisions.md` |
| Retune a baseline | `configuration.md`, `design-decisions.md`, `behaviour/blocker-appearance.md`, `flows/planet-generation-and-blocker-placement.md` |
| Change a tier | `configuration.md`, `behaviour/research-and-clearing.md`, `components/technology-definitions.md` |
| Change the descriptor | `repository-overview.md`, `configuration.md`, `build-and-deployment.md`, `flows/startup-and-loading.md` |
| Change the release process or channels | `build-and-deployment.md`, `dependencies.md`, `integrations.md` |
| Resolve an open question | `ambiguities-and-open-questions.md`, plus the document that states the unresolved behaviour |
| Discover a new coupling or failure mode | `invariants.md`, `testing.md`, `troubleshooting.md` |

## Style conventions

| Convention | Rule |
|------------|------|
| Heading case | Sentence case for `##` and below |
| File names | kebab-case |
| Code symbols | Backticks |
| Repository paths | Backticks with the repository-relative path |
| Cross-references | Relative links from the `docs/` root |
| Root documents | Referenced as `../README.md`, `../ARCHITECTURE.md`, and similar |
| Tables | Used for catalogues and contracts |
| Diagrams | Mermaid for topology and sequence; never ASCII art |
| Claims | Every claim traceable to a repository file; engine behaviour that was not observed is labelled as unverified |
| Emojis | Not used in `docs/` headings |

## Accuracy rules

1. **Do not assert engine behaviour that was not observed.** Record it in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) instead.
2. **Do not restate repository facts imprecisely.** Prefer exact identifiers, exact values, and exact counts over approximations.
3. **Distinguish intent from inference.** Where a design reason cannot be established from evidence, say so.
4. **Keep counts synchronised.** If the number of blockers changes, every document that states a count must change with it.
5. **Do not duplicate the root documents.** `docs/architecture.md` complements the root `ARCHITECTURE.md`; `docs/security.md` complements the root `SECURITY.md`; the root [`PRIVACY.md`](../PRIVACY.md) is not restated.

## Ownership

| Field | Value |
|-------|-------|
| Owner | Repository maintainer (`hmlendea`) |
| Last reviewed | 2026-11-17 |
| Review trigger | Any content or descriptor change, or any Stellaris version change |
| Coverage | Complete for repository-owned content; engine internals documented as externally owned |

## Where to go next

- Master index: [`INDEX.md`](INDEX.md)
- Unresolved items: [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md)
