# Build and deployment

## Build

**Not applicable.** The repository has no compilation, bundling, transpilation, packaging, or asset-generation step. Distributable content is exactly what is committed:

| Content | Path |
|---------|------|
| Mod metadata | `ui-deposit-blockers.mod` |
| Mod payload | `ui-deposit-blockers/` (including its `descriptor.mod`) |

The `.gitignore` excludes only `*.zip`, the extension used by the archives that the workflows build, so a locally built archive cannot be committed accidentally.

## Deployment targets

The mod ships to four distribution channels, all evidenced in `README.md`:

| Channel | Identifier | Link |
|---------|------------|------|
| Steam Workshop | Item `2073660612` | <https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612> |
| Paradox Mods | Listing `34638` | <https://mods.paradoxplaza.com/mods/34638/Any> |
| Nexus Mods | Listing `91` | <https://www.nexusmods.com/stellaris/mods/91> |
| GitHub Releases | Repository releases | <https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases> |

There is no build step in the repository itself, but three deployment workflows package and publish the content: `.github/workflows/github-release.yml` builds the release archive and attaches it to the GitHub release, while `.github/workflows/nexus-mods-release.yml` and `.github/workflows/steam-workshop-release.yml` build their own archives and publish them to Nexus Mods and Steam Workshop. Paradox Mods remains manual.

## Automation

Three publications are automated and follow the same tag-triggered pattern as the other Universum Infinitum mods:

| Trigger | Behaviour |
|---------|-----------|
| Publication of a GitHub release | Builds `ui-deposit-blockers_<version>.zip` and attaches it to the release |
| Push of a `v*.*.*` tag | Builds and publishes that tag to Nexus Mods and Steam Workshop |
| Manual dispatch with a `tag` input (Nexus Mods only) | Publishes the supplied tag to Nexus Mods |

| Aspect | Nexus Mods | Steam Workshop | GitHub Release |
|--------|------------|----------------|----------------|
| Workflow file | `.github/workflows/nexus-mods-release.yml` | `.github/workflows/steam-workshop-release.yml` | `.github/workflows/github-release.yml` |
| Job | `release`, on `ubuntu-latest` | `release`, on `ubuntu-latest` | `build-and-upload`, on `ubuntu-latest` |
| Trigger | `v*.*.*` tag push or manual dispatch | `v*.*.*` tag push | Published GitHub release |
| Built artefact | `release.zip` containing `ui-deposit-blockers/` and `ui-deposit-blockers.mod` | `release.zip`, extracted to `release/ui-deposit-blockers/` | `ui-deposit-blockers_<version>.zip` containing `ui-deposit-blockers/` and `ui-deposit-blockers.mod` |
| Upload | `Nexus-Mods/upload-action@v1.0.0-beta.10`, `file_id` `955873`, category `main` | `hmlendea/steam-workshop-update@v1.0.0`, `appid` `281990`, `itemid` `2073660612` | `softprops/action-gh-release@v2`, which attaches the archive to the release |
| Required secrets | `NEXUSMODS_API_KEY` | `STEAM_USERNAME`, `STEAM_PASSWORD`, `STEAM_2FA_SEED` | `GITHUB_TOKEN` (provided automatically) |
| Version handling | `VERSION` is the tag, and the workflow checks out that ref before packaging | Archive base name is fixed; the tag is used only for the changelog link | `VERSION` is the release tag with the leading `v` stripped for the archive name |

The `nexus-mods-release.yml` and `steam-workshop-release.yml` runs each check out the tagged content and build their own archive, so the tag itself is sufficient; only `github-release.yml` attaches its archive to the GitHub release. Publication to Paradox Mods remains manual.

## Publishing procedure

1. Confirm the change set is complete across all coupled surfaces — see [`change-guide.md`](change-guide.md).
2. Run the cross-checks in [`testing.md`](testing.md) against the tagged contents.
3. Raise `supported_version` in both descriptors if targeting a new game-version family; confirm `diff` is clean.
4. Tag the release in Git and publish a GitHub release; `.github/workflows/github-release.yml` builds the archive and attaches it to that release.
5. Let `.github/workflows/nexus-mods-release.yml` and `.github/workflows/steam-workshop-release.yml` publish the tag to Nexus Mods and Steam Workshop, and upload the same content set manually to Paradox Mods so that no channel drifts.

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
| Manual | Extract a release archive (for example `ui-deposit-blockers_<version>.zip` from GitHub Releases) into the game's mod directory so `ui-deposit-blockers.mod` and `ui-deposit-blockers/` sit alongside other installed mods |

## Rollback

There is no documented rollback procedure. Reverting a bad publication means re-publishing the previously tagged content set to every channel, because the channels store independent copies and none of them reads the Git tag directly.

## Where to go next

- Versioning and descriptor contract: [`configuration.md`](configuration.md)
- Verification before publishing: [`testing.md`](testing.md)
- Distribution dependency implications: [`dependencies.md`](dependencies.md)
