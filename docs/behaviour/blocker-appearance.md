# Blocker appearance behaviour

## Summary

Blockers appear automatically on eligible generated planets. The player has no control over placement; the only player choice is whether and when to clear a blocker after researching its removal technology.

## Which planets can receive which blocker

| Blocker | Climate requirement | Size requirement | Favoured classes | Base weight |
|---------|--------------------|------------------|------------------|-------------|
| Ashland | dry | — | `pc_nuked` (×3), `pc_alpine` (×2) | 7 |
| Canyon | dry | — | `pc_arid`, `pc_savannah` (×2) | 10 |
| Floodplain | wet | — | `pc_ocean`, `pc_continental` (×2) | 10 |
| Giant Earthworms | dry | ≥ 10 | none | 5 |
| Lava Lake | dry | — | `pc_desert`, `pc_arid` (×2) | 7 |
| Sea Monsters | wet | ≥ 10 | `pc_ocean` (×3) | 5 |
| Tarland | dry | — | `pc_desert`, `pc_arid` (×2) | 7 |

## What the player observes

| Observation | Explanation |
|-------------|-------------|
| A blocked region appears on a planet with a readable name | The localisation key resolved |
| The blocker shows an icon | The icon file name matched the identifier |
| The planet's maximum districts are reduced by one or two | `planet_max_districts_add` is in effect |
| The tooltip states a clearing cost and duration | `resources.cost` and `time` are declared |
| Clearing is refused at first | The prerequisite technology is not yet researched |
| The removal technology appears in the society tree | Its weight became non-zero after the empire owned two planets and a matching planet |
| Clearing restores districts and consumes resources | The cost is paid and `planet_modifier` is reversed |
| A devouring swarm receives food after clearing two specific blockers | The `on_cleared` effect fired |

## Frequency expectations

The declared effect on relative frequency, before any class or size modifier:

| Relationship | Ratio |
|--------------|-------|
| Canyon (10) versus Ashland (7) | 10:7 |
| Ashland (7) versus Giant Earthworms (5) | 7:5 |
| Canyon (10) versus Giant Earthworms (5) | 2:1 |

Two structural factors compress the perceived differences:

1. **The class modifiers are larger than the baseline spread.** A `factor = 2` class bonus can make a `@blocker_low` blocker more likely on its favoured class than a `@blocker_high` blocker on an unfavoured one.
2. **The universal free-district guard removes all candidates on cramped planets**, so on a planet with fewer than two free city districts no blocker from this mod will be placed at all.

Whether these produce the intended variety is not measured — see [A9](../ambiguities-and-open-questions.md).

## Climate-family outcomes

| Planet climate | Possible blockers from this mod | Never |
|----------------|-------------------------------|-------|
| Dry | Ashland, Canyon, Giant Earthworms, Lava Lake, Tarland | Floodplain, Sea Monsters |
| Wet | Floodplain, Sea Monsters | Ashland, Canyon, Giant Earthworms, Lava Lake, Tarland |
| Neither dry nor wet | None | All seven |

Dry planets have five possible blockers and wet planets have two, so the mod's variety is concentrated in the dry climate family. This follows directly from the `potential` gates.

## Size-related outcomes

| Planet size | Consequence |
|-------------|-------------|
| ≥ 15 | No size penalty; every blocker competes at full weight |
| 10–14 | All blockers except Floodplain receive `factor = 0.5` |
| < 10 | Only Ashland, Canyon, Floodplain, Lava Lake, and Tarland are candidates; the two `-2` blockers are excluded |
| Any size, fewer than two free city districts | No blocker from this mod is placed |

Floodplain's absence from the size penalty means a small wet planet is somewhat more likely to receive Floodplain than a comparable small dry planet is to receive any single dry blocker.

## Behaviour summary by blocker

| Blocker | Placement | Severity | Clearing gate | Clearing cost |
|---------|-----------|----------|---------------|---------------|
| Ashland | Dry planets; most likely on nuked, then alpine | -1 district | Tier 1 | 1000 energy, 270 days |
| Canyon | Dry planets; most likely on arid or savannah | -1 district | Tier 1 | 1000 energy, 270 days |
| Floodplain | Wet planets; most likely on ocean or continental; no size penalty | -1 district | Tier 1 | 500 energy, 180 days |
| Giant Earthworms | Dry planets of size ≥ 10; lowest base weight | -2 districts | Tier 2 | 500 energy, 270 days |
| Lava Lake | Dry planets; most likely on desert or arid | -1 district | Tier 2 | 500 energy, 180 days |
| Sea Monsters | Wet planets of size ≥ 10; most likely on ocean | -2 districts | Tier 2 | 500 energy, 270 days |
| Tarland | Dry planets; most likely on desert or arid | -1 district | Tier 1 | 750 energy + 250 minerals, 270 days |

## Where to go next

- Research and clearing behaviour: [`research-and-clearing.md`](research-and-clearing.md)
- Placement mechanics: [`../flows/planet-generation-and-blocker-placement.md`](../flows/planet-generation-and-blocker-placement.md)
- Frequency uncertainty: [`../ambiguities-and-open-questions.md`](../ambiguities-and-open-questions.md)
