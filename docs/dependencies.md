# Dependencies

## Dependency classes

| Class | Count | Modifiable by the maintainer |
|-------|-------|------------------------------|
| Vanilla scripted variables | 4 | No — supplied by the base game |
| Vanilla identifiers (technology, category, civic, tradition, perk, trait) | 8 distinct | No — supplied by the base game |
| Vanilla predicate identifiers | 6 distinct | No |
| Engine file-format contracts | 5 | No |
| Distribution platforms | 4 | Partially — publication is maintainer-controlled |
| Other mods | 0 | — |

## Vanilla scripted variables

| Variable | Used by | Tier | Consequence of vanilla change |
|----------|---------|------|-------------------------------|
| `@tier1cost2` | Five tier 1 technologies (`cost`) | 1 | Research cost of five technologies moves with the base game |
| `@tier1weight2` | Five tier 1 technologies (`weight`) | 1 | Base selection weight moves with the base game |
| `@tier2cost2` | Two tier 2 technologies (`cost`) | 2 | Research cost of two technologies moves with the base game |
| `@tier2weight2` | Two tier 2 technologies (`weight`) | 2 | Base selection weight moves with the base game |

These variables are not declared in the repository. They resolve from the base game, so the mod cannot pin a cost or weight value independently without overriding the variables locally.

## Vanilla identifiers

| Identifier | Kind | Referenced by | Role | Consequence of rename or removal |
|------------|------|---------------|------|----------------------------------|
| `tech_colonization_1` | Technology | All 7 `prerequisites` | Gate for every removal technology | The entire family becomes unresearched and no blocker can be cleared |
| `new_worlds` | Technology category | All 7 `category` | Places the family in the society tree | The family disappears from the intended tree location |
| `deposit_cat_blockers` | Deposit category | All 7 deposits | Marks the deposit as a blocker | Blockers stop being treated as blockers |
| `deposit_blockers` | Resource category | Ashland only (`resources.category`) | Resource grouping of the clearing cost | Undefined for the six deposits that omit it already; see [ambiguities](ambiguities-and-open-questions.md) |
| `civic_hive_devouring_swarm` | Civic | 2 `on_cleared` blocks | Gate for the food grant | Organic blockers stop granting food |
| `tr_expansion_adopt` | Tradition | 7 `weight_modifier` blocks | `1.25` weight multiplier | Expansion-focused empires lose the research advantage |
| `ap_mastery_of_nature` | Ascension perk | 7 `weight_modifier` blocks | `4` weight multiplier | The strongest thematic synergy in the mod silently disappears |
| `leader_trait_expertise_new_worlds` | Leader trait | 14 modifier blocks (7 player + 7 AI) | `1.25` weight multiplier | Council-trait bonus lost for both players and AI |

## Vanilla predicates

| Predicate | Used by | Meaning as used |
|-----------|---------|-----------------|
| `is_dry` | Ashland, Canyon, Giant Earthworms, Lava Lake, Tarland | Planet is in the dry climate family |
| `is_wet` | Floodplain, Sea Monsters | Planet is in the wet climate family |
| `planet_size` | Six blockers (comparisons `< 15` and `>= 10`) | Planet size for gating and weighting |
| `is_planet_class` | Six blockers, twelve comparisons | Specific planet class match |
| `num_free_districts` | All 7 (with `type = district_city`, `value < 2`) | Free city districts remaining |
| `is_nomadic` | All 7 technologies (`is_nomadic = no`) | Nomadic empire exclusion |

## Vanilla planet classes referenced

| Class | Referenced by | Weight effect |
|-------|---------------|---------------|
| `pc_nuked` | Ashland | `factor = 3` |
| `pc_alpine` | Ashland | `factor = 2` |
| `pc_arid` | Canyon, Lava Lake, Tarland | `factor = 2` |
| `pc_savannah` | Canyon | `factor = 2` |
| `pc_ocean` | Floodplain (`factor = 2`), Sea Monsters (`factor = 3`) | Class-specific |
| `pc_continental` | Floodplain | `factor = 2` |
| `pc_desert` | Lava Lake, Tarland | `factor = 2` |

## Vanilla sub-selectors

| Selector | Used by | Purpose |
|----------|---------|---------|
| `has_deposit` | All 7 `weight_modifier` and `ai_weight` blocks | Zero weight when no owned planet carries the blocker |
| `any_owned_planet` | All 7, as the enclosing scope for `has_deposit` | Planet scope for the deposit check |
| `num_owned_planets` | All 7, both weight paths | Zero weight while the empire owns fewer than two planets |
| `has_tradition` | All 7 `weight_modifier` blocks | Tradition check |
| `has_ascension_perk` | All 7 `weight_modifier` blocks | Perk check |
| `has_trait_in_council` | All 7, both weight paths | Council trait check |
| `has_civic` | 2 `on_cleared` blocks | Owner civic check |
| `exists owner` | 2 `on_cleared` blocks | Guards against unowned planets |
| `owner` scope | 2 `on_cleared` blocks | Applies the resource grant to the owner |
| `add_resource` | 2 `on_cleared` blocks | Grants `food = 200` |
| `is_dry`, `is_wet`, `is_for_colonizable`, `all_blocker_swap_types`, `OR`, `AND`, `NOT`, `limit`, `if` | Various | Condition and control flow |

## Engine file-format contracts

| Contract | Where declared | Consequence of change |
|----------|----------------|-----------------------|
| `common/deposits/*.txt` merge semantics | Directory name and script syntax | Definitions stop being loaded |
| `common/technology/*.txt` merge semantics | Directory name and script syntax | Technologies stop being loaded |
| Identifier resolution for localisation keys | Localisation table format | Raw keys displayed |
| File-name-based texture resolution under `gfx/interface/icons/` | Directory convention only — no explicit path is declared | Icons render blank |
| `.mod` descriptor key set | `ui-deposit-blockers.mod` structure | Launcher rejection |

## Distribution dependencies

| Platform | Role | Verified evidence | Risk |
|----------|------|-------------------|------|
| Steam Workshop | Primary distribution | Item `2073660612` linked in `README.md`, published by `.github/workflows/steam-workshop-release.yml` | A stale published version ships outdated definitions; an upload failure leaves the channel behind |
| Paradox Mods | Distribution | Listing `34638` linked in `README.md` | Same |
| Nexus Mods | Distribution | Listing `91`, file `955873`, published by `.github/workflows/nexus-mods-release.yml` | A stale published version ships outdated definitions; an upload failure leaves the channel behind |
| GitHub Releases | Distribution and source of truth | Releases `v1.0`, `v1.0.1`; the archive built by `.github/workflows/github-release.yml` and attached to each release | Repository content may be ahead of the last release |
| GitHub Actions | Publication automation | `.github/workflows/github-release.yml`, `softprops/action-gh-release@v2`, automatic `GITHUB_TOKEN`; `.github/workflows/nexus-mods-release.yml`, `Nexus-Mods/upload-action@v1.0.0-beta.10`, `NEXUSMODS_API_KEY` secret; `.github/workflows/steam-workshop-release.yml`, `hmlendea/steam-workshop-update@v1.0.0`, Steam secrets | Action tag changes or a missing secret block publication |
| GitHub (repository hosting) | Documentation, issues, advisories | Remote `git@github.com:hmlendea/stellaris-ui-deposit-blockers.git` | — |

## Internal dependencies

The repository has no module boundaries. Its internal coupling is identifier-based and is fully enumerated in [`invariants.md`](invariants.md) Categories 2–5.

## Cross-mod dependencies

None. No sibling mod is declared as a requirement, and no shared file is consumed. Same-name sibling mods from the Universum Infinitum collection are listed in `README.md` for discoverability only.

## Where to go next

- Vanilla-value impact on behaviour: [`flows/technology-research-and-blocker-clearing.md`](flows/technology-research-and-blocker-clearing.md)
- Integration surface: [`integrations.md`](integrations.md)
