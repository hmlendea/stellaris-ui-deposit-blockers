# Definition reference index

This is the schema reference index. For the documentation master index, see [`../INDEX.md`](../INDEX.md).

## There is no HTTP API

This mod exposes no HTTP, gRPC, GraphQL, CLI, or library API. Its "API" is the pair of **engine script schemas** the Stellaris engine parses:

| Schema | File | Documents | Schema reference |
|--------|------|-----------|------------------|
| Deposit (blocker) definition | `ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt` | 7 blocks | [`deposit-definition-schema.md`](deposit-definition-schema.md) |
| Technology definition | `ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt` | 7 blocks | [`technology-definition-schema.md`](technology-definition-schema.md) |
| Localisation table | `ui-deposit-blockers/localisation/english/*.yml` | 28 keys | [`deposit-definition-schema.md`](deposit-definition-schema.md#localisation-entry-schema) |
| Mod descriptor | `ui-deposit-blockers.mod`, `.../descriptor.mod` | 4 keys | [`deposit-definition-schema.md`](deposit-definition-schema.md#mod-descriptor-schema) |

Conventional REST concerns therefore map as follows:

| Conventional concern | Status here |
|----------------------|-------------|
| Authentication / authorisation | Not applicable — no endpoint and no identity |
| Versioning | `supported_version="v4.5.*"` in the descriptor; no API versioning |
| Rate limiting / throttling | Not applicable — the engine owns all scheduling |
| Error status codes | Not applicable — parse failures are silent |
| Request/response bodies | Not applicable — declarations are read, not exchanged |

## Authentication schemes

None. The mod is a set of files. The only access control in play is the operating system's permission on the mod directory.

## Block catalogue

### Deposit blocks

| Identifier | File | Line | Tier | Districts | Baseline | Swap types | `on_cleared` |
|------------|------|------|------|-----------|----------|------------|--------------|
| `d_ui_ashland_blocker` | deposits | 6 | 1 | -1 | `@blocker_medium` | yes | no |
| `d_ui_canyon_blocker` | deposits | 56 | 1 | -1 | `@blocker_high` | yes | no |
| `d_ui_floodplain_blocker` | deposits | 104 | 1 | -1 | `@blocker_high` | yes | no |
| `d_ui_giant_earthworms_blocker` | deposits | 148 | 2 | -2 | `@blocker_low` | no | yes |
| `d_ui_lava_lake_blocker` | deposits | 208 | 2 | -1 | `@blocker_medium` | yes | no |
| `d_ui_sea_monsters_blocker` | deposits | 256 | 2 | -2 | `@blocker_low` | no | yes |
| `d_ui_tarland_blocker` | deposits | 320 | 1 | -1 | `@blocker_medium` | yes | no |

### Technology blocks

| Identifier | File | Line | Tier | Cost | Weight | Group decay |
|------------|------|------|------|------|--------|-------------|
| `tech_d_ui_ashland_blocker` | technology | 1 | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 |
| `tech_d_ui_canyon_blocker` | technology | 73 | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 |
| `tech_d_ui_floodplain_blocker` | technology | 145 | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 |
| `tech_d_ui_giant_earthworms_blocker` | technology | 218 | 2 | `@tier2cost2` | `@tier2weight2` | 0.5 |
| `tech_d_ui_lava_lake_blocker` | technology | 291 | 2 | `@tier2cost2` | `@tier2weight2` | 0.5 |
| `tech_d_ui_sea_monsters_blocker` | technology | 364 | 2 | `@tier2cost2` | `@tier2weight2` | 0.5 |
| `tech_d_ui_tarland_blocker` | technology | 437 | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 |

### Scripted variables

| Variable | File | Line | Value | Scope |
|----------|------|------|-------|-------|
| `@blocker_high` | deposits | 1 | `10` | Mod-local |
| `@blocker_medium` | deposits | 2 | `7` | Mod-local |
| `@blocker_low` | deposits | 3 | `5` | Mod-local |
| `@tier1cost2` | — | — | vanilla | Base game |
| `@tier1weight2` | — | — | vanilla | Base game |
| `@tier2cost2` | — | — | vanilla | Base game |
| `@tier2weight2` | — | — | vanilla | Base game |

### Localisation keys

`localisation/english/ui-tile-blockers_l_english.yml` — 14 keys:

| Key pattern | Count |
|-------------|-------|
| `d_ui_<name>_blocker:0` | 7 |
| `d_ui_<name>_blocker_desc:0` | 7 |

`localisation/english/ui-tile-blockers_technologies_l_english.yml` — 14 keys:

| Key pattern | Count |
|-----------|-------|
| `tech_d_ui_<name>_blocker:0` | 7 |
| `tech_d_ui_<name>_blocker_desc:1` | 7 |

### Icon files

| Directory | Count | Geometry | Format |
|-----------|-------|----------|--------|
| `gfx/interface/icons/deposits/` | 7 | 98x75 | 32-bit ARGB8888 |
| `gfx/interface/icons/technologies/` | 7 | 52x52 | 24-bit RGB888 |

## Vanilla identifiers referenced

| Identifier | Kind | Referenced by |
|------------|------|---------------|
| `tech_colonization_1` | Technology | All 7 technologies (`prerequisites`) |
| `new_worlds` | Technology category | All 7 technologies (`category`) |
| `deposit_cat_blockers` | Deposit category | All 7 deposits (`category`) |
| `deposit_blockers` | Resource category | Ashland only — see [`../ambiguities-and-open-questions.md`](../ambiguities-and-open-questions.md) |
| `civic_hive_devouring_swarm` | Civic | Giant Earthworms, Sea Monsters (`on_cleared`) |
| `tr_expansion_adopt` | Tradition | All 7 technologies (`weight_modifier`) |
| `ap_mastery_of_nature` | Ascension perk | All 7 technologies (`weight_modifier`) |
| `leader_trait_expertise_new_worlds` | Leader trait | All 7 technologies (both weight paths) |
| `pc_nuked`, `pc_alpine`, `pc_arid`, `pc_savannah`, `pc_ocean`, `pc_continental`, `pc_desert` | Planet classes | Six deposits (`drop_weight`) |

## Related documentation

| Topic | Document |
|-------|----------|
| Deposit schema | [`deposit-definition-schema.md`](deposit-definition-schema.md) |
| Technology schema | [`technology-definition-schema.md`](technology-definition-schema.md) |
| Complete annotated examples | [`../api-usage-examples.md`](../api-usage-examples.md) |
| Field semantics | [`../data-model.md`](../data-model.md) |
| Coupling rules | [`../invariants.md`](../invariants.md) |
| Placing and clearing flows | [`../flows/planet-generation-and-blocker-placement.md`](../flows/planet-generation-and-blocker-placement.md), [`../flows/technology-research-and-blocker-clearing.md`](../flows/technology-research-and-blocker-clearing.md) |
