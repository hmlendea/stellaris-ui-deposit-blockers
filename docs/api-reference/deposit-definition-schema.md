# Model field reference

This is the schema reference. For the schema index and the identifier catalogue, see [`INDEX.md`](INDEX.md).

## Deposit block schema

A deposit block is a named scope whose fields configure blocker behaviour.

| Field | Required | Type | Accepted values in this mod | Constraint |
|-------|----------|------|------------------------------|------------|
| `time` | Yes | Integer (days) | `180`, `270` | Positive |
| `is_for_colonizable` | Yes | Boolean | `yes` | Must be `yes` for the intended scope |
| `category` | Yes | Identifier | `deposit_cat_blockers` | Must be a vanilla deposit category |
| `resources` | Yes | Block | See below | Must contain `cost` |
| `potential` | Yes | Condition block | `is_dry = yes` / `is_wet = yes` / `AND { is_dry = yes; planet_size >= 10 }` | Predicates must exist |
| `planet_modifier` | Yes | Block | `planet_max_districts_add = -1` or `-2` | Negative integer |
| `all_blocker_swap_types` | Conditional | Boolean | `yes` on five deposits | Present only on `-1` blockers |
| `on_cleared` | Conditional | Effect block | Present on the two `-2` blockers | Contains `hidden_effect` |
| `drop_weight` | Yes | Block | See below | Must contain `weight` |
| `prerequisites` | Yes | Identifier list | `{ "tech_d_ui_<name>_blocker" }` | Must resolve to a defined technology |

### `resources` sub-schema

| Field | Required | Type | Values used | Notes |
|-------|----------|------|-------------|-------|
| `category` | Conditional | Identifier | `deposit_blockers` | Present on `d_ui_ashland_blocker` only — see [A1](../ambiguities-and-open-questions.md) |
| `cost` | Yes | Block | — | Contains at least one resource |
| `cost.energy` | Yes | Integer | `500`, `750`, `1000` | All seven deposits |
| `cost.minerals` | Optional | Integer | `250` | `d_ui_tarland_blocker` only |

### `potential` variants used

| Variant | Block |
|---------|-------|
| Dry climate | `potential = { is_dry = yes }` |
| Wet climate | `potential = { is_wet = yes }` |
| Dry and sized | `potential = { AND = { is_dry = yes; planet_size >= 10 } }` |
| Wet and sized | `potential = { AND = { is_wet = yes; planet_size >= 10 } }` |

### `on_cleared` schema

| Field | Type | Value | Purpose |
|-------|------|-------|---------|
| `hidden_effect` | Block | — | Prevents a visible tooltip entry |
| `if.limit.exists` | Condition | `owner` | Guards against unowned planets |
| `if.limit.owner.has_civic` | Condition | `civic_hive_devouring_swarm` | Selects the beneficiary |
| `if.owner.add_resource.food` | Integer | `200` | The grant |

### `drop_weight` schema

| Field | Required | Type | Value |
|-------|----------|------|-------|
| `weight` | Yes | Scripted variable reference | `@blocker_high`, `@blocker_medium`, `@blocker_low` |
| `modifier` | Yes (repeatable) | Block | Contains `factor` and a condition |

### `drop_weight.modifier` conditions observed

| Condition shape | Blocks using it | Factor(s) |
|-----------------|-----------------|-----------|
| `planet_size < 15` | Six (Floodplain omits) | `0.5` |
| `is_planet_class = <class>` | Ashland (`pc_nuked`), Sea Monsters (`pc_ocean`) | `3` |
| `is_planet_class = <class>` | Ashland (`pc_alpine`) | `2` |
| `OR { is_planet_class = <a>; is_planet_class = <b> }` | Canyon, Floodplain, Lava Lake, Tarland | `2` |
| `num_free_districts = { type = district_city; value < 2 }` | All seven | `0` |

## Technology block schema

| Field | Required | Type | Accepted values in this mod | Constraint |
|-------|----------|------|------------------------------|------------|
| `cost` | Yes | Scripted variable | `@tier1cost2`, `@tier2cost2` | Must match the declared tier |
| `area` | Yes | Identifier | `society` | Must be a vanilla research area |
| `tier` | Yes | Integer | `1`, `2` | Must match the cost and weight variables |
| `category` | Yes | Identifier set | `{ new_worlds }` | Must be a vanilla technology category |
| `prerequisites` | Yes | Identifier list | `{ "tech_colonization_1" }` | Must resolve to a defined technology |
| `weight` | Yes | Scripted variable | `@tier1weight2`, `@tier2weight2` | Must match the declared tier |
| `potential` | Yes | Condition block | `is_nomadic = no` | — |
| `weight_groups` | Yes | Identifier set | `deposit_blockers` | Shared across the family |
| `mod_weight_if_group_picked` | Yes | Block | `deposit_blockers = 0.5` | Group key must match `weight_groups` |
| `weight_modifier` | Yes | Modifier list | Six modifiers | See below |
| `ai_weight` | Yes | Modifier list | Four modifiers | See below |

### `weight_modifier` schema

| Index | Condition | Factor |
|-------|-----------|--------|
| 1 | (none) | `2` |
| 2 | `has_tradition = tr_expansion_adopt` | `1.25` |
| 3 | `has_ascension_perk = ap_mastery_of_nature` | `4` |
| 4 | `NOT { any_owned_planet = { has_deposit = "d_ui_<name>_blocker" } }` | `0` |
| 5 | `has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }` | `1.25` |
| 6 | `num_owned_planets < 2` | `0` |

The blocker identifier inside modifier 4 is the only per-technology variation in the entire modifier set.

### `ai_weight` schema

| Index | Condition | Factor |
|-------|-----------|--------|
| 1 | (none) | `3` |
| 2 | `NOT { any_owned_planet = { has_deposit = "d_ui_<name>_blocker" } }` | `0` |
| 3 | `has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }` | `1.25` |
| 4 | `num_owned_planets < 2` | `0` |

The AI path omits the tradition and ascension-perk modifiers and uses base `3` rather than `2`.

## Localisation entry schema

| Part | Format | Example |
|------|--------|---------|
| Block header | `l_english:` | — |
| Name entry | `<identifier>:<version> "<text>"` | `d_ui_ashland_blocker:0 "£blocker  Ashland"` |
| Description entry | `<identifier>_desc:<version> "<text>"` | `d_ui_ashland_blocker_desc:0 "This whole region is a desolate ash wasteland."` |
| Technology name version | `0` | `tech_d_ui_ashland_blocker:0` |
| Technology description version | `1` | `tech_d_ui_ashland_blocker_desc:1` |
| Escape sequences | `\n\n` at end of technology descriptions | — |
| File encoding | UTF-8 with BOM | — |

## Mod descriptor schema

| Key | Type | Value |
|-----|------|-------|
| `name` | String | `Universum Infinitum - Deposit Blockers` |
| `path` | String | `mod/ui-deposit-blockers` |
| `tags` | String list | `Galaxy Generation`, `Technologies` |
| `supported_version` | String | `v4.5.*` |

## Where to go next

- Schema index and identifier catalogue: [`INDEX.md`](INDEX.md)
- Annotated complete examples: [`../api-usage-examples.md`](../api-usage-examples.md)
