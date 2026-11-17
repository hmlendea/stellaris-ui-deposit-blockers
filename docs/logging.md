# Logging

## Status statement

The mod emits **no logs**. It has no logging framework, no log levels, no structured fields, no correlation identifiers, no sinks, no sampling, and no retention policy, because it contains no executable code. Everything in this document therefore describes what is observable through the **game's own log output**, not through any mod-provided facility.

## Why there is no mod-side logging

Stellaris mods of the declarative kind have no hook into a logging subsystem. A script definition is read by the engine; it cannot call a logger. Adding logging would require a different extension mechanism entirely, which the architecture deliberately avoids (see [D1](design-decisions.md)).

## Engine log signals relevant to this mod

When the engine encounters a problem in the mod's files, it may emit a diagnostic into its own log. The following classes of signal are relevant. The exact wording is engine-version dependent and is not reproduced here.

| Signal class | Likely cause | Action |
|--------------|--------------|--------|
| Script parse error naming a `d_ui_*_blocker` block | Malformed deposit syntax | Inspect the named block for a missing brace or invalid field |
| Script parse error naming a `tech_d_ui_*_blocker` block | Malformed technology syntax | Same |
| Unknown identifier for `tech_colonization_1` | A vanilla technology was renamed or removed | Update the prerequisite in all seven technology blocks |
| Unknown identifier for `new_worlds` | A vanilla technology category changed | Update `category` in all seven technology blocks |
| Unknown identifier for `deposit_cat_blockers` | A vanilla deposit category changed | Update `category` in all seven deposit blocks |
| Unknown identifier for `tr_expansion_adopt` | A vanilla tradition changed | Update the modifier in the technology blocks |
| Unknown identifier for `ap_mastery_of_nature` | A vanilla ascension perk changed | Update the modifier in the technology blocks |
| Unknown identifier for `civic_hive_devouring_swarm` | A vanilla civic changed | Update the `on_cleared` gate on two deposits |
| Unknown identifier for `leader_trait_expertise_new_worlds` | A vanilla leader trait changed | Update both weight paths in all seven technologies |
| Unknown identifier for a `pc_*` planet class | A vanilla planet class changed | Update the class modifiers in the affected deposits |
| Unknown variable for `@tier*cost2` or `@tier*weight2` | A vanilla scripted variable changed | Override the variables in the technology file |
| Localisation key warning | A key is missing or the file encoding lost its BOM | Verify the key and the encoding |

## What the log will not tell you

| Silent failure | Why it is silent |
|----------------|------------------|
| Missing or misnamed icon | Textures resolve by file-name convention with no identity check |
| Missing localisation key | Falls back to displaying the raw identifier |
| Unresolvable `prerequisites` | The blocker remains placeable; the failure surfaces only as an unclearable blocker |
| A modifier referring to a renamed vanilla identifier | The modifier becomes inert without necessarily raising a parse error |
| A blocker removed from the mod while present in a save | See [A8](ambiguities-and-open-questions.md); unverified |

## Practical diagnostic workflow

1. Enable only this mod and start the game to remove identifier-collision noise.
2. Open the engine log and search for `ui-deposit-blockers` or `ui_`.
3. For each script error, resolve against the corresponding file and block.
4. For each unknown-identifier warning, cross-reference [`dependencies.md`](dependencies.md) to identify which declaration is affected.
5. For symptoms with no log entry, use the diagnosis table in [`troubleshooting.md`](troubleshooting.md) and the cross-checks in [`testing.md`](testing.md).

## Observability gaps

| Gap | Consequence |
|-----|-------------|
| No placement logging | Drop frequency cannot be measured; see [A9](ambiguities-and-open-questions.md) |
| No technology-roll logging | Availability weighting cannot be measured |
| No identifier-coupling diagnostics | Missing keys and icons require external checks |
| No version-drift detection | A vanilla rename that does not produce a warning is undetectable until observed in play |

## Where to go next

- Failure semantics: [`error-handling.md`](error-handling.md)
- Diagnosis table: [`troubleshooting.md`](troubleshooting.md)
- Verifiable signals: [`testing.md`](testing.md)
