# Technology definitions

Component: `ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt`

## Purpose

Defines the seven society technologies that unlock blocker removal. This file determines **when a player can clear a blocker**, and how strongly the research pool favours or suppresses each option.

## Structure

Seven blocks, each 63–72 lines, ordered alphabetically by the `<name>` token so that the file order matches the deposits file order:

| Line range | Technology | Tier |
|------------|------------|------|
| 1–71 | `tech_d_ui_ashland_blocker` | 1 |
| 73–143 | `tech_d_ui_canyon_blocker` | 1 |
| 145–216 | `tech_d_ui_floodplain_blocker` | 1 |
| 218–289 | `tech_d_ui_giant_earthworms_blocker` | 2 |
| 291–362 | `tech_d_ui_lava_lake_blocker` | 2 |
| 364–435 | `tech_d_ui_sea_monsters_blocker` | 2 |
| 437–508 | `tech_d_ui_tarland_blocker` | 1 |

There are no separators or comments; blocks follow one another directly.

## Common block skeleton

```
tech_d_ui_<name>_blocker = {
    cost = @tier<1|2>cost2
    area = society
    tier = <1|2>
    category = { new_worlds }
    prerequisites = { "tech_colonization_1" }
    weight = @tier<1|2>weight2

    potential = {
        is_nomadic = no
    }

    weight_groups = {
        deposit_blockers
    }
    mod_weight_if_group_picked = {
        deposit_blockers = 0.5
    }

    weight_modifier = { ... 6 modifiers ... }
    ai_weight = { ... 4 modifiers ... }
}
```

The only per-block differences are the tier, the two variables implied by the tier, and the blocker identifier inside the `has_deposit` condition.

## Tier distribution

| Tier | Technologies | Cost variable | Weight variable |
|------|--------------|---------------|-----------------|
| 1 | Ashland, Canyon, Floodplain, Tarland | `@tier1cost2` | `@tier1weight2` |
| 2 | Giant Earthworms, Lava Lake, Sea Monsters | `@tier2cost2` | `@tier2weight2` |

The tier assignment does not map exactly onto district severity: Lava Lake is tier 2 while removing a single district, and Giant Earthworms/Sea Monsters are tier 2 while removing two. See [A2](../ambiguities-and-open-questions.md).

## Availability gate

`potential = { is_nomadic = no }` excludes nomadic empires. This is the only hard gate in the block; every other adjustment is a weight change, which means a technology can theoretically still be offered against a zero-weight modifier if the engine's rollout requires a non-zero candidate — the practical consequence is that the technology is effectively suppressed.

## Weight group mechanics

```
weight_groups = {
    deposit_blockers
}
mod_weight_if_group_picked = {
    deposit_blockers = 0.5
}
```

Interpretation supported by the script:

1. Every member of this family belongs to a single named group.
2. Once a member of the group has been picked, the group's remaining members have their weight multiplied by `0.5`.
3. The group name and the decay are declared per technology, not centrally, so the constant must be kept consistent manually across all seven blocks.

Consequence for contributors: adding an eighth technology to the group lowers each existing member's realised selection share, even without editing any other block.

## `weight_modifier` construction

Every block contains the same six modifiers in the same order:

| # | Condition | Factor | Effect |
|---|-----------|--------|--------|
| 1 | (none) | 2 | Doubles the base weight |
| 2 | `has_tradition = tr_expansion_adopt` | 1.25 | Expansion adopters get a 25% boost |
| 3 | `has_ascension_perk = ap_mastery_of_nature` | 4 | Nature mastery gives the largest boost |
| 4 | No owned planet has this blocker (`NOT` + `any_owned_planet` + `has_deposit`) | 0 | Suppresses unusable technology |
| 5 | `has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }` | 1.25 | New-worlds expertise gives a 25% boost |
| 6 | `num_owned_planets < 2` | 0 | Suppresses the family before the second planet |

## `ai_weight` construction

Every block contains the same four modifiers:

| # | Condition | Factor | Effect |
|---|-----------|--------|--------|
| 1 | (none) | 3 | Triples the AI base weight |
| 2 | No owned planet has this blocker | 0 | Suppresses unusable technology |
| 3 | Council trait present | 1.25 | Same boost as the player path |
| 4 | `num_owned_planets < 2` | 0 | Same early-game suppression |

The AI path omits the tradition and ascension-perk boosts and uses an unconditional base of `3` instead of `2`. Whether the resulting AI availability is higher or lower than the player's depends on the player's current traditions and perks, because those multipliers are absent from the AI path.

## Dependency on the sibling component

Each technology embeds its blocker identifier inside the suppression check:

```
modifier = {
    factor = 0
    NOT = {
        any_owned_planet = {
            has_deposit = "d_ui_<name>_blocker"
        }
    }
}
```

This is the only place where a technology names a deposit. Together with the deposit's `prerequisites`, it forms the bidirectional pair that [`invariants.md`](../invariants.md) invariant I6 checks. If the identifier inside `has_deposit` is misspelled, the technology remains available even when no owned planet carries the blocker — a silent behavioural change with no parse error.

Renaming a blocker therefore requires editing two files, and the rename is easy to perform incompletely.

## Modification impact

| Change | Also requires |
|--------|---------------|
| Add a technology | A deposit block with the matching `prerequisites`, four localisation keys, one icon file |
| Change tier | Update `tier`, `cost`, and `weight` together so they stay consistent (invariant I21) |
| Change the group decay | Apply the same value to all seven blocks (invariant I22) |
| Add a new group member | Review whether the 0.5 decay still produces the intended spread |
| Change prerequisites | Verify the vanilla identifier exists in the target game version |

## Where to go next

- Field-by-field schema: [`../api-reference/technology-definition-schema.md`](../api-reference/technology-definition-schema.md)
- Companion component: [`deposit-definitions.md`](deposit-definitions.md)
- Research flow: [`../flows/technology-research-and-blocker-clearing.md`](../flows/technology-research-and-blocker-clearing.md)
