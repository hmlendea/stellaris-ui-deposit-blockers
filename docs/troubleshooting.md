# Troubleshooting

## Diagnosis table

| Symptom | Probable cause | Resolution |
|---------|----------------|------------|
| Mod is absent from the launcher's playset | `supported_version` does not match the installed game family | Raise `supported_version` in *both* descriptors, or install a matching game version |
| Mod loads but no blocker from this mod ever appears | `potential` predicates do not match the planets being generated, or the planets have fewer than two free city districts | Verify the climate gates in the deposits file, and check planets with at least two free city districts |
| A blocker appears but its name is a raw identifier (`d_ui_…`) | The localisation key is missing or misspelled | Add or fix the `<identifier>:0` key in `ui-tile-blockers_l_english.yml` |
| A blocker's description is missing | The `_desc` key is missing | Add `<identifier>_desc:0` |
| A technology shows a raw identifier | The technology localisation key is missing | Check `ui-tile-blockers_technologies_l_english.yml` |
| Localisation changes have no effect in game | Strings are cached for the session | Restart the game |
| A blocker icon renders blank | The `.dds` file name does not equal the identifier | Rename the file to match exactly |
| A technology icon renders blank | Same, in `gfx/interface/icons/technologies/` | Rename the file |
| The blocker cannot be cleared and no cost is shown | The prerequisite technology is undefined or unresolvable | Run the prerequisite cross-check in [`testing.md`](testing.md) |
| The removal technology never appears in the society tree | The empire owns fewer than two planets, or no owned planet carries the blocker, or the empire is nomadic | Acquire a second planet that carries the blocker; nomads can never receive these technologies |
| The technology appears only once, then never again | The `deposit_blockers` group decay of `0.5` halves the remaining members after the first is researched | Expected behaviour |
| All seven technologies appear simultaneously | The group declaration or decay is missing from some blocks | Verify `weight_groups` and `mod_weight_if_group_picked` on all seven |
| Clearing is unaffordable although the technology is researched | Insufficient energy (and, for Tarland, minerals) | Accumulate the declared `resources.cost` |
| A cleared blocker did not restore the expected number of districts | A `-1` blocker restores one district, a `-2` blocker restores two | Confirm which blocker was cleared |
| A devouring swarm did not receive food after clearing | Only Giant Earthworms and Sea Monsters grant food, and only to `civic_hive_devouring_swarm` owners | Expected behaviour |
| Changes to weights have no effect | Placement is decided at planet-generation time | Generate a new game |
| A planet in an old save lost its blocker after an update | A blocker was removed or renamed in the mod files | See [`state-and-persistence.md`](state-and-persistence.md) and [A8](ambiguities-and-open-questions.md) |
| The blocker appears on a planet that already has very few free districts | The free-district guard may be evaluated before or after other blockers are placed | See [A11](ambiguities-and-open-questions.md) |

## Diagnosing from the engine log

The mod writes nothing to the log. The engine may nonetheless emit parse diagnostics. Check the game log for:

| Log signal | Interpretation |
|------------|----------------|
| Script parse error naming a `d_ui_*` or `tech_d_ui_*` block | The block is malformed; the definition is not loaded |
| Unknown identifier warning for `tech_colonization_1`, `new_worlds`, `tr_expansion_adopt`, `ap_mastery_of_nature`, `civic_hive_devouring_swarm`, or a `pc_*` class | A vanilla identifier changed; the corresponding reference no longer resolves |
| Unknown variable for `@tier*cost2` or `@tier*weight2` | The vanilla scripted variable was renamed or removed |
| Localisation key warning | A key is missing or the file encoding changed |

See [`logging.md`](logging.md) for the full signal catalogue and the limits of mod-side observability.

## Isolating the cause

1. **Reproduce without other mods.** Disable all other mods and start a new game. Identifier collisions and load-order effects then disappear from the candidate causes.
2. **Check the descriptors.** Run `diff` on the two `.mod` files; a divergence explains installation-level symptoms.
3. **Run the three cross-checks.** They eliminate the three most common silent failures: an unlinked prerequisite, a misnamed icon, and a descriptor divergence.
4. **Test one blocker.** Restrict investigation to a single blocker — for example verify Ashland appears, is named, shows an icon, has its technology, and clears correctly. If a single blocker works end to end, the mechanism is sound and the fault is data-specific.
5. **Check the game log.** A parse error localises the fault to a file and block.

## Where to go next

- Invariants that a fault may have violated: [`invariants.md`](invariants.md)
- Verification commands: [`testing.md`](testing.md)
- Unresolved doubts: [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md)
