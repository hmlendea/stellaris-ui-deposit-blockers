[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases/latest)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/blob/master/LICENSE)

# Universum Infinitum - Deposit Blockers

Stellaris mod that adds seven planet deposit blockers and one matching society technology to clear each of them, purely to increase planetary variety.

## 📑 Table of Contents

- [Capabilities](#-capabilities)
- [Usage](#-usage)
- [Known Limitations](#-known-limitations)
- [Installation](#-installation)
  - [Manual Installation](#manual-installation)
- [Localisation](#-localisation)
- [Compatibility](#-compatibility)
- [Privacy and Data](#-privacy-and-data)
- [Development](#-development)
  - [Requirements](#requirements)
  - [Setup](#setup)
  - [Run](#run)
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

## ✨ Capabilities

- Seven planet deposit blockers, each gated by planet class, climate family, planet size, and remaining free city districts
- One dedicated society technology per blocker, offered at tier 1 or tier 2 within the `new_worlds` category
- Every blocker removes one or two maximum districts until it is cleared through its technology
- Thematic planet-class weighting, so each blocker favours the worlds that match its flavour
- Two organic blockers grant food to devouring swarms when they are cleared
- Mod-namespaced identifiers with a `ui` infix, so the mod does not collide with vanilla or third-party definitions

## 🚀 Usage

Enable the mod in the Stellaris launcher and start a new game. The blockers are placed during planet generation, and each one can be cleared after its matching technology has been researched.

## ⚠️ Known Limitations

- Only English localisation is provided; other locales display the English strings.
- Blocker placement is decided at planet-generation time, so weight changes affect only planets generated afterwards.
- Validity of the definitions depends on manual cross-checks and in-game observation, because the engine reports malformed blocks only indirectly.

## 📦 Installation

[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612)
[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/stellaris/mods/91)
[![Obtain it from Paradox Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/paradox-mods.png)](https://mods.paradoxplaza.com/mods/34638/Any)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases)

### Manual Installation

1. Download the [latest release](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases).
2. Extract the contents into your game's mod directory, so that `ui-deposit-blockers.mod` and the `ui-deposit-blockers/` directory sit alongside the other installed mods.

## 🌍 Localisation

The mod ships a single localisation table covering the blocker and technology names and descriptions.

| Language | Locale | Status |
|----------|--------|--------|
| English | `en` | Complete |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Stellaris | `v4.5.*` | Declared through `supported_version` in both descriptors |
| Operating systems | All operating systems supported by the game | Actively tested and developed on Linux, natively or through Steam Play |
| Other mods | Any | All identifiers are mod-namespaced, so no vanilla file is overridden |

## 🛡️ Privacy and Data

Universum Infinitum - Deposit Blockers does not request, collect, transmit, or persist personal data; its files define planet deposit blockers and their removal technologies for Stellaris.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

## 🛠️ Development

### Requirements

- [Stellaris](https://store.steampowered.com/app/281990/Stellaris/)

### Setup

Clone the repository and make its contents available to the game's mod directory:

```bash
git clone https://github.com/hmlendea/stellaris-ui-deposit-blockers.git
cd stellaris-ui-deposit-blockers
ln -s "$PWD/ui-deposit-blockers" "$HOME/.steam/steam/steamapps/common/Stellaris/mod/ui-deposit-blockers"
ln -s "$PWD/ui-deposit-blockers.mod" "$HOME/.steam/steam/steamapps/common/Stellaris/mod/ui-deposit-blockers.mod"
```

No dependency installation or packaging step is required; edit the mod files directly.

### Run

Enable the mod in the Stellaris launcher and start a new game. Any definition, localisation, or icon change is picked up on the next launch.

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [ Nexus Mods ](https://github.com/hmlendea/stellaris-ui-deposit-blockers/blob/master/.github/workflows/nexus-mods-release.yml) | Publish releases to Nexus Mods | Runs on a `v*.*.*` tag push or a manual dispatch with a `tag` input, downloads the release archive `ui-deposit-blockers_<version>.zip` from the matching GitHub release, and uploads it with the `Nexus-Mods/upload-action` action |

## 🗂️ Project Structure

```
.
├── .github/
│   ├── FUNDING.yml
│   ├── ui_cover.png
│   └── workflows/
│       └── nexus-mods-release.yml
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

- [Character Traits](https://github.com/hmlendea/stellaris-ui-character-traits): Sibling mod in the same Universum Infinitum collection
- [Flags](https://github.com/hmlendea/stellaris-ui-flags): Sibling mod in the same Universum Infinitum collection
- [Galaxy Sizes](https://github.com/hmlendea/stellaris-ui-galaxy-sizes): Sibling mod in the same Universum Infinitum collection
- [Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists): Sibling mod in the same Universum Infinitum collection
- [Planet Modifiers](https://github.com/hmlendea/stellaris-ui-planet-modifiers): Sibling mod in the same Universum Infinitum collection
- [Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries): Sibling mod in the same Universum Infinitum collection
- [Species Names](https://github.com/hmlendea/stellaris-ui-species-names): Sibling mod in the same Universum Infinitum collection
- [Species Traits](https://github.com/hmlendea/stellaris-ui-species-traits): Sibling mod in the same Universum Infinitum collection
- [Star Names](https://github.com/hmlendea/stellaris-ui-star-names): Sibling mod in the same Universum Infinitum collection

## 🔒 Security

Universum Infinitum - Deposit Blockers ships static game definitions, localisation, and image assets, and does not implement accounts, telemetry, diagnostics, or built-in transfers of personal data.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 💬 Support

For support, please [open an issue](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues).

## 💝 Project Engagement

Discovered a problem with Universum Infinitum - Deposit Blockers or have a suggestion for it? [Open an issue](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub, or giving it a thumbs up and adding it as a favourite on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612)!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License version 3` or later.
See [LICENSE](./LICENSE) for additional information.
