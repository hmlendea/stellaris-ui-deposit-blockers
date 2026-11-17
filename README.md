[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/fund.html) [![Latest GitHub release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-deposit-blockers)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases/latest)

# About

Mod that adds a few more deposit blockers and their respective technologis, purely to increase variety.

# Compatibility

The mod is **OS-agnostic**, which means that it will work on all of the operating systems supported by the game.

It is actively tested (and developed) on **Linux**, where it is guaranteed to work with either the native version of the game or through Steam Play.

It should also be compatible with any other mods.

# Installation

[![Get it from the Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612) [![Get it from Paradox Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/paradox-mods.png)](https://mods.paradoxplaza.com/mods/34638/Any) [![Get it from the Nexus](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://www.nexusmods.com/stellaris/mods/91) [![Get it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases)

## Manual installation

 - Download the [latest release](https://github.com/hmlendea/stellaris-ui-deposit-blockers/releases)
 - Extract the contents into your game's mod directory

## 🏗️ Architecture

Universum Infinitum - Deposit Blockers is a declarative Stellaris mod that adds seven planet deposit blockers and one prerequisite society technology per blocker, all consumed by the game engine without any mod-owned executable code.

See the [Architecture Documentation](./ARCHITECTURE.md) for the system context, principal components, runtime flows, ownership boundaries, dependencies, constraints, and extension points.

## 🛡️ Privacy and Data

Universum Infinitum - Deposit Blockers does not request, collect, transmit, or persist personal data; its files define planet deposit blockers and their removal technologies for Stellaris.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

The mod contains local game definitions, localisation, and image assets. It has no built-in personal-data transfer; interactions with distribution platforms are separate from mod execution.

## 🔒 Security

Universum Infinitum - Deposit Blockers ships static game definitions, localisation, and image assets, and does not implement accounts, telemetry, diagnostics, or built-in transfers of personal data.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

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

# Contributions

You are welcome to bring any suggestion, feedback or modification to this project.

Here are a few ways in which you can do so:
 - Commenting directly on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2073660612) page
 - Raising a new [issue](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues) for this project
 - Creating a git pull request _(This requires git knowledge)_

See the [Contributing Guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

# Universul Unfinitum Collection

 - [Character Traits](https://github.com/hmlendea/stellaris-ui-character-traits)
 - [Deposit Blockers](https://github.com/hmlendea/stellaris-ui-deposit-blockers)
 - [Flags](https://github.com/hmlendea/stellaris-ui-flags)
 - [Galaxy Sizes](https://github.com/hmlendea/stellaris-ui-galaxy-sizes)
 - [Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists)
 - [Planet Modifiers](https://github.com/hmlendea/stellaris-ui-planet-modifiers)
 - [Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries)
 - [Species Names](https://github.com/hmlendea/stellaris-ui-species-names)
 - [Species Traits](https://github.com/hmlendea/stellaris-ui-species-traits)
 - [Star Names](https://github.com/hmlendea/stellaris-ui-star-names)
