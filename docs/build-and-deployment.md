# Build and deployment

## Build

**Not applicable.** The repository has no compilation, bundling, transpilation, packaging, or asset-generation step. Distributable content is exactly what is committed:

| Content | Path |
|---------|------|
| Mod metadata | `ui-deposit-blockers.mod` |
| Mod payload | `ui-deposit-blockers/` (including its `descriptor.mod`) |

The `.gitignore` excludes only `*.zip`, which is the extension used by hand-assembled release archives, so a packaged archive cannot be committed accidentally.

## Deployment targets

The mod ships to four distribution channels, all evidenced in `README.md`:

| Channel | Identifier | Link |
|---------|------------|------|
| Steam Workshop | Item `2073660612` | <https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612> |
| Paradox Mods | Listing `34638` | <https://mods.paradoxplaza.com/mods/34638/Any> |
| Nexus Mods | Listing `91` | <https://www.nexusmods.com/stellaris/mods/91> |
| GitHub Releases | Repository releases | <https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases> |

There is no deployment automation, no scripting, and no CI pipeline.

## Publishing procedure

1. Confirm the change set is complete across all coupled surfaces — see [`change-guide.md`](change-guide.md).
2. Run the cross-checks in [`testing.md`](testing.md) against the tagged contents.
3. Raise `supported_version` in both descriptors if targeting a new game-version family; confirm `diff` is clean.
4. Tag the release in Git.
5. Upload the same content set to each channel so that no channel drifts.

Channel drift risk is the material operational hazard: each channel stores an independent copy, and a channel updated later than the others serves a snapshot of an older definition set. See [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) A12 for the absent documented release process.

## Release history

| Version | Published | Notes |
|---------|-----------|-------|
| `v1.0` | 2020-04-25 | Initial release |
| `v1.0.1` | 2021-12-15 | Latest GitHub release |

Later repository commits (Stellaris 3.2 and 4.5 updates) carry no release tag, so the repository content is ahead of the newest published GitHub release. Players installing from a store receive whatever the channel hosts, which may lag the repository.

## Runtime environment

| Requirement | Value |
|-------------|-------|
| Game version | `v4.5.*` |
| Operating systems | All operating systems supported by the game (declared as OS-agnostic in `README.md`) |
| Tested platform | Linux, native build or Steam Play |
| Network access at runtime | None |
| Persistent state | The save game only |
| Privileged ports or files | None |

## Installation routes

| Route | Procedure |
|-------|-----------|
| Steam Workshop | Subscribe through the Workshop |
| Paradox Mods | Add the listing to the playset |
| Nexus Mods | Install through the Nexus launcher or manually |
| Manual | Extract the release archive into the game's mod directory so `ui-deposit-blockers.mod` and `ui-deposit-blockers/` sit alongside other installed mods |

## Rollback

There is no documented rollback procedure. Reverting a bad publication means re-publishing the previously tagged content set to every channel, because the channels store independent copies and none of them reads the Git tag directly.

## Where to go next

- Versioning and descriptor contract: [`configuration.md`](configuration.md)
- Verification before publishing: [`testing.md`](testing.md)
- Distribution dependency implications: [`dependencies.md`](dependencies.md)
