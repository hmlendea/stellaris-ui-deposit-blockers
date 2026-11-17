# Technology research and blocker clearing

## Scope

Describes the two engine processes the mod's technology definitions participate in: technology availability arbitration at each research rollout, and blocker clearing once the technology is researched. The clearing process also covers the `on_cleared` effect present on two blockers.

## Part 1: research availability

```mermaid
sequenceDiagram
    participant Rollout as Research rollout
    participant Tech as Technology definition
    participant Group as deposit_blockers group
    participant Empire as Empire state
    Rollout->>Tech: Evaluate potential (is_nomadic = no)
    alt Nomadic empire
        Rollout->>Tech: Technology excluded from the pool
    else Non-nomadic empire
        Rollout->>Tech: Read weight (@tier1weight2 / @tier2weight2)
        Rollout->>Tech: Apply weight_modifier (player path)
        Tech->>Empire: num_owned_planets < 2 → weight 0
        Tech->>Empire: No owned planet has the blocker → weight 0
        Tech->>Empire: tr_expansion_adopt → ×1.25
        Tech->>Empire: ap_mastery_of_nature → ×4
        Tech->>Empire: council trait → ×1.25
        Rollout->>Group: mod_weight_if_group_picked = 0.5
        Rollout->>Rollout: Select from remaining candidates
    end
```

### Ordered steps

1. **`potential` gate.** An empire with `is_nomadic` never receives the technology, regardless of any other factor.
2. **Baseline weight.** Read from `@tier1weight2` or `@tier2weight2` depending on the tier.
3. **Suppression checks.** Two conditions collapse the weight to zero: the empire owning fewer than two planets, and no owned planet carrying the blocker. Both are present in the player and AI paths.
4. **Progression bonuses.** `tr_expansion_adopt` multiplies by `1.25`, `ap_mastery_of_nature` by `4`, and the council trait by `1.25`.
5. **Group decay.** Once any member of `deposit_blockers` has been researched, the remaining members are multiplied by `0.5`.
6. **Selection.** The engine picks from the surviving pool.

### Progressive weight behaviour

| Empire state | Effective weight |
|--------------|------------------|
| Owning one planet | 0 (early-game suppression) |
| Two or more planets, no blocker owned | 0 (no deposit check passes) |
| Two or more planets, blocker owned | `base × 2` |
| Plus expansion tradition adopted | `base × 2.5` |
| Plus nature mastery perk | `base × 10` |
| Plus new-worlds council trait | `base × 12.5` |
| Plus one family member already researched | halved by the group decay |

Because two independent conditions must pass before a technology can be offered, a technology appears abruptly in the pool the moment the empire both owns two planets and controls a planet carrying the matching blocker.

### Why a technology can appear and disappear

| Event | Effect |
|-------|--------|
| Empire conquers a second planet | Both suppression conditions become satisfiable |
| Empire acquires a planet with the blocker | Weight stops being zero |
| Empire loses every planet carrying the blocker | Weight returns to zero |
| Empire researches another family member | Remaining members halve |
| Empire adopts the nature mastery perk | Weight quadruples |

## Part 2: clearing a blocker

```mermaid
sequenceDiagram
    participant Player
    participant Clear as Clearing action
    participant Dep as Deposit definition
    participant Planet as Planet state
    participant Owner as Owner empire
    Player->>Clear: Request blocker removal
    Clear->>Dep: Check prerequisites resolved ("tech_d_ui_<name>_blocker")
    alt Technology not researched
        Clear-->>Player: Clearing unavailable
    else Technology researched
        Clear->>Dep: Read resources.cost (energy, optional minerals)
        alt Resources insufficient
            Clear-->>Player: Clearing unavailable
        else Resources affordable
            Clear->>Player: Deduct cost, schedule time (180 or 270 days)
            Clear->>Dep: Apply planet_modifier reversal (districts restored)
            Clear->>Dep: Execute on_cleared if present
            alt Owner has civic_hive_devouring_swarm
                Dep->>Owner: add_resource food = 200
            else Other owner
                Dep->>Dep: Effect skipped
            end
            Clear->>Planet: Blocker removed
            Planet->>Planet: Persist new state to save
        end
    end
```

### Ordered steps

1. **Prerequisite check.** The blocker's `prerequisites` identifies the removal technology. Clearing is unavailable until that technology is researched.
2. **Cost check.** The declared `resources.cost` must be affordable. Only `energy` is required, with `minerals` additionally required for Tarland.
3. **Duration.** Clearing occupies the blocker's `time` value — 180 or 270 days.
4. **District restoration.** Clearing reverses `planet_modifier.planet_max_districts_add`, returning one district for a `-1` blocker and two for a `-2` blocker.
5. **`on_cleared` execution.** Only Giant Earthworms and Sea Monsters declare this block.
6. **Persistence.** The engine writes the new planet state into the save game.

### The `on_cleared` effect in detail

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

| Element | Effect |
|---------|--------|
| `hidden_effect` | The grant does not appear as a visible tooltip line |
| `exists = owner` | Guards against unowned planets where the `owner` scope would be invalid |
| `has_civic` | Restricts the grant to devouring swarms |
| `add_resource` | Grants `food = 200` once |

The grant is a one-time event tied to clearing, not a recurring modifier. Because it is `hidden_effect`, a player who clears the blocker has no explicit notification that the food was granted.

## Asymmetries between the two processes

| Aspect | Placement | Clearing |
|--------|-----------|----------|
| Gate type | `potential` (hard) plus weight modifiers | `prerequisites` (hard) plus resource cost (hard) |
| Reversibility | Blocker persists until cleared | Permanent once cleared |
| Retuning effect | New planets only | Immediate for subsequent attempts |
| Player control | None — placement is automatic | Full — the player chooses when and whether |

## Where to go next

- Player-visible behaviour: [`../behaviour/research-and-clearing.md`](../behaviour/research-and-clearing.md)
- Technology weights: [`../configuration.md`](../configuration.md)
- Save-state implications: [`../state-and-persistence.md`](../state-and-persistence.md)
