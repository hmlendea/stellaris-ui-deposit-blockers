# Concurrency and scheduling

## Status statement

**Not applicable at the mod level.** The mod contains no threads, no asynchronous operations, no locks, no queues, no schedulers, no timers, no worker pools, and no shared mutable state. Every one of those concerns belongs to the game engine, which the mod cannot observe or influence.

This document therefore records what the engine schedules on the mod's behalf, so that a contributor does not mistakenly look for concurrency primitives in the repository.

## What is scheduled, and by whom

| Scheduled activity | Owner | Frequency | Mod's contribution |
|--------------------|-------|-----------|--------------------|
| Script parsing | Engine | Once per session at startup | The files to parse |
| Planet generation | Engine | During galaxy generation and planet creation | `potential` and `drop_weight` |
| Blocker drop rolls | Engine | Per candidate planet per blocker | Weight baselines and modifiers |
| Technology rollout | Engine | At each research selection | `weight`, `weight_modifier`, `ai_weight`, `weight_groups` |
| Clearing action | Engine | On player or AI request | `resources.cost`, `time`, `planet_modifier` |
| `on_cleared` execution | Engine | Immediately on clearing completion | The effect block |
| Save write | Engine | On the game's own schedule | Nothing |
| Texture load | Engine | Lazily on first display | The asset files |

## Why the mod cannot express concurrency

The script format the mod uses is a declarative definition language. A deposit block describes properties of an entity; there is no syntax for spawning, dispatching, awaiting, locking, or scheduling. The only temporal constructs reachable are engine-defined durations such as the deposit `time` field, and those are interpreted by the engine's own scheduler rather than by mod code.

## Duration fields and their scheduling meaning

| Field | Value | Scheduling interpretation |
|-------|-------|---------------------------|
| Deposit `time` | `180` or `270` | The engine schedules clearing over this many days |
| `on_cleared` | — | Executed once, synchronously with clearing completion as observed in game |
| Technology `cost` | Vanilla variable | Resolved by the engine's research progress scheduler |

The engine's exact threading model — whether these resolve on the main simulation thread or otherwise — is not observable from the repository and is not asserted here.

## Determinism considerations

| Factor | Effect on reproducibility |
|--------|---------------------------|
| Weighted selection | Non-deterministic across runs; baselines and modifiers determine distribution, not outcome |
| Group decay ordering | Depends on which family member the engine selects first |
| Ordering among candidates | Engine-owned; see [A11](ambiguities-and-open-questions.md) |
| Save/load | Placement state is persisted, so a loaded campaign is stable even though generation was stochastic |

A contributor tuning weights can therefore reason about **relative likelihood** but cannot expect a specific planet to receive a specific blocker.

## Resource-use considerations

| Resource | Consumption |
|----------|-------------|
| Processor | Comparable to any other deposit definition; the mod adds seven candidates to the blocker pool |
| Memory | Seven deposit definitions, seven technology definitions, 28 localisation keys, and fourteen textures |
| Disk | Mod content plus two store images |
| Network | None |

Adding blockers increases the candidate pool for every planet, so a large expansion of the family has a measurable — though small — effect on generation time and on the research pool size.

## Boundedness

| Bound | Value |
|-------|-------|
| Maximum blockers from this mod per planet | Unbounded by the mod; the engine decides how many blockers a planet receives |
| Maximum technology candidates from this mod | Seven, halved in realised weight after each family research |
| Maximum clearing duration | 270 days |
| Maximum district restoration per clearing | 2 |

## Where to go next

- Placement mechanics: [`flows/planet-generation-and-blocker-placement.md`](flows/planet-generation-and-blocker-placement.md)
- Engine integration: [`architecture.md`](architecture.md)
- Unverified ordering: [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) A11
