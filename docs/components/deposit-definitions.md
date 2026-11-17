# Deposit definitions

Component: `ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt`

## Purpose

Defines the seven planet blockers this mod adds, together with the scripted placement-weight baselines they share. This is the file that determines **whether and how often** a blocker appears; the technology file determines when it can be removed.

## Structure

| Section | Lines | Content |
|---------|-------|---------|
| Weight baselines | 1–3 | Three scripted variables |
| Ashland block | 5–53 | `d_ui_ashland_blocker` |
| Canyon block | 55–101 | `d_ui_canyon_blocker` |
| Floodplain block | 103–145 | `d_ui_floodplain_blocker` |
| Giant Earthworms block | 147–205 | `d_ui_giant_earthworms_blocker` |
| Lava Lake block | 207–253 | `d_ui_lava_lake_blocker` |
| Sea Monsters block | 255–317 | `d_ui_sea_monsters_blocker` |
| Tarland block | 319–366 | `d_ui_tarland_blocker` |

Blocks are separated by `# <Display Name>` comments and ordered alphabetically by the `<name>` token.

## Baseline declarations

```
@blocker_high = 10
@blocker_medium = 7
@blocker_low = 5
```

These are the only declarations in the file with a `@` prefix. They are referenced exclusively from `drop_weight.weight` values in the same file, so editing them retunes placement frequency globally.

## Common block skeleton

Every deposit follows this shape, in this order:

```
d_ui_<name>_blocker = {
    time = <180|270>

    is_for_colonizable = yes
    category = deposit_cat_blockers

    resources = {
        [category = deposit_blockers]        # Ashland only
        cost = {
            energy = <500|750|1000>
            [minerals = 250]                 # Tarland only
        }
    }

    potential = { ... }
    planet_modifier = { planet_max_districts_add = <-1|-2> }
    [all_blocker_swap_types = yes]           # Five deposits
    [on_cleared = { ... }]                   # Two deposits

    drop_weight = {
        weight = @blocker_<high|medium|low>
        modifier = { ... }
        ...
    }

    prerequisites = { "tech_d_ui_<name>_blocker" }
}
```

Field order is consistent across all seven blocks, which makes block comparison mechanical.

## Field semantics

| Field | Owned behaviour | Engine action |
|-------|-----------------|---------------|
| `time` | Clearing duration in days | Applied to the clearing action |
| `is_for_colonizable` | Availability on colonisable planets | Gates placement scope |
| `category` | Deposit categorisation | Places the deposit in `deposit_cat_blockers` |
| `resources.cost` | Clearing price | Deducted when clearing succeeds |
| `potential` | Planet eligibility | Must evaluate true or the blocker is never considered |
| `planet_modifier` | District loss while blocked | Applied while the deposit exists |
| `all_blocker_swap_types` | Engine substitution permission | Allows equivalent blocker type substitution |
| `on_cleared` | Post-clearing effect | Executed once when the blocker is removed |
| `drop_weight` | Relative placement likelihood | Rolled during generation |
| `prerequisites` | Removal gate | Must be researched before clearing is offered |

## Per-blocker variation

| Blocker | Time | Cost | Potential | Districts | Swap | `on_cleared` | Baseline |
|---------|------|------|-----------|-----------|------|--------------|----------|
| `d_ui_ashland_blocker` | 270 | 1000 energy | `is_dry` | -1 | yes | no | `@blocker_medium` |
| `d_ui_canyon_blocker` | 270 | 1000 energy | `is_dry` | -1 | yes | no | `@blocker_high` |
| `d_ui_floodplain_blocker` | 180 | 500 energy | `is_wet` | -1 | yes | no | `@blocker_high` |
| `d_ui_giant_earthworms_blocker` | 270 | 500 energy | `is_dry` AND `planet_size >= 10` | -2 | no | yes | `@blocker_low` |
| `d_ui_lava_lake_blocker` | 180 | 500 energy | `is_dry` | -1 | yes | no | `@blocker_medium` |
| `d_ui_sea_monsters_blocker` | 270 | 500 energy | `is_wet` AND `planet_size >= 10` | -2 | no | yes | `@blocker_low` |
| `d_ui_tarland_blocker` | 270 | 750 energy + 250 minerals | `is_dry` | -1 | yes | no | `@blocker_medium` |

Two correlations are worth noting for contributors:

- Both `-2` blockers are the only ones with `all_blocker_swap_types` **absent** and `on_cleared` **present**.
- Both `-2` blockers require a minimum planet size, because removing two districts from a very small planet would be disproportionate.

## Drop-weight modifier patterns

The universal modifier (present in all seven):

```
modifier = {
    factor = 0
    num_free_districts = {
        type = district_city
        value < 2
    }
}
```

This guards against rendering a planet uncolonisable. It is the only tag called out as an invariant in [`../invariants.md`](../invariants.md) (I15).

The near-universal small-planet modifier (present in six, absent on Floodplain):

```
modifier = {
    factor = 0.5
    planet_size < 15
}
```

The class-favouring modifier (present in six; Giant Earthworms has none):

| Blocker | Condition | Factor |
|---------|-----------|--------|
| Ashland | `is_planet_class = pc_nuked` | 3 |
| Ashland | `is_planet_class = pc_alpine` | 2 |
| Canyon | `pc_arid` OR `pc_savannah` | 2 |
| Floodplain | `pc_ocean` OR `pc_continental` | 2 |
| Lava Lake | `pc_desert` OR `pc_arid` | 2 |
| Sea Monsters | `is_planet_class = pc_ocean` | 3 |
| Tarland | `pc_desert` OR `pc_arid` | 2 |

Ashland is the only blocker with two class modifiers, which is consistent with it being the only `@blocker_medium` blocker with the 0.5 small-planet penalty applied.

## The `on_cleared` effect

Present identically on Giant Earthworms and Sea Monsters:

```
on_cleared = {
    hidden_effect = {
        if = {
            limit = {
                exists = owner
                owner = {
                    has_civic = civic_hive_devouring_swarm
                }
            }
            owner = {
                add_resource = {
                    food = 200
                }
            }
        }
    }
}
```

Properties:

- `hidden_effect` prevents the grant from appearing as a visible tooltip entry.
- `exists = owner` guards against unowned planets, where an `owner` scope would be invalid.
- The grant is conditional on a vanilla civic identifier, making this block dependent on a base-game definition (see [`../dependencies.md`](../dependencies.md)).

## Modification impact

| Change | Also requires |
|--------|---------------|
| Add a blocker | A technology block in the sibling file, four localisation keys, two icon files, and a README/ARCHITECTURE content update |
| Rename a blocker | Rename across all five surfaces |
| Change `potential` | Re-verify that the predicate exists in the target game version |
| Change a baseline value | Re-check whether any blocker's relative frequency is now unintuitive |
| Remove a blocker | Accept the save-game risk recorded in [`../state-and-persistence.md`](../state-and-persistence.md) |
| Add a new `@` baseline | Declare it in the header, and consider whether the existing three still express the intended ordering |

## Where to go next

- Field-by-field schema: [`../api-reference/deposit-definition-schema.md`](../api-reference/deposit-definition-schema.md)
- Companion component: [`technology-definitions.md`](technology-definitions.md)
- Placement flow: [`../flows/planet-generation-and-blocker-placement.md`](../flows/planet-generation-and-blocker-placement.md)
