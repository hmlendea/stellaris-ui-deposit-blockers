# Testing

## Status statement

There is no test suite, lint configuration, CI workflow, or build step in this repository. Verification is manual, static, and observational — the only automated gate visible to the mod author is a human running the cross-checks below and then playing the game. This is a consequence of the declarative architecture ([architecture.md](architecture.md)), not of an intentional decision to skip validation.

## Verification that runs entirely in-window

### 1. Deposit-to-technology prerequisite cross-check

Every technology identifier must appear as a deposit's `prerequisites`. Execute this shell loop against the repository root:

```bash
for t in $(grep -o '^tech_d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt); do grep -q "prerequisites = { \"$t\" }" ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt || echo "unlinked: $t"; done
```

**Pass criterion:** silence. Every line starting `unlinked:` identifies a technology no deposit references.

### 2. Icon cross-check

Every deposit identifier must have a matching `.dds` file in `gfx/interface/icons/deposits/`; every technology identifier must have one in `gfx/interface/icons/technologies/`.

```bash
for d in $(grep -o '^d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt); do test -f "ui-deposit-blockers/gfx/interface/icons/deposits/$d.dds" || echo "missing icon: $d"; done
for t in $(grep -o '^tech_d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt); do test -f "ui-deposit-blockers/gfx/interface/icons/technologies/$t.dds" || echo "missing icon: $t"; done
```

**Pass criterion:** silence.

### 3. Descriptor parity check

Compare the two descriptor files:

```bash
diff ui-deposit-blockers.mod ui-deposit-blockers/descriptor.mod
```

**Pass criterion:** silence ("descriptors identical"). Any output is a metadata drift route ready to fail at installation time.

### 4. Localisation key set check

Every definition identifier must produce exactly two keys (`name` and `_desc`) in the corresponding localisation table. This is a manual correlation rather than a single command:

1. List the seven deposit identifiers.
2. In `ui-tile-blockers_l_english.yml`, confirm `<identifier>` and `<identifier>_desc` both exist.
3. List the seven technology identifiers.
4. In `ui-tile-blockers_technologies_l_english.yml`, confirm `<identifier>` and `<identifier>_desc` both exist.

[`invariants.md`](invariants.md) invariant I8 covers this as a rule; there is no automated check.

## Verification that requires playing the game

Because every behavioural property is engine-owned, it cannot be verified from repository text alone:

| Test | Procedure | What it confirms |
|------|-----------|------------------|
| Load | Enable the mod in the launcher and start Stellaris | Descriptor parity, supported_version acceptance |
| Placement | Generate planets and survey dry and wet worlds | `potential` gates and weighted placement function |
| Names | Read blocker tooltips | Localisation key correctness |
| Icons | Inspect blocker and technology icons | Icon naming resolution |
| Technologies | Open the society tree | tier, category, prerequisites all resolve |
| Clearing | Research each technology, clear the matching blocker | prerequisite link, cost, duration, district restoration |
| Clearing tech gate | Attempt clearing before researching | Clearing unavailable until the prerequisite is researched |
| Early game | Play a one-planet empire | Apply the `num_owned_planets < 2` suppression |
| Devouring swarm | Clear Giant Earthworms or Sea Monsters as a hive | The `on_cleared` food grant fires |
| Save game | Load a save containing blockers after a minor change | Blockers persist as declared |

## Coverage gaps

These verifications are deliberately **not** possible from static repository evidence:

- **Drop frequency.** Weights are declared but never measured; see [A9](ambiguities-and-open-questions.md).
- **Engine-side settings changes across patches.** A game patch may rename vanilla predicates without changing `supported_version`; each patch requires a fresh load test.
- **Save-game removal risk.** Removing a blocker from the mod may strand references in existing saves — see [A8](ambiguities-and-open-questions.md).
- **Non-numeric XML/attribute semantics.** The interplay of `@`-variables with weekly or monthly game events is not observable from the repository.
- **Font and icon glyph availability.** The `£blocker` prefix may not render in locales other than English.

## Where to go next

- How to modify content safely: [`change-guide.md`](change-guide.md)
- Recorded invariants and how to check them: [`invariants.md`](invariants.md)
