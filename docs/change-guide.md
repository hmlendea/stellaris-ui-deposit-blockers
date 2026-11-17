# Change guide

## How to modify content safely

All edits are made to static text files inside `ui-deposit-blockers/`; there is nothing to compile. The sole risk is introducing an inconsistency that surfaces silently in game. The procedure below is the minimum safe sequence.

### Before editing

1. Confirm which surface(s) an identifier spans — deposit identifier, technology identifier, two localisation keys, and two icon file names — using [`invariants.md`](invariants.md).
2. Decide whether the change is global or per-blocker:
   - Global: edit the three `@blocker_*` baselines once (affects every deposit that references one).
   - Per-blocker: edit both files in lockstep.

### Adding a blocker and its technology

1. Add `d_ui_<name>_blocker` to `ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt`, keeping field order: `time`, `is_for_colonizable`, `category`, `resources`, `potential`, `planet_modifier`, optional `all_blocker_swap_types` or `on_cleared`, `drop_weight`, `prerequisites`.
2. Add `tech_d_ui_<name>_blocker` to `ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt`, keeping tier alignment (`tier`, `cost`, `weight`), `area = society`, `category = { new_worlds }`, `prerequisites`, `weight_groups = { deposit_blockers }`, `mod_weight_if_group_picked`, and the modifier blocks.
3. Add four localisation keys (`name` and `_desc` per file) and two `.dds` icons.
4. Run the cross-checks in [`testing.md`](testing.md).
5. Commit all five surfaces plus update in the same commit.

### Retuning a blocker

| What you want | Safe edit |
|---------------|-----------|
| Make a blocker more or less common | Change its `weight` baseline references (`@blocker_high`/`_medium`/`_low`) or edit the baselines themselves |
| Make a blocker cheaper/faster to clear | Edit its `resources.cost` and `time` in the deposits file |
| Restore more districts on clearing | Edit `planet_max_districts_add` (must remain `-1` or `-2` per invariant I16) |
| Change which planet classes a blocker prefers | Edit the `drop_weight` class modifiers |
| Make a blocker removable at a different tier | Edit its technology block's tier plus `cost` and `weight` variables together |

### Restrictions to observe

| Action | May do | Must not do |
|--------|--------|--------------|
| Rename a blocker | With a commit touching all five surfaces | Rename in one file only — see invariant I10 |
| Remove a blocker | Commit both definitions and all surfaces | Expect existing save games to migrate cleanly — see [A8](ambiguities-and-open-questions.md) |
| Edit `all_blocker_swap_types` | Yes — on any `-1` blocker | Add it to any `-2` blocker — the two `-2` blockers deliberately omit it |
| Change tiers | Move a blocker between tiers | Leave `cost` and `weight` mismatched with the new tier — invariant I21 |
| Change developer baseline values | Edit all occurrences of one baseline | Expect neighbouring blockers' frequencies to remain untouched — they won't |
| Add a new technology to the group | Apply `deposit_blockers` with decay `0.5` | Use a different decay for one member — invariant I22 |

### After editing

Run every cross-check command in [`testing.md`](testing.md), then verify in game:

1. Load the mod successfully.
2. Generate planets and confirm the new blocker appears with a name and icon.
3. Research the new technology and confirm its appearance in the society tree.
4. Clear the blocker and confirm cost, duration, restoration, and any `on_cleared` effect.

Commit only after both steps pass.

## Descriptor edits

Version bumps (`supported_version`) and tag changes must be applied to both descriptors identically. Run `diff ui-deposit-blockers.mod ui-deposit-blockers/descriptor.mod` before committing.

## Release process

There is no release tooling. To publish:

1. Update both descriptors' `supported_version` if targeting a new game-version family.
2. Tag the release.
3. Upload the repository contents to each distribution channel (Steam Workshop, Paradox Mods, Nexus Mods, GitHub Releases).
4. Run the cross-checks against the tagged contents, not just the working tree.

## Where to go next

- Rules that edits must preserve: [`invariants.md`](invariants.md)
- Verified examples: [`api-usage-examples.md`](api-usage-examples.md)
- Unsafe or unverified edits: [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md)
