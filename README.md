[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases/latest)
[![Website](https://img.shields.io/badge/Website-Visit-blue)](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/blob/master/LICENSE)

# Universum Infinitum - Deposit Blockers

Universum Infinitum - Deposit Blockers is a declarative Stellaris mod that adds seven planet deposit blockers and one matching society technology to clear each of them, purely to increase planetary variety.

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
- [GitHub Actions](#-github-actions)
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

- Seven planet deposit blockers: `Ashland`, `Canyon`, `Floodplain`, `Giant Earthworms`, `Lava Lake`, `Sea Monsters`, and `Tarland`.
- One dedicated society technology per blocker, offered at tier 1 or tier 2 within the `new_worlds` category.
- Every blocker removes one or two maximum districts until it is cleared through its technology.
- Thematic planet-class weighting, so each blocker favours the worlds that match its flavour.
- Placement gated by planet class, climate family, planet size, and remaining free city districts.
- Two organic blockers grant food to devouring swarms when they are cleared.
- Every blocker and technology ships its own icon, display name, and description.
- The seven technologies share a `deposit_blockers` weight group, so they are researched gradually instead of all at once.
- Mod-namespaced identifiers with a `ui` infix, so the mod collides with neither vanilla nor third-party definitions.

## 🚀 Usage

The mod is declarative content: it contains no executable code, and the game engine applies every definition it ships.

1. Install the mod from one of the distribution channels and enable **Universum Infinitum - Deposit Blockers** in the launcher's playset.
2. Start or load a campaign and survey planets. Eligible dry and wet worlds may carry one of the seven blockers.
3. Research the matching removal technology in the society tree, under the `new_worlds` category, gated by `tech_colonization_1`.
4. Clear the blocker from the planet interface by paying its declared cost in energy — and, for `Tarland`, minerals as well.

## ⚠️ Known Limitations

- Blocker placement is decided at planet-generation time, so weight and gate changes affect only planets generated afterwards; existing saves keep their original blockers.
- Clearing a blocker is permanent. No definition re-adds a cleared blocker.
- The removal technologies stay unavailable until the empire owns two or more planets and controls a planet carrying the matching blocker, and nomadic empires never receive them.
- Researching one technology of the `deposit_blockers` group halves the availability of the remaining six.
- Only English localisation is provided; other locales display the English strings.
- The repository content is ahead of the newest published GitHub release (`v1.0.1`), so a store installation may serve older definitions.
- Validity of the definitions depends on manual cross-checks and in-game observation, because the engine reports malformed blocks only indirectly.

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

- [Stellaris](https://store.steampowered.com/app/281990/Stellaris/) `v4.5.*` for in-game verification.
- Git, for cloning the repository.
- A POSIX shell providing `grep`, `test`, and `diff` for the content cross-checks.
- No compiler, runtime, package manager, build step, or test framework is required.

### Setup

Clone the repository and make its contents available to the game's mod directory:

```bash
git clone https://github.com/hmlendea/stellaris-ui-deposit-blockers.git
cd stellaris-ui-deposit-blockers
ln -s "$PWD/ui-deposit-blockers" "$HOME/.steam/steam/steamapps/common/Stellaris/mod/ui-deposit-blockers"
ln -s "$PWD/ui-deposit-blockers.mod" "$HOME/.steam/steam/steamapps/common/Stellaris/mod/ui-deposit-blockers.mod"
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
4. Tag the release in Git and attach the packaged `ui-deposit-blockers_<version>.zip` archive to the GitHub release.
5. Let `.github/workflows/nexus-mods-release.yml` and `.github/workflows/steam-workshop-release.yml` publish the tag to Nexus Mods and Steam Workshop, and upload the same content set manually to Paradox Mods and GitHub Releases, because each channel stores an independent copy and can drift.

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [ Nexus Mods ](https://github.com/hmlendea/stellaris-ui-deposit-blockers/blob/master/.github/workflows/nexus-mods-release.yml) | Publish releases to Nexus Mods | Runs on a `v*.*.*` tag push or a manual dispatch with a `tag` input, downloads the release archive `ui-deposit-blockers_<version>.zip` from the matching GitHub release, and uploads it with the `Nexus-Mods/upload-action` action |
| [ Steam Workshop ](https://github.com/hmlendea/stellaris-ui-deposit-blockers/blob/master/.github/workflows/steam-workshop-release.yml) | Publish releases to Steam Workshop | Runs on a `v*.*.*` tag push, downloads and unzips the release archive `ui-deposit-blockers_<version>.zip` from the matching GitHub release, and uploads it to Workshop item `2073660612` with the `hmlendea/steam-workshop-update` action |

## 🗂️ Project Structure

```
.
├── .github/
│   ├── FUNDING.yml
│   ├── ui_cover.png
│   └── workflows/
│       ├── nexus-mods-release.yml
│       └── steam-workshop-release.yml
├── docs/                       # Documentation knowledge base
├── ui-deposit-blockers.mod     # Root mod descriptor
└── ui-deposit-blockers/        # Mod payload
    ├── descriptor.mod
    ├── common/
    ├── gfx/
    └── localisation/
```

### Directories

| Directory | Purpose |
|-----------|---------|
| `ui-deposit-blockers/` | The complete mod payload shipped to every distribution channel |
| `ui-deposit-blockers/common/` | Deposit blocker and society technology definitions |
| `ui-deposit-blockers/gfx/interface/icons/` | Blocker and technology icons, resolved by file name |
| `ui-deposit-blockers/localisation/english/` | English strings keyed by definition identifier |
| `docs/` | Documentation knowledge base; not shipped to players |
| `.github/` | Funding declaration, repository cover image, and the Nexus Mods and Steam Workshop publication workflows |

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
- Preserve the existing public contract unless a breaking change is intentional
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Revise the documentation when functionality changes
- Properly test all modifications, including edge cases and error conditions
- Include tests for additional or modified functionality
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

Universum Infinitum - Deposit Blockers ships static game definitions, localisation, and image assets, and neither implements accounts, telemetry, or diagnostics, nor transfers personal data.

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
