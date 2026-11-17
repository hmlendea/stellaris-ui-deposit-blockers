# Definition examples

Complete, annotated examples taken from the repository. These show the schemas in [`api-reference/`](api-reference/INDEX.md) applied to real content, plus counter-examples that illustrate the variation between blocks.

## Example 1: Minimum viable deposit

`d_ui_lava_lake_blocker` is the closest to a minimal conforming deposit — one cost resource, one potential predicate, a short weight block, and a swap-type declaration.

```
# Lava Lake
d_ui_lava_lake_blocker = {
    time = 180

    is_for_colonizable = yes
    category = deposit_cat_blockers

    resources = {
        cost = {
            energy = 500
        }
    }

    potential = {
        is_dry = yes
    }

    planet_modifier = {
        planet_max_districts_add = -1
    }

    all_blocker_swap_types = yes

    drop_weight = {
        weight = @blocker_medium
        modifier = {
            factor = 0.5
            planet_size < 15
        }
        modifier = {
            factor = 2
            OR = {
                is_planet_class = pc_desert
                is_planet_class = pc_arid
            }
        }
        modifier = {
            factor = 0
            num_free_districts = {
                type = district_city
                value < 2
            }
        }
    }

    prerequisites = { "tech_d_ui_lava_lake_blocker" }
}
```

Annotations:

| Element | Why it is there |
|---------|-----------------|
| `time = 180` | Shorter clearing time aligned with its tier and low severity |
| `is_dry = yes` | Restricts placement to the dry climate family, which is what makes the volcanic theme coherent |
| `@blocker_medium` | Mid baseline, matching its moderate severity |
| `factor = 0.5` on `planet_size < 15` | Discourages placement on cramped planets |
| `factor = 2` on desert/arid OR | Expresses the volcanic theme |
| `factor = 0` on `num_free_districts` | Universal guard; present in all seven deposits |
| `all_blocker_swap_types = yes` | Present because this is a `-1` blocker |

## Example 2: Deposit with a resource category (the only one)

`d_ui_ashland_blocker` differs from every other deposit in exactly one respect: its `resources` block declares a category.

```
    resources = {
        category = deposit_blockers
        cost = {
            energy = 1000
        }
    }
```

Compare with `d_ui_canyon_blocker`, which is otherwise the closest analogue (same climate gate, same district effect, same clearing time) but declares only `cost`. The asymmetry is tracked in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) A1; when adding a new deposit, prefer the form used by six of the seven.

## Example 3: Deposit with a post-clearing effect

`d_ui_sea_monsters_blocker` is one of two `-2` blockers and carries an `on_cleared` effect instead of `all_blocker_swap_types`.

```
    potential = {
        AND = {
            is_wet = yes
            planet_size >= 10
        }
    }

    planet_modifier = {
        planet_max_districts_add = -2
    }

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

    drop_weight = {
        weight = @blocker_low
        modifier = {
            factor = 0.5
            planet_size < 15
        }
        modifier = {
            factor = 3
            is_planet_class = pc_ocean
        }
        modifier = {
            factor = 0
            num_free_districts = {
                type = district_city
                value < 2
            }
        }
    }
```

Design observations:

- `AND` combines the climate gate with a size gate because a `-2` blocker on a tiny planet would be disproportionately punishing.
- The class modifier is a single `is_planet_class = pc_ocean` at `factor = 3`, not an `OR` pair, because only one class is thematically correct.
- `@blocker_low` balances the severity against frequency: the most damaging blockers appear least often.
- The `on_cleared` effect is gated on `exists = owner` before the `owner` scope is entered, which is required because unowned planets have no owner to grant resources to.

## Example 4: Deposit with a second resource cost

`d_ui_tarland_blocker` is the only deposit costing minerals as well as energy.

```
    resources = {
        cost = {
            energy = 750
            minerals = 250
        }
    }
```

Its class modifiers are identical to Lava Lake's (`pc_desert` and `pc_arid` at factor 2), so the two blockers compete directly on the same worlds while differing in cost, duration (270 versus 180), and baseline (both `@blocker_medium`).

## Example 5: Full technology block

`tech_d_ui_canyon_blocker`, complete:

```
tech_d_ui_canyon_blocker = {
    cost = @tier1cost2
    area = society
    tier = 1
    category = { new_worlds }
    prerequisites = { "tech_colonization_1" }
    weight = @tier1weight2

    potential = {
        is_nomadic = no
    }

    weight_groups = {
        deposit_blockers
    }
    mod_weight_if_group_picked = {
        deposit_blockers = 0.5
    }

    weight_modifier = {
        modifier = {
            factor = 2
        }
        modifier = {
            factor = 1.25
            has_tradition = tr_expansion_adopt
        }
        modifier = {
            factor = 4
            has_ascension_perk = ap_mastery_of_nature
        }
        modifier = {
            factor = 0
            NOT = {
                any_owned_planet =  {
                    has_deposit = "d_ui_canyon_blocker"
                }
            }
        }
        modifier = {
            factor = 1.25
            has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }
        }
        modifier = {
            factor = 0
            num_owned_planets < 2
        }
    }

    ai_weight = {
        modifier = {
            factor = 3
        }
        modifier = {
            factor = 0
            NOT = {
                any_owned_planet =  {
                    has_deposit = "d_ui_canyon_blocker"
                }
            }
        }
        modifier = {
            factor = 1.25
            has_trait_in_council = { TRAIT = leader_trait_expertise_new_worlds }
        }
        modifier = {
            factor = 0
            num_owned_planets < 2
        }
    }
}
```

Note the doubled space in `any_owned_planet =  {` inside both weight paths. This whitespace pattern is present in all seven technology blocks and is harmless to the parser, but it is worth preserving for consistency when copying a block as a template.

### What changes when copying this block for a new technology

| Field | Change required |
|-------|-----------------|
| Block name | `tech_d_ui_<new>_blocker` |
| `cost` | Keep if tier 1; switch to `@tier2cost2` if tier 2 |
| `tier` | Keep `1` or change to `2` |
| `weight` | Keep if tier 1; switch to `@tier2weight2` if tier 2 |
| `has_deposit` (two occurrences) | `"d_ui_<new>_blocker"` |
| Everything else | Unchanged — the family shares an identical modifier set |

## Example 6: Localisation entries

```
l_english:
d_ui_ashland_blocker:0 "£blocker  Ashland"
d_ui_ashland_blocker_desc:0 "This whole region is a desolate ash wasteland."
```

```
l_english:
tech_d_ui_ashland_blocker:0 "Ashland Removal"
tech_d_ui_ashland_blocker_desc:1 "Special machines dedicated to heavy soil enhancement and atmospheric cleansing to keep the ash away.\n\n"
```

| Pattern | Blocker file | Technology file |
|---------|--------------|-----------------|
| Version marker on names | `:0` | `:0` |
| Version marker on descriptions | `:0` | `:1` |
| Icon glyph prefix | `£blocker` plus two spaces | none |
| Trailing escape | none | `\n\n` |
| Encoding | UTF-8 with BOM | UTF-8 with BOM |

## Example 7: Cross-file coupling

A single logical blocker spans five surfaces. `d_ui_canyon_blocker` / `tech_d_ui_canyon_blocker` appears as:

| Surface | Value |
|---------|-------|
| Deposit identifier | `d_ui_canyon_blocker` |
| Deposit prerequisite | `"tech_d_ui_canyon_blocker"` |
| Technology identifier | `tech_d_ui_canyon_blocker` |
| Technology back-reference | `has_deposit = "d_ui_canyon_blocker"` (twice) |
| Blocker localisation | `d_ui_canyon_blocker:0`, `d_ui_canyon_blocker_desc:0` |
| Technology localisation | `tech_d_ui_canyon_blocker:0`, `tech_d_ui_canyon_blocker_desc:1` |
| Blocker icon | `gfx/interface/icons/deposits/d_ui_canyon_blocker.dds` |
| Technology icon | `gfx/interface/icons/technologies/tech_d_ui_canyon_blocker.dds` |

Renaming the `<name>` token requires editing all of these. [`change-guide.md`](change-guide.md) provides the procedure.

## Where to go next

- Schema detail: [`api-reference/deposit-definition-schema.md`](api-reference/deposit-definition-schema.md), [`api-reference/technology-definition-schema.md`](api-reference/technology-definition-schema.md)
- Safe modification procedure: [`change-guide.md`](change-guide.md)
- Cross-check commands: [`testing.md`](testing.md)
