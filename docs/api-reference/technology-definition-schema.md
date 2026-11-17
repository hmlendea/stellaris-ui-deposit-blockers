# Technology definition schema

This document specifies the technology block schema in isolation, so contributors can modify a technology without reading the deposit schema. For general notes and the identifier catalogue, see [`INDEX.md`](INDEX.md).

## Block form

```
tech_d_ui_<name>_blocker = {
    cost = @tier<t>cost2
    area = society
    tier = <t>
    category = { new_worlds }
    prerequisites = { "tech_colonization_1" }
    weight = @tier<t>weight2

    potential = {
        is_nomadic = no
    }

    weight_groups = {
        deposit_blockers
    }
    mod_weight_if_group_picked = {
        deposit_blockers = 0.5
    }

    weight_modifier = { ... six modifiers ... }
    ai_weight = { ... four modifiers ... }
}
```

Where `<t>` is `1` or `2`, and `<name>` matches the corresponding deposit's `<name>` token.

## Header fields

| Field | Type | Tier 1 value | Tier 2 value | Constraint |
|-------|------|--------------|--------------|------------|
| `cost` | Scripted variable | `@tier1cost2` | `@tier2cost2` | Must correspond to `tier` |
| `area` | Identifier | `society` | `society` | Constant across the family |
| `tier` | Integer | `1` | `2` | Drives the two variables above |
| `category` | Identifier set | `{ new_worlds }` | `{ new_worlds }` | Constant across the family |
| `prerequisites` | Identifier list | `tech_colonization_1` | `tech_colonization_1` | Constant across the family |
| `weight` | Scripted variable | `@tier1weight2` | `@tier2weight2` | Must correspond to `tier` |

### Tier assignment in this repository

| Tier 1 | Tier 2 |
|--------|--------|
| `tech_d_ui_ashland_blocker` | `tech_d_ui_giant_earthworms_blocker` |
| `tech_d_ui_canyon_blocker` | `tech_d_ui_lava_lake_blocker` |
| `tech_d_ui_floodplain_blocker` | `tech_d_ui_sea_monsters_blocker` |
| `tech_d_ui_tarland_blocker` | — |

## Availability gate

```
potential = {
    is_nomadic = no
}
```

| Field | Type | Value | Effect |
|-------|------|-------|--------|
| `potential` | Condition block | `is_nomadic = no` | Excludes nomadic empires from the research pool for this technology |

Because this is a `potential` block, an excluded empire never sees the technology at all, unlike a `weight_modifier` with `factor = 0` which suppresses weight.

## Weight group

```
weight_groups = {
    deposit_blockers
}
mod_weight_if_group_picked = {
    deposit_blockers = 0.5
}
```

| Field | Type | Value | Contract |
|-------|------|-------|----------|
| `weight_groups` | Identifier set | `deposit_blockers` | Group name is global; a third-party mod could join the same group |
| `mod_weight_if_group_picked` | Block | `deposit_blockers = 0.5` | Key must exactly equal the entry in `weight_groups` |

The group is declared per technology, so the name and the decay value are repeated seven times. Any change must be applied to all seven blocks to preserve balance.

## `weight_modifier` — player availability

Six modifiers, in fixed order:

| # | Condition | Factor | Mechanism |
|---|-----------|--------|-----------|
| 1 | (unconditional) | `2` | Base multiplier applied to `weight` |
| 2 | `has_tradition = tr_expansion_adopt` | `1.25` | Vanilla tradition check |
| 3 | `has_ascension_perk = ap_mastery_of_nature` | `4` | Vanilla ascension perk check |
| 4 | `NOT { any_owned_planet = { has_deposit = "d_ui_<name>_blocker" } }` | `0` | Suppression when unusable |
| 5 | `has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }` | `1.25` | Vanilla council trait check |
| 6 | `num_owned_planets < 2` | `0` | Early-game suppression |

Modifier 4 is the only element that must be edited per technology when a blocker is renamed.

### Multiplicative composition

Because every modifier is multiplicative, a fully progressed player with all three bonuses active resolves to:

```text
base weight × 2 × 1.25 × 4 × 1.25 = base weight × 12.5
```

before group decay. With the group decay applied after a first member is researched, the value becomes `base weight × 6.25`. This arithmetic is derived from the declared factors; the engine's exact evaluation order is not observable from the repository, so treat the number as indicative rather than exact.

Any suppression modifier (`factor = 0`) collapses the product to `0` regardless of the other factors.

## `ai_weight` — AI availability

Four modifiers:

| # | Condition | Factor |
|---|-----------|--------|
| 1 | (unconditional) | `3` |
| 2 | `NOT { any_owned_planet = { has_deposit = "d_ui_<name>_blocker" } }` | `0` |
| 3 | `has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }` | `1.25` |
| 4 | `num_owned_planets < 2` | `0` |

Differences from the player path:

- Base `3` instead of `2`.
- The tradition and ascension-perk modifiers are absent, so AI empires do not benefit from the player's progression bonuses.

## Identifier coupling

The technology block references its deposit in exactly one place: the `has_deposit` value inside modifier 4 of both weight paths.

| Direction | Where | Consequence of a broken link |
|-----------|-------|------------------------------|
| Technology → deposit | `has_deposit = "d_ui_<name>_blocker"` (twice per block) | Technology remains available when no owned planet carries the blocker |
| Deposit → technology | `prerequisites` in the deposit block | Blocker becomes unclearable |

Both directions must agree. The cross-check in [`../testing.md`](../testing.md) verifies the deposit → technology direction only; the reverse direction requires manual inspection or an identifier-set comparison, which [`../change-guide.md`](../change-guide.md) describes.

## Where to go next

- Deposit schema: [`deposit-definition-schema.md`](deposit-definition-schema.md)
- Identifier catalogue: [`INDEX.md`](INDEX.md)
- Annotated examples: [`../api-usage-examples.md`](../api-usage-examples.md)
