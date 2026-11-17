# FAQ

## What does this mod do?

It adds seven planet deposit blockers and one society technology per blocker that unlocks its removal. Each blocker occupies a region of a planet and removes one or two maximum districts until cleared. Repositories structure: [`repository-overview.md`](repository-overview.md).

## Is there any code in this repository?

No. Every file is a text script, a localisation table, a `.mod` descriptor, or an image. Nothing is compiled or executed by the mod; the Stellaris engine reads and applies the declarations. See [`architecture.md`](architecture.md).

## Do I need to install anything else?

No. The mod is standalone and declares no dependency on any other mod, including the other Universum Infinitum mods listed in `README.md`.

## Which game version does it support?

`v4.5.*`, declared by `supported_version` in both descriptors. The wildcard accepts any 4.5 patch. Raising it requires editing both descriptor files identically.

## Why do blockers not appear on my small planets?

Every blocker applies a zero-weight modifier when the planet has fewer than two free city districts, so no blocker from this mod is placed on a cramped planet. Two blockers additionally require `planet_size >= 10`, and six of the seven apply a 0.5 penalty for planets smaller than 15. See [`behaviour/blocker-appearance.md`](behaviour/blocker-appearance.md).

## Why do I see `d_ui_lava_lake_blocker` instead of a name?

The localisation key did not resolve. Check that the key exists in `ui-tile-blockers_l_english.yml` and that the file still has its UTF-8 BOM. See [`components/localisation.md`](components/localisation.md).

## Why is a blocker icon blank?

The `.dds` file name does not equal the definition identifier. The engine resolves textures by file name only. See [`components/presentation-assets.md`](components/presentation-assets.md).

## Why can I not clear a blocker?

The prerequisite removal technology is not researched, or the declared resource cost is unaffordable. Tarland additionally requires minerals. See [`behaviour/research-and-clearing.md`](behaviour/research-and-clearing.md).

## Why does the removal technology never appear in my research options?

Three causes are possible: the empire owns fewer than two planets, no owned planet carries the matching blocker, or the empire is nomadic. All three conditions suppress the technology. See [`behaviour/research-and-clearing.md`](behaviour/research-and-clearing.md).

## Why did a technology appear suddenly mid-game?

Its weight is zero until the empire both owns two or more planets and controls a planet carrying the matching blocker. The moment both hold, the technology enters the pool.

## Why does clearing some blockers take longer than others?

Two blockers take 180 days and five take 270 days, declared by the deposit `time` field. Ashland and Canyon cost 1000 energy each; Floodplain, Giant Earthworms, Lava Lake, and Sea Monsters cost 500; Tarland costs 750 energy plus 250 minerals.

## Do cleared blockers ever come back?

No. Clearing is permanent, and no definition re-adds a cleared blocker.

## Do my changes to weights affect an existing campaign?

No. Placement is decided at planet-generation time, so weight and potential changes affect only planets generated afterwards.

## Do I need to start a new game for a new blocker to appear?

Yes for placement changes, because placement happens during generation. Technology weight changes affect subsequent research rollouts in an existing campaign.

## Why do all seven technologies appear in the same `new_worlds` category and use the same modifiers?

The family is designed as a coherent group: same area, same category, same prerequisite, and the same availability modifiers, differing only by tier and by the blocker they unlock. See [`components/technology-definitions.md`](components/technology-definitions.md).

## What is the `deposit_blockers` weight group for?

It makes the seven technologies compete as a family: once one member is researched, the remaining members have their weight halved. This spreads research instead of letting a player collect the entire family in sequence.

## Does the mod work in multiplayer?

The mod is data-only and has no network behaviour, so multiplayer compatibility depends on the game's own mod-matching rules. Clients with mismatched mod versions fail the checksum comparison. See [`build-and-deployment.md`](build-and-deployment.md).

## Is there anything to build or test?

No build step exists. Three cross-check scripts verify prerequisite linkage, icon existence, and descriptor parity; everything else requires playing the game. See [`testing.md`](testing.md).

## Where are the unresolved unknowns?

[`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) lists twelve open items, including the Ashland resource-category asymmetry, the Lava Lake tier, the Floodplain size-modifier omission, and the packet of unverified engine behaviours.

## Where is the documentation maintained?

See [`documentation-maintenance.md`](documentation-maintenance.md) for the inventory, update triggers, and style conventions.
