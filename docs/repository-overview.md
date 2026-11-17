# Repository overview

## Purpose

Universum Infinitum - Deposit Blockers extends Stellaris planet generation with seven additional deposit blockers and adds one society technology per blocker that unlocks its removal. The stated design goal in `README.md` is to increase variety in planetary blocker composition.

The repository is a **mod**, not an application. It contains no executable code, no build pipeline, and no tests. Its deliverable is a directory of text and image files that the Stellaris engine parses; the only GitHub Actions workflow publishes tagged releases to Nexus Mods.

## What the mod owns

| Owned artefact | File |
|----------------|------|
| Mod metadata (twice, deliberately) | `ui-deposit-blockers.mod`, `ui-deposit-blockers/descriptor.mod` |
| Blocker behaviour and placement weights | `ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt` |
| Removal technologies and their availability weights | `ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt` |
| English display names and descriptions | `ui-deposit-blockers/localisation/english/*.yml` |
| Blocker and technology icons | `ui-deposit-blockers/gfx/interface/icons/deposits/*.dds`, `ui-deposit-blockers/gfx/interface/icons/technologies/*.dds` |
| Store listing imagery | `ui-deposit-blockers/thumbnail.png`, `.github/ui_cover.png` |
| Funding declaration | `.github/FUNDING.yml` |

## What the engine owns

| Owned behaviour | Consequence |
|-----------------|-------------|
| Parsing the script files | A malformed block is rejected silently; the mod cannot report it |
| Planetary blocker selection | The mod supplies weights; the engine rolls them |
| Technology arbitration | The mod supplies weights and modifiers; the engine selects |
| Clearing cost, duration, and effects | The mod declares `resources`, `time`, `planet_modifier`, and `on_cleared`; the engine executes them |
| Save-game persistence | Blocker state survives in the save; the mod performs no I/O |
| Rendering and presentation | Icon paths are resolved by naming convention |

## Key facts

| Fact | Value | Evidence |
|------|-------|----------|
| Blockers defined | 7 | Seven `d_ui_*_blocker` blocks in the deposits file |
| Technologies defined | 7 | Seven `tech_d_ui_*_blocker` blocks in the technology file |
| Society tiers used | 1 and 2 | `tier = 1` (five), `tier = 2` (two) |
| Supported game version | `v4.5.*` | `supported_version` in both descriptors |
| Descriptor tags | `Galaxy Generation`, `Technologies` | `tags` block in both descriptors |
| Mod filesystem path | `mod/ui-deposit-blockers` | `path` in both descriptors |
| Scripted weight baselines | high 10, medium 7, low 5 | Header of the deposits file |
| Shared technology weight group | `deposit_blockers`, decay `0.5` | Every technology block |
| Distribution channels | Steam Workshop, Paradox Mods, Nexus Mods, GitHub Releases | `README.md` badges and `gh release list` |
| Published releases | `v1.0` (2020-04-25), `v1.0.1` (2021-12-15) | Git tags and GitHub releases |
| Continuous integration | Nexus Mods and Steam Workshop publication only | `.github/workflows/nexus-mods-release.yml`, `.github/workflows/steam-workshop-release.yml` |
| Localisation locales | `english` only | `localisation/english/` |

## Scope boundaries

- The mod does **not** add planets, planet classes, districts, or vanilla technology content. It only adds blockers and their removal technologies.
- The mod does **not** patch or override vanilla files. Every definition uses mod-owned identifiers, so load-order conflicts are not expected.
- The mod does **not** implement any network, account, telemetry, or filesystem behaviour. See [`security.md`](security.md) and the root [`PRIVACY.md`](../PRIVACY.md).

## Relationship to other Universum Infinitum mods

The `README.md` lists nine sibling repositories in the same collection. This mod is standalone: no sibling mod is a declared dependency, and no shared script file is consumed at runtime. The only cross-repository coupling is the shared visual identity in the store listings and the three additional distribution channels.

## Where to go next

- Runtime integration: [`architecture.md`](architecture.md)
- Exact fields and values: [`api-reference/INDEX.md`](api-reference/INDEX.md)
- Changing content safely: [`change-guide.md`](change-guide.md)
- Unresolved questions: [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md)
