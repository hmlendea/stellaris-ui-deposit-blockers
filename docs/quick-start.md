# Quick start

## Prerequisites

- Stellaris version `v4.5.*`, as declared by `supported_version` in both descriptors.
- A Stellaris installation with the `common/`, `gfx/`, and `localisation/` content directories.
- Write access to the game's mod directory for manual installation.

No compiler, runtime, package manager, or service is required.

## Install through a store

The mod is published on four channels. The `README.md` installation badges link to each:

| Channel | Link |
|---------|------|
| Steam Workshop | <https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612> |
| Paradox Mods | <https://mods.paradoxplaza.com/mods/34638/Any> |
| Nexus Mods | <https://www.nexusmods.com/stellaris/mods/91> |
| GitHub Releases | <https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases> |

Subscribing through Steam Workshop or adding the counterpart on Paradox Mods or Nexus Mods installs the mod into the launcher's mod list.

## Install manually

1. Download the latest release archive from GitHub Releases.
2. Extract the contents into the game's mod directory so that the `ui-deposit-blockers.mod` file and the `ui-deposit-blockers/` directory sit alongside the other installed mods.

The archive contents are ignored by `.gitignore` only when they carry the `.zip` extension; the repository itself stores the unpacked files.

## Enable the mod

1. Open the Stellaris launcher.
2. Add **Universum Infinitum - Deposit Blockers** to the active playset and enable it.
3. Start the game.

If the launcher rejects the mod, verify that `supported_version="v4.5.*"` in `ui-deposit-blockers.mod` matches your game version family — see [`troubleshooting.md`](troubleshooting.md).

## Confirm the mod is working

| Observation | Where to look |
|-------------|---------------|
| Blockers appear on eligible planets | Survey or colonise a dry or wet planet and inspect its blockers |
| Names are readable, not raw keys | The blocker tooltip should read e.g. `Ashland`, not `d_ui_ashland_blocker` |
| Icons render | Each blocker should show an icon instead of a blank placeholder |
| Removal technologies exist | The society research tree should offer e.g. `Ashland Removal` under the `new_worlds` category |
| Clearing restores districts | Clearing a `-1` blocker restores one maximum district; a `-2` blocker restores two |

A new game is required for placement changes to take effect; blockered planets generated before a change retain their original blockers.

## First development check

There is nothing to build. To confirm the definitions are mutually consistent, run the cross-check from [`testing.md`](testing.md):

```bash
for t in $(grep -o '^tech_d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt); do grep -q "prerequisites = { \"$t\" }" ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt || echo "unlinked: $t"; done
```

Silence means every technology is referenced as a deposit prerequisite.

## Where to go next

- Understand what the mod does and does not own: [`repository-overview.md`](repository-overview.md)
- Learn the file layout: [`repository-structure.md`](repository-structure.md)
- Change content safely: [`change-guide.md`](change-guide.md)
