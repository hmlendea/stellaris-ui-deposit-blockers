# Invariants

Invariants are rules that must remain true for the mod to load correctly and behave as intended. Each entry states the rule, its evidence, how to check it, and what breaks when it is violated.

## Category 1: Descriptor invariants

| # | Invariant | Evidence | Check | Violation effect |
|---|-----------|----------|-------|------------------|
| I1 | `ui-deposit-blockers.mod` and `ui-deposit-blockers/descriptor.mod` are byte-identical | `diff` returns no output | `diff ui-deposit-blockers.mod ui-deposit-blockers/descriptor.mod` | Distribution channels read different metadata; version drift between install routes |
| I2 | Both descriptors declare `path="mod/ui-deposit-blockers"` | `path` key in both | Inspect key in both files | Installer cannot locate the content directory |
| I3 | Both descriptors declare `supported_version="v4.5.*"` | `supported_version` key in both | Inspect key in both files | Launcher may refuse to load the mod |
| I4 | Both descriptors declare the same `tags` set | `tags` block in both | Inspect block in both files | Store categorisation drift between channels |
| I5 | `name` is `Universum Infinitum - Deposit Blockers` | `name` key in both | Inspect key in both files | Inconsistent mod-list label |

## Category 2: Identifier coupling invariants

| # | Invariant | Evidence | Check | Violation effect |
|---|-----------|----------|-------|------------------|
| I6 | Every `tech_d_ui_*_blocker` is referenced by exactly one deposit's `prerequisites` | Seven deposits, seven technologies, one-to-one | [Link cross-check](testing.md) | An orphan technology never becomes useful; an unlinked deposit cannot be cleared |
| I7 | Every `d_ui_*_blocker` has a matching `tech_d_ui_*_blocker` | Same `<name>` token in both files | Compare identifier token sets | Blocker cannot be cleared, or technology unlocks nothing |
| I8 | Every identifier has a name key and a `_desc` key in the corresponding localisation file | 7 + 7 keys per file | Key cross-check | Raw identifier shown in game |
| I9 | Every identifier has a `.dds` file with exactly that name | 7 deposit icons, 7 technology icons | [Icon cross-check](testing.md) | Blank or placeholder icon |
| I10 | The `<name>` token is identical in the deposit identifier, technology identifier, both localisation files, and both icon file names | Observed in all seven sets | Set comparison | Partial coupling break — one surface disagrees with the others |

## Category 3: Deposit definition invariants

| # | Invariant | Evidence | Check | Violation effect |
|---|-----------|----------|-------|------------------|
| I11 | Every deposit declares `is_for_colonizable = yes` | All seven | `grep -c 'is_for_colonizable = yes'` | Blocker cannot appear on colonisable planets as intended |
| I12 | Every deposit declares `category = deposit_cat_blockers` | All seven | `grep -c 'category = deposit_cat_blockers'` | Deposit is not treated as a blocker |
| I13 | Every deposit declares `prerequisites` naming a mod technology | All seven | Prerequisite cross-check | Clearing gate unresolved |
| I14 | Every deposit declares a `drop_weight` with a `weight` referring to a declared baseline | All seven | Inspect `weight` values | Weight resolves to zero or an undefined variable |
| I15 | The `num_free_districts` guard (`value < 2` → `factor = 0`) is present in every deposit | Seven occurrences | `grep -c num_free_districts` | Blockers could render a planet effectively uncolonisable |
| I16 | `planet_max_districts_add` is `-1` or `-2` | Five `-1`, two `-2` | Inspect values | Unbounded district loss |
| I17 | Clearing cost is `energy` (plus optional `minerals`) with `energy >= 500` | Energy 500/750/1000; minerals 250 on one | Inspect `resources.cost` | Cost semantics diverge across the family |

## Category 4: Technology definition invariants

| # | Invariant | Evidence | Check | Violation effect |
|---|-----------|----------|-------|------------------|
| I18 | Every technology declares `area = society` | All seven | `grep -c 'area = society'` | Technologies scatter across research areas |
| I19 | Every technology declares `category = { new_worlds }` | All seven | `grep -c 'new_worlds'` | Technologies appear in an unrelated tree category |
| I20 | Every technology declares `prerequisites = { "tech_colonization_1" }` | All seven | `grep -c tech_colonization_1` | Gate changes silently, altering progression |
| I21 | Tier 1 technologies use `@tier1cost2` and `@tier1weight2`; tier 2 use `@tier2cost2` and `@tier2weight2` | Five tier 1, two tier 2 | Compare per-block pairs | Cost/weight mismatch against the declared tier |
| I22 | Every technology declares the `deposit_blockers` weight group with decay `0.5` | Seven occurrences | `grep -c 'deposit_blockers = 0.5'` | One technology gains disproportionate availability |
| I23 | Every technology declares `potential = { is_nomadic = no }` | All seven | `grep -c is_nomadic` | Nomadic empires receive unusable technologies |
| I24 | The council-trait modifier uses `leader_trait_expertise_new_worlds` | Fourteen occurrences (7 in `weight_modifier`, 7 in `ai_weight`) | `grep -c leader_trait_expertise_new_worlds` | Multiplier silently lost |
| I25 | The `num_owned_planets < 2` guard is present in both `weight_modifier` and `ai_weight` | Fourteen occurrences | `grep -c 'num_owned_planets < 2'` | Early-game technology competition against colonisation |

## Category 5: Presentation invariants

| # | Invariant | Evidence | Check | Violation effect |
|---|-----------|----------|-------|------------------|
| I26 | Deposit icons are 98x75, 32-bit ARGB8888 DDS | Inspected file headers | `file gfx/interface/icons/deposits/*.dds` | Rendering artefacts or failure to load |
| I27 | Technology icons are 52x52, 24-bit RGB888 DDS | Inspected file headers | `file gfx/interface/icons/technologies/*.dds` | Rendering artefacts |
| I28 | Both localisation files are UTF-8 with BOM | `file` reports "with BOM" | `file localisation/english/*.yml` | Encoding errors; engine may fail to read keys |
| I29 | Blocker descriptions and technology descriptions end with the trailing newline encoding used by the existing entries | `_desc:0` blocks contain `\n\n` in the technology file | Inspect `_desc` values | Tooltip layout regression |

## Category 6: Build and distribution invariants

| # | Invariant | Evidence | Check | Violation effect |
|---|-----------|----------|-------|------------------|
| I30 | No build, test, or packaging step is required | No build files exist; the sole workflow publishes an already-packaged archive | File inspection | A contributor assuming a build step wastes effort |
| I31 | `.gitignore` ignores packaged archives (`*.zip`) only | `.gitignore` content | Inspect file | Accidental commit of a release archive, or accidental exclusion of mod content |
| I32 | The only workflow is the Nexus Mods publisher; no validation workflow exists | `.github/workflows/nexus-mods-release.yml` is the sole workflow file | Directory inspection | No automated validity gate — see [`testing.md`](testing.md); tag pushes attempt a Nexus Mods upload |
| I33 | The mod is load-order independent | All identifiers are mod-namespaced (`ui` infix); no vanilla override file exists | Identifier inspection | Load-order-dependent breakage |

## Invariants that are deliberately *not* asserted

| Candidate | Why it is not an invariant |
|-----------|---------------------------|
| `resources.category` present on every deposit | Only Ashland declares it; six deposits omit it. Recorded as an observed inconsistency, not an invariant — see [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) |
| Balanced drop frequency between blockers | No measurement exists; the baselines differ deliberately |
| Technology roll rate | Engine-owned and not measured |
| Save-game forward/backward compatibility on blocker removal | Not verified |

## Where to go next

- Check these automatically where possible: [`testing.md`](testing.md)
- Understand why each invariant exists: [`design-decisions.md`](design-decisions.md)
