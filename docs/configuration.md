# Configuration

## Configuration surfaces

The mod has no runtime configuration file, no environment variables, no command-line options, and no user-facing settings. All tunable behaviour is expressed inside the game script files listed below.

| Surface | File | Scope | Who edits it |
|---------|------|-------|--------------|
| Mod identity | `ui-deposit-blockers.mod`, `ui-deposit-blockers/descriptor.mod` | Launcher registration | Maintainer |
| Placement weights | Scripted variables at the top of the deposits file | All seven blockers | Maintainer |
| Blocker behaviour | Seven deposit blocks | Per blocker | Maintainer |
| Technology behaviour | Seven technology blocks | Per technology | Maintainer |
| Display strings | Two localisation tables | Per identifier | Maintainer |
| Distribution selection | README badges and store listings | Publication only | Maintainer |

There is no user-configurable setting. A player's only controls are enabling or disabling the mod in the launcher and choosing whether to clear a blocker in game.

## Descriptor configuration

| Key | Value | Notes |
|-----|-------|-------|
| `name` | `Universum Infinitum - Deposit Blockers` | Must be identical in both files |
| `path` | `mod/ui-deposit-blockers` | Relative to the game's mod root |
| `tags` | `Galaxy Generation`, `Technologies` | Two entries |
| `supported_version` | `v4.5.*` | Wildcard patch acceptance within 4.5 |

The descriptor has no supersede mechanism: both copies are read in different installation scenarios, and neither overrides the other.

## Placement-weight configuration

| Variable | Value | Referenced by | Combined effect |
|----------|-------|---------------|-----------------|
| `@blocker_high` | `10` | Canyon, Floodplain | Highest base weight |
| `@blocker_medium` | `7` | Ashland, Lava Lake, Tarland | Mid base weight |
| `@blocker_low` | `5` | Giant Earthworms, Sea Monsters | Lowest base weight |

Each deposit's effective weight is the baseline multiplied by its modifiers, subject to the universal `num_free_districts` zero guard. Because the baselines are shared, changing one value changes the relative frequency of every blocker that references it.

## Per-blocker configuration

The complete field reference lives in [`api-reference/deposit-definition-schema.md`](api-reference/deposit-definition-schema.md). The configuration summary:

| Blocker | Baseline | Time | Energy | Minerals | Districts | Tier | Swap types | `on_cleared` |
|---------|----------|------|--------|----------|-----------|------|------------|--------------|
| Ashland | medium | 270 | 1000 | — | -1 | 1 | yes | no |
| Canyon | high | 270 | 1000 | — | -1 | 1 | yes | no |
| Floodplain | high | 180 | 500 | — | -1 | 1 | yes | no |
| Giant Earthworms | low | 270 | 500 | — | -2 | 2 | no | yes |
| Lava Lake | medium | 180 | 500 | — | -1 | 2 | yes | no |
| Sea Monsters | low | 270 | 500 | — | -2 | 2 | no | yes |
| Tarland | medium | 270 | 750 | 250 | -1 | 1 | yes | no |

## Per-technology configuration

| Technology | Tier | Cost variable | Weight variable | Group decay | Player modifiers | AI modifiers |
|------------|------|---------------|-----------------|-------------|------------------|--------------|
| `tech_d_ui_ashland_blocker` | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 | 6 | 4 |
| `tech_d_ui_canyon_blocker` | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 | 6 | 4 |
| `tech_d_ui_floodplain_blocker` | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 | 6 | 4 |
| `tech_d_ui_giant_earthworms_blocker` | 2 | `@tier2cost2` | `@tier2weight2` | 0.5 | 6 | 4 |
| `tech_d_ui_lava_lake_blocker` | 2 | `@tier2cost2` | `@tier2weight2` | 0.5 | 6 | 4 |
| `tech_d_ui_sea_monsters_blocker` | 2 | `@tier2cost2` | `@tier2weight2` | 0.5 | 6 | 4 |
| `tech_d_ui_tarland_blocker` | 1 | `@tier1cost2` | `@tier1weight2` | 0.5 | 6 | 4 |

All seven share the same modifier set. The only per-technology differences are tier, the cost and weight variables implied by the tier, and the blocker identifier used in the `has_deposit` check.

## Technology modifier reference

| Modifier | Condition | Factor | Applies to |
|----------|-----------|--------|-----------|
| Base | — | 2 | `weight_modifier` |
| Tradition | `has_tradition = tr_expansion_adopt` | 1.25 | `weight_modifier` |
| Ascension perk | `has_ascension_perk = ap_mastery_of_nature` | 4 | `weight_modifier` |
| Deposit absent | No owned planet has the blocker | 0 | `weight_modifier` and `ai_weight` |
| Council trait | `has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }` | 1.25 | `weight_modifier` and `ai_weight` |
| Small empire | `num_owned_planets < 2` | 0 | `weight_modifier` and `ai_weight` |
| AI base | — | 3 | `ai_weight` |

## Precedence

There is no configuration precedence because there is no layered configuration. The only effective override is that a later-loaded mod defining the same identifier replaces this mod's definition; this mod avoids that scenario by namespace-guarding every identifier with the `ui` infix.

## Reload behaviour

| Change type | Reload requirement |
|-------------|-------------------|
| Definitions (`common/`) | Game restart — the engine parses scripts at startup |
| Localisation | Game restart in practice; the engine reloads strings when the locale changes |
| Descriptor metadata | Launcher restart |
| Existing save games | Changes do not retroactively alter planets generated earlier |

## Validation

Run the cross-checks in [`testing.md`](testing.md). There is no configuration validator and no schema file; the engine is the only validator.

## Where to go next

- Exact field constraints: [`api-reference/INDEX.md`](api-reference/INDEX.md)
- Changing a value safely: [`change-guide.md`](change-guide.md)
- Configuration-dependent failures: [`troubleshooting.md`](troubleshooting.md)
