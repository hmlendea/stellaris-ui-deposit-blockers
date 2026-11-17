# Design decisions

Each decision below is recorded only where repository evidence supports it. Engine-side rationale that could not be established from the repository is recorded in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) instead.

## D1: Declarative content with no executable code

**Decision:** The mod consists purely of engine-parsed text and image files.

**Evidence:** No source files, build files, scripts, or validation workflows exist in the repository; the workflows only package and publish releases.

**Rationale:** A blocker mod has no computation to perform; the engine already owns placement, arbitration, and clearing. Adding code would require a different extension mechanism and increase the failure surface.

**Consequence:** Contributor verification is limited to static cross-checks and in-game observation. There is nothing to unit-test, and a malformed block surfaces only as missing content.

**Alternative rejected:** A scripted-effect or on-action based implementation. Not evidenced; would add runtime coupling for no functional gain.

## D2: Mod-namespaced identifiers with a `ui` infix

**Decision:** Every identifier carries the `ui` token: `d_ui_<name>_blocker`, `tech_d_ui_<name>_blocker`.

**Evidence:** All fourteen identifiers follow the pattern.

**Rationale:** The engine merges identifiers from all active mods into a shared namespace. A distinctive infix prevents accidental collision with vanilla or third-party definitions.

**Consequence:** Identifiers are verbose but load-order independent. Any rename must be applied across five surfaces (see [I10](invariants.md)).

## D3: One technology per blocker, tiered by severity

**Decision:** Each blocker has exactly one dedicated removal technology. Blockers removing `-2` districts are tier 2; blockers removing `-1` districts are tier 1, with two exceptions described in D4.

**Evidence:** `d_ui_giant_earthworms_blocker` and `d_ui_sea_monsters_blocker` remove `-2` and are tier 2; the remaining five remove `-1`. Four of those five are tier 1.

**Rationale:** Tiering aligns research effort with the severity of the obstruction.

**Consequence:** Removing a blocker's technology is not a local change; it breaks [I6](invariants.md).

## D4: Tier does not perfectly track district severity

**Decision:** `tech_d_ui_lava_lake_blocker` is tier 2 although its blocker removes only `-1` district.

**Evidence:** `tier = 2` and `cost = @tier2cost2` in the technology file; `planet_max_districts_add = -1` in the deposit file.

**Rationale:** Not established from repository evidence. The blocker's low base weight (`@blocker_medium`) and its 180-day clearing time are comparable to tier 1 members, so the tier choice is not explained by weight or duration.

**Consequence:** Recorded as an intentional-or-accidental asymmetry in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md). Changing it would alter research pacing.

## D5: Three shared drop-weight baselines instead of per-blocker literals

**Decision:** Placement weights are expressed as `@blocker_high` (10), `@blocker_medium` (7), and `@blocker_low` (5), declared once at the top of the deposits file.

**Evidence:** The header of `common/deposits/ui-deposit-blockers.txt`.

**Rationale:** Retuning the colour of the whole family becomes a three-line change rather than a seven-block edit, and the relative ordering remains explicit.

**Consequence:** A single edit silently retunes every referring blocker. The indirection must be documented for contributors, which [`change-guide.md`](change-guide.md) does.

## D6: A single shared technology weight group with `0.5` decay

**Decision:** All seven technologies join the `deposit_blockers` group with `mod_weight_if_group_picked = { deposit_blockers = 0.5 }`.

**Evidence:** Identical blocks in all seven technology definitions.

**Rationale:** Without a shared group, each technology competes independently and an empire could research the entire family in an unnatural sequence. The decay spreads selection across the family.

**Consequence:** Adding an eighth technology mechanically reduces every existing member's share unless the decay constant is revisited. This is a non-obvious coupling recorded in [`change-guide.md`](change-guide.md).

## D7: Suppress technologies the empire cannot use

**Decision:** Every technology zeroes its weight when no owned planet carries the blocker, and again when the empire owns fewer than two planets. All seven also require `is_nomadic = no`.

**Evidence:** `weight_modifier` and `ai_weight` blocks in every technology.

**Rationale:** Prevents the research pool from being diluted by dead options, and prevents nomads — who do not colonise planets conventionally — from receiving unusable technologies.

**Consequence:** A player who conquers a planet late in a campaign can see the technology appear abruptly as its weight changes from zero.

## D8: Discourage placement on small planets and reserve blockers for planets with room

**Decision:** Six of seven blockers apply `factor = 0.5` when `planet_size < 15`, and all seven apply `factor = 0` when the planet has fewer than two free city districts.

**Evidence:** `drop_weight` blocks; `grep -c 'planet_size < 15'` returns six; `num_free_districts` appears seven times. `d_ui_floodplain_blocker` omits the size modifier.

**Rationale:** A blocker removes maximum districts, so placing one on a cramped planet would be disproportionately punishing. The free-district guard is the stronger protection and is applied universally.

**Consequence:** The free-district guard restores parity between blockers, but the size modifier is applied inconsistently — Floodplain has none. Recorded in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md).

## D9: Planet-class modifiers differentiate blockers within a climate family

**Decision:** Each blocker with a climate flavour adds a `factor = 2` or `factor = 3` modifier favouring the planet classes that match its theme.

**Evidence:** Ashland favours `pc_nuked` (factor 3) and `pc_alpine` (2); Canyon favours `pc_arid` and `pc_savannah` (2); Floodplain favours `pc_ocean` and `pc_continental` (2); Lava Lake and Tarland favour `pc_desert` and `pc_arid` (2); Sea Monsters favours `pc_ocean` (3).

**Rationale:** Thematic coherence: a lava lake belongs on an arid world, sea monsters on an ocean world.

**Consequence:** Lava Lake and Tarland share an identical class-favouring rule, so on desert and arid worlds they compete directly.

## D10: Two blockers grant food to devouring swarms on clearing

**Decision:** `d_ui_giant_earthworms_blocker` and `d_ui_sea_monsters_blocker` include an `on_cleared` hidden effect granting `food = 200` when the owner has `civic_hive_devouring_swarm`.

**Evidence:** Identical `on_cleared` blocks in both deposits.

**Rationale:** Thematically these blockers are biological ("giant earthworms", "sea monsters"), so clearing them yields biomass for a hive that consumes organic matter.

**Consequence:** The effect is gated on a vanilla civic identifier; if that civic is renamed in a future game version, the food grant silently disappears while the blocker remains clearable.

## D11: Universal clearing cost floors at 500 energy, with one mineral component

**Decision:** All blockers cost energy; three cost 1000 and four cost 500, except Tarland which costs 750 energy plus 250 minerals.

**Evidence:** `resources.cost` blocks.

**Rationale:** Not fully established. Tarland's mineral component is unique in the family and is not explained by its weight (medium) or duration (270).

**Consequence:** Recorded in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md). It is the only blocker whose clearing cost consumes a second resource.

## D12: Duplicated descriptor files

**Decision:** Mod metadata exists twice, as `ui-deposit-blockers.mod` at the repository root and `ui-deposit-blockers/descriptor.mod` inside the content directory.

**Evidence:** Both files exist and are byte-identical.

**Rationale:** Distribution channels differ in whether they read the metadata from beside the content directory or from the parent of a `mod/`-relative path. Keeping both satisfies every channel.

**Consequence:** Any metadata edit must be applied twice. [I1](invariants.md) makes this checkable with a single `diff`.

## D13: No validation workflow, publication workflows only

**Decision:** The repository defines only publication workflows, which package and publish to GitHub Releases, Nexus Mods, and Steam Workshop rather than validating content.

**Evidence:** `.github/workflows/` contains `github-release.yml`, `nexus-mods-release.yml`, and `steam-workshop-release.yml`; each job builds the release archive from the tagged content and uploads it with `softprops/action-gh-release@v2`, `Nexus-Mods/upload-action@v1.0.0-beta.10`, and `hmlendea/steam-workshop-update@v1.0.0` respectively.

**Rationale:** No automated check is possible without a Stellaris installation, and the repository has no lint or build step, so a validation workflow cannot be expressed. Publication, by contrast, is a mechanical upload that does not require the engine.

**Consequence:** Validity still depends on manual review, and [`testing.md`](testing.md) provides repository-runnable cross-checks that a future validation workflow could adopt. GitHub Releases, Nexus Mods, and Steam Workshop publication become tag-driven while Paradox Mods stays manual, which widens the channel-drift window unless the manual uploads accompany every tag.

**Alternative rejected:** Automating the Paradox Mods upload in the same change. It needs its own identifiers and secrets, and none is evidenced for this repository yet.

## D14: English-only localisation

**Decision:** Only `english` is provided.

**Evidence:** `localisation/english/` is the sole locale directory.

**Rationale:** Not established. The engine falls back to the raw key when a locale has no entry, so other locales display identifiers.

**Consequence:** Non-English players see raw keys for these blockers unless the game's locale falls back to English. Recorded in [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md).

## Where to go next

- Open questions and unverified behaviour: [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md)
- Applying changes safely: [`change-guide.md`](change-guide.md)
