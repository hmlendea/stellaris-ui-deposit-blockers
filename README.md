[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases/latest)
[![Website](https://img.shields.io/badge/Website-Visit-blue)](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/blob/master/LICENSE)

# Universum Infinitum - Deposit Blockers

Universum Infinitum - Deposit Blockers is a declarative Stellaris mod that adds seven planet deposit blockers and one prerequisite society technology per blocker, increasing the variety of blocked districts across the galaxy.

## 📑 Table of Contents

- [Table of Contents](#-table-of-contents)
- [Demo](#-demo)
- [Capabilities](#-capabilities)
- [Usage](#-usage)
- [Known Limitations](#-known-limitations)
- [Installation](#-installation)
  - [Manual Installation](#manual-installation)
  - [Verification](#verification)
  - [Uninstallation](#uninstallation)
- [Localisation](#-localisation)
- [Compatibility](#-compatibility)
- [Privacy and Data](#-privacy-and-data)
- [Development](#-development)
  - [Requirements](#requirements)
  - [Setup](#setup)
  - [Test](#test)
  - [Release](#release)
- [Project Structure](#-project-structure)
  - [Directories](#directories)
- [Architecture](#-architecture)
- [Documentation](#-documentation)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [Code of Conduct](#-code-of-conduct)
- [Related Projects](#-related-projects)
- [Security](#-security)
- [Support](#-support)
- [Project Engagement](#-project-engagement)
- [License](#-license)

## 🎬 Demo

![Universum Infinitum - Deposit Blockers cover image](.github/ui_cover.png)

## ✨ Capabilities

- Adds seven planet deposit blockers: `Ashland`, `Canyon`, `Floodplain`, `Giant Earthworms`, `Lava Lake`, `Sea Monsters`, and `Tarland`.
- Adds one society technology per blocker, in tiers 1 and 2, that unlocks the removal of the matching blocker.
- Restores one maximum district when a `-1` blocker is cleared and two when a `-2` blocker is cleared.
- Gates placement by climate, planet size, and free district count, so blockers appear only on plausible worlds.
- Presents every blocker and technology with its own icon, display name, and description.
- Keeps the seven technologies in a shared `deposit_blockers` weight group, so they are researched gradually instead of all at once.
- Grants food to a devouring swarm when `Giant Earthworms` or `Sea Monsters` are cleared.
- Overrides no vanilla file and declares no dependency on any other mod.

## 🚀 Usage

The mod is declarative content: it contains no executable code, and the game engine applies every definition it ships.

1. Install the mod from one of the distribution channels and enable **Universum Infinitum - Deposit Blockers** in the launcher's playset.
2. Start or load a campaign and survey planets. Eligible dry and wet worlds may carry one of the seven blockers.
3. Research the matching removal technology in the society tree, under the `new_worlds` category, gated by `tech_colonization_1`.
4. Clear the blocker from the planet interface by paying its declared cost in energy — and, for `Tarland`, minerals as well.

## ⚠️ Known Limitations

- Blockers are placed during planet generation, so weight and gate changes affect only planets generated afterwards; existing saves keep their original blockers.
- Clearing a blocker is permanent. No definition re-adds a cleared blocker.
- The removal technologies stay unavailable until the empire owns two or more planets and controls a planet carrying the matching blocker, and nomadic empires never receive them.
- Researching one technology of the `deposit_blockers` group halves the availability of the remaining six.
- Localisation ships in English only.
- The repository content is ahead of the newest published GitHub release (`v1.0.1`), so a store installation may serve older definitions.
- There is no test suite, continuous integration, or build step; behaviour is verified by running the repository cross-checks and by playing the game.

## 📦 Installation

[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612)
[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/stellaris/mods/91)
[![Obtain it from Paradox Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/paradox-mods.png)](https://mods.paradoxplaza.com/mods/34638/Any)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases)

### Manual Installation

1. Download the latest release archive from [GitHub Releases](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases).
2. Extract the contents into the game's mod directory so that `ui-deposit-blockers.mod` and the `ui-deposit-blockers/` directory sit alongside the other installed mods.
3. Add the mod to the launcher's active playset and enable it.

### Verification

After enabling the mod, start a campaign and confirm:

- Blockers appear on eligible planets, each with a readable name and an icon.
- The society tree offers the matching removal technologies, for example `Ashland Removal`.
- Clearing a blocker restores the number of maximum districts it removed.

### Uninstallation

Remove the mod from the launcher's active playset, then delete `ui-deposit-blockers.mod` and the `ui-deposit-blockers/` directory from the game's mod directory. Campaign saves retain references to the blockers the mod defined, and the behaviour of such saves after removal is unverified — see [`docs/ambiguities-and-open-questions.md`](./docs/ambiguities-and-open-questions.md).

## 🌍 Localisation

Every blocker and technology identifier is mapped to a display name and a description by an English localisation table; without those keys the game displays the raw identifier.

| Language | Locale | Status |
|----------|--------|--------|
| English | `en` | Complete; the only shipped locale |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Stellaris | `v4.5.*` | Declared by `supported_version` in both descriptors; the wildcard accepts any 4.5 patch |
| Operating system | Any supported by the game | The mod is OS-agnostic, and is developed and tested on Linux, natively and through Steam Play |
| Other mods | Any | No vanilla file is overridden, and every identifier carries the `ui` infix |
| Multiplayer | Matching mod version | The mod is data-only, so compatibility follows the game's own mod-matching rules |

## 🛡️ Privacy and Data

Universum Infinitum - Deposit Blockers ships static game definitions, localisation, and image assets, and neither requests nor stores personal data.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

The mod has no runtime behaviour of its own: the game engine parses its files, so there are no data categories, storage locations, retention periods, or telemetry controls to document. Interactions with Steam Workshop, Paradox Mods, Nexus Mods, and GitHub are governed by those platforms.

## 🛠️ Development

### Requirements

- Stellaris `v4.5.*` for in-game verification.
- Git, for cloning the repository.
- A POSIX shell providing `grep`, `test`, and `diff` for the content cross-checks.
- No compiler, runtime, package manager, build step, or test framework is required.

### Setup

```bash
git clone https://github.com/hmlendea/stellaris-ui-deposit-blockers.git
cd stellaris-ui-deposit-blockers
```

No dependency installation, compilation, or packaging step is required; the mod files are edited directly and loaded by the game.

### Test

There is no test suite. Three cross-checks validate the repository's content invariants, and each passes when its output is silent.

Every technology must be referenced as a deposit prerequisite:

```bash
for t in $(grep -o '^tech_d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt); do grep -q "prerequisites = { \"$t\" }" ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt || echo "unlinked: $t"; done
```

Every deposit and technology identifier must have a matching icon file:

```bash
for d in $(grep -o '^d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt); do test -f "ui-deposit-blockers/gfx/interface/icons/deposits/$d.dds" || echo "missing icon: $d"; done
```

```bash
for t in $(grep -o '^tech_d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/technology/ui-deposit-blockers_technologies.txt); do test -f "ui-deposit-blockers/gfx/interface/icons/technologies/$t.dds" || echo "missing icon: $t"; done
```

The two descriptor files must be identical:

```bash
diff ui-deposit-blockers.mod ui-deposit-blockers/descriptor.mod
```

### Release

1. Confirm the change set covers every coupled surface: definitions, localisation keys, and icons.
2. Run the cross-checks above against the exact content being published.
3. Raise `supported_version` in both descriptors when targeting a new game-version family, and keep the two files identical.
4. Tag the release in Git.
5. Publish the same content set to Steam Workshop, Paradox Mods, Nexus Mods, and GitHub Releases, because each channel stores an independent copy and can drift.

## 🗂️ Project Structure

The repository is a Stellaris mod accompanied by a knowledge base. Two descriptor files register the mod, one directory holds the shipped payload, and `docs/` documents it. Nothing is compiled or generated.

### Directories

| Directory | Purpose |
|-----------|---------|
| `ui-deposit-blockers/` | Shipped mod payload: deposit definitions, technology definitions, English localisation, and icons |
| `docs/` | Knowledge base covering architecture, data model, flows, behaviour, and operations |
| `.github/` | Funding declaration and the repository cover image |

## 🏗️ Architecture

Universum Infinitum - Deposit Blockers is a declarative Stellaris mod that adds seven planet deposit blockers and one prerequisite society technology per blocker, all consumed by the game engine without any mod-owned executable code.

See the [Architecture Documentation](./ARCHITECTURE.md) for the system context, principal components, runtime flows, ownership boundaries, dependencies, constraints, and extension points.

## 📚 Documentation

Detailed documentation for the mod is available in the [`docs/`](./docs/INDEX.md) directory.

| Resource | Description |
|----------|-------------|
| [Documentation Index](./docs/INDEX.md) | Master index of every document in the knowledge base |
| [Quick Start](./docs/quick-start.md) | Installation, enabling, and confirmation of a working install |
| [Repository Overview](./docs/repository-overview.md) | What the mod is, what it owns, and what the game engine owns |
| [Architecture](./docs/architecture.md) | Engine integration model, composition, and boundary rules |
| [Data Model](./docs/data-model.md) | Deposit and technology field reference |
| [Definition Reference](./docs/api-reference/INDEX.md) | Schema index and identifier catalogue |
| [Flows](./docs/flows/startup-and-loading.md) | Startup, placement, research, and clearing execution flows |
| [Behaviour](./docs/behaviour/blocker-appearance.md) | What a player observes when blockers appear and are cleared |
| [Change Guide](./docs/change-guide.md) | How to add, retune, rename, or remove a blocker safely |
| [Testing](./docs/testing.md) | Cross-check commands, manual verification, and coverage gaps |
| [Troubleshooting](./docs/troubleshooting.md) | Diagnosis table for symptoms, causes, and resolutions |
| [Invariants](./docs/invariants.md) | Identifier coupling, weight baseline, and descriptor rules |
| [FAQ](./docs/faq.md) | Frequently asked questions |

## 🩺 Troubleshooting

See [Troubleshooting Guide](./docs/troubleshooting.md) for diagnostic procedures and resolutions to recurring problems.

## ❓ FAQ

See the [Frequently Asked Questions](./docs/faq.md) for responses to frequently raised inquiries.

## 🤝 Contributing

You are welcome to submit any suggestion, feedback, or modification to this project.

When doing so, please:
- Maintain cross-platform compatibility
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Revise the documentation when functionality changes
- Raise a new [issue](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues) for problems or suggestions

See the [Contributing Guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

## 🔗 Related Projects

- [Universum Infinitum - Character Traits](https://github.com/hmlendea/stellaris-ui-character-traits): Adds new character traits.
- [Universum Infinitum - Flags](https://github.com/hmlendea/stellaris-ui-flags): Adds hundreds of flag designs matching the vanilla flag style.
- [Universum Infinitum - Galaxy Sizes](https://github.com/hmlendea/stellaris-ui-galaxy-sizes): Adds additional galaxy sizes.
- [Universum Infinitum - Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists): Adds additional detailed name lists.
- [Universum Infinitum - Planet Modifiers](https://github.com/hmlendea/stellaris-ui-planet-modifiers): Adds new planet modifiers.
- [Universum Infinitum - Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries): Adds prescripted countries for players to select or for the AI to use.
- [Universum Infinitum - Species Names](https://github.com/hmlendea/stellaris-ui-species-names): Adds hundreds of new species names.
- [Universum Infinitum - Species Traits](https://github.com/hmlendea/stellaris-ui-species-traits): Adds new species traits and increases the maximum number of selectable traits.
- [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names): Adds thousands of new names for stars, nebulas, asteroids, and black holes.

## 🔒 Security

Universum Infinitum - Deposit Blockers ships static game definitions, localisation, and image assets, and implements no accounts, telemetry, diagnostics, or transfers of personal data.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 💬 Support

For support, please [open an issue](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues).

## 💝 Project Engagement

Discovered a problem with Universum Infinitum - Deposit Blockers or have a suggestion for it? [Open an issue](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub, or giving it a thumbs up and adding it as a favourite on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612)!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License version 3`.
See [LICENSE](./LICENSE) for additional information.
