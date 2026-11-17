# Ambiguities and open questions

Each entry states what is unknown, what evidence exists, what would resolve it, and the impact if the ambiguity is a defect.

## A1: `resources.category` present on only one deposit

**Unknown:** Whether the missing `category = deposit_blockers` inside `resources` on six deposits changes clearing cost behaviour.

**Evidence:** `d_ui_ashland_blocker` declares:

```
resources = {
    category = deposit_blockers
    cost = { energy = 1000 }
}
```

The other six declare only `cost`:

```
resources = {
    cost = { energy = 500 }
}
```

**Grep result:** `category = deposit_blockers` occurs exactly once in the file; `category = deposit_cat_blockers` (the deposit-level category) occurs seven times.

**Why it matters:** If the resources category affects which resource pool pays the cost or how the tooltip displays it, then Ashland behaves differently from the other six. If it is redundant, it is dead script.

**Resolution path:** Inspect the vanilla deposit definitions for blockers with an energy cost, or observe the clearing tooltip for Ashland versus Canyon in game.

**Status:** Tracked, not asserted as a defect. Recorded in the root [`ARCHITECTURE.md`](../ARCHITECTURE.md) as an observed inconsistency.

## A2: Lava Lake tier does not match its district severity

**Unknown:** Why `tech_d_ui_lava_lake_blocker` is tier 2 when its blocker removes only `-1` district.

**Evidence:** `tier = 2`, `cost = @tier2cost2`, `weight = @tier2weight2` in the technology file; `planet_max_districts_add = -1` and `weight = @blocker_medium` in the deposit file. Compare `d_ui_canyon_blocker`, which also removes `-1` at `@blocker_high` but is tier 1.

**Why it matters:** Tier determines research cost. A tier 2 gate for a single-district blocker may make the blocker economically pointless in the early game, or may be deliberate pacing.

**Resolution path:** Maintainer intent, or a comparison of measured clearing benefit against research cost.

**Status:** Tracked as an intentional-or-accidental asymmetry (see [D4](design-decisions.md)).

## A3: Floodplain omits the small-planet weight modifier

**Unknown:** Whether the absence of `factor = 0.5` when `planet_size < 15` on `d_ui_floodplain_blocker` is deliberate.

**Evidence:** `grep -c 'planet_size < 15'` returns six for seven deposits. Floodplain is the only omission.

**Why it matters:** Floodplain already has the highest base weight (`@blocker_high`) and no size penalty, so it will appear disproportionately often on small wet planets compared with Canyon on small dry planets.

**Resolution path:** Maintainer intent.

**Status:** Tracked. See [D8](design-decisions.md).

## A4: Tarland's unique mineral cost and unique class-favour pair

**Unknown:** Why Tarland costs 750 energy plus 250 minerals while every other blocker costs energy only.

**Evidence:** Tarland is the only deposit with a `minerals` entry. Its class modifiers (`pc_desert`, `pc_arid` at factor 2) are identical to Lava Lake's.

**Why it matters:** The mineral component makes Tarland unaffordable in some early-game economies where the other blockers are affordable, which may or may not be intended to differentiate it.

**Resolution path:** Maintainer intent.

**Status:** Tracked.

## A5: Whether localisation falls back to English for other locales

**Unknown:** Whether a player running a non-English locale sees the English strings or raw identifiers.

**Evidence:** Only `localisation/english/` exists. The two files both carry a BOM, and the technology descriptions embed trailing newlines.

**Why it matters:** If there is no fallback, non-English players see `d_ui_ashland_blocker` instead of `Ashland`, which would be a visible defect rather than a stylistic gap.

**Resolution path:** Observe the mod with the game language set to a non-English locale.

**Status:** Tracked. Recorded in [D14](design-decisions.md).

## A6: Whether `deposit_blockers` as a resource category collides with vanilla

**Unknown:** Whether the identifier `deposit_blockers` is defined in the base game or is mod-invented.

**Evidence:** The identifier appears only inside `d_ui_ashland_blocker`'s `resources` block. No local definition exists in the repository.

**Why it matters:** If it is mod-invented and undefined, the Ashland resource block may be partially ignored, which would explain why the other six omit it.

**Resolution path:** Search the vanilla script files for a `deposit_blockers` resource category definition.

**Status:** Tracked.

## A7: Engine behaviour for a deposit whose prerequisite technology is undefined

**Unknown:** Whether an unresolvable `prerequisites` value renders the blocker permanently uncleareable, silently ignores the gate, or raises a parse error.

**Evidence:** All seven references currently resolve. No test of the failure mode exists.

**Why it matters:** This determines the severity of a renamed or removed technology: a cosmetic `unlinked:` output from the cross-check script could correspond to a blocker that can never be cleared.

**Resolution path:** Deliberately introduce a broken prerequisite in a disposable local install and observe.

**Status:** Tracked. The cross-check in [`testing.md`](testing.md) is the mitigation.

## A8: Save-game behaviour when a blocker is removed from the mod

**Unknown:** Whether a save containing a planet with a now-undefined blocker loads cleanly, drops the blocker, or blocks loading.

**Evidence:** No migration logic or versioning exists in the repository.

**Why it matters:** Blocker removal is an apparently safe repository edit that may have campaign-level consequences.

**Resolution path:** Observe a save game created before the change and loaded after.

**Status:** Tracked. See [`state-and-persistence.md`](state-and-persistence.md).

## A9: Actual drop and research frequencies

**Unknown:** Whether the baselines and modifiers produce the perceived variety the mod intends.

**Evidence:** Weights and modifiers are declared but never measured. No instrumentation is possible without an engine-side logging facility.

**Why it matters:** `@blocker_high` (10) versus `@blocker_low` (5) is only a 2:1 ratio before modifiers, and the shared technology group decay of 0.5 compresses the family further. Whether the differentiators are perceptible is unknown.

**Resolution path:** Extended play observation, or engine log inspection if deposit selection is logged.

**Status:** Tracked. Recorded as a coverage gap in [`testing.md`](testing.md).

## A10: Whether the `ui` namespace token is guaranteed collision-free

**Unknown:** Whether any vanilla or popular third-party definition uses the `ui` token in a way that could collide.

**Evidence:** No collision has been reported, and the mod has been published since 2020. Removing the concern entirely would require an exhaustive search of the vanilla and ecosystem namespaces.

**Why it matters:** A collision would silently override another definition depending on load order.

**Resolution path:** Namespace survey.

**Status:** Tracked; low risk given the `d_ui_*_blocker` and `tech_d_ui_*_blocker` specificity.

## A11: Whether `num_free_districts` is evaluated before or after other blockers are placed

**Unknown:** The evaluation order when multiple blockers are candidates for the same planet.

**Evidence:** Every deposit applies `factor = 0` when `num_free_districts < 2`, but the engine's sequencing is not observable from the repository.

**Why it matters:** If evaluations are simultaneous, a planet could receive two blockers that together exceed its free districts before either guard fires.

**Resolution path:** Engine documentation or observation of planets with multiple blockers relative to their size.

**Status:** Tracked.

## A12: No documented support or release process

**Unknown:** Whether a formal release process, support window, or deprecation policy exists.

**Evidence:** No `SUPPORT.md`, `CHANGELOG.md`, `ROADMAP.md`, or versioning statement. `SECURITY.md` nonetheless lists all four distribution channels as supported for the latest version.

**Why it matters:** Contributors cannot determine how to propose a release or how long a version is supported.

**Resolution path:** Maintainer statement.

**Status:** Tracked.

## Where to go next

- Decisions that explain settled behaviour: [`design-decisions.md`](design-decisions.md)
- Verification gaps: [`testing.md`](testing.md)
- Engine-owned state questions: [`state-and-persistence.md`](state-and-persistence.md)
