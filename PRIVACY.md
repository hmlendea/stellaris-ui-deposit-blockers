# Privacy and Personal Data

This document covers Universum Infinitum - Deposit Blockers, a locally installed Stellaris mod distributed from https://github.com/hmlendea/stellaris-ui-deposit-blockers. The mod consists of deposit definitions, technology definitions, localisation strings, and image assets, and it does not request, collect, transmit, or store personal data. Information is current as of 2026-11-17.

## 📑 Table of Contents

- [Table of Contents](#-table-of-contents)
- [What This Document Covers](#-what-this-document-covers)
- [Self-Hosted Deployments](#-self-hosted-deployments)
- [Data We Handle](#-data-we-handle)
  - [Data Provided to the Application](#data-provided-to-the-application)
  - [Data Generated or Collected by the Application](#data-generated-or-collected-by-the-application)
  - [Data Received from Integrations](#data-received-from-integrations)
- [Processing and Use](#-processing-and-use)
- [Storage, Retention, and Deletion](#-storage-retention-and-deletion)
- [External Processing and Integrations](#-external-processing-and-integrations)
- [User Controls and Requests](#-user-controls-and-requests)
- [Data Protection and Security](#-data-protection-and-security)
- [Document Changes](#-document-changes)
- [Contact](#-contact)

## 🔎 What This Document Covers

This document describes how Universum Infinitum - Deposit Blockers at https://github.com/hmlendea/stellaris-ui-deposit-blockers manages personal data. It covers the application conduct and verified integrations described below. Where the software is self-hosted, the instance operator may have separate responsibilities described below.

## 🏠 Self-Hosted Deployments

This project distributes a mod installed and operated locally alongside Stellaris; it does not operate a hosted instance. This notice covers the mod and project-maintainer-controlled behaviour. Each player or operator controls their game configuration, local files, logs, backups, access controls, preservation, and request handling unless the project directly controls those functions.

The mod consists of static deposit and technology definitions, localisation, and image assets. It has no mod-level telemetry, update checks, crash reports, email, authentication, reverse-proxy, object-storage, or monitoring integrations, and it sends no data from a local installation to project maintainers or external services. The game, launcher, operating system, and distribution platforms may have their own data practices outside this mod.

## 📥 Data We Handle

### Data Provided to the Application

- The mod does not request personal data. Stellaris reads the deposited blocker definitions and removal technologies; this mod does not use game data to identify a person.

### Data Generated or Collected by the Application

- The mod does not automatically generate or collect personal data, telemetry, diagnostics, or usage data.

### Data Received from Integrations

- The mod has no integrations that receive personal data.

## 🧭 Processing and Use

The application processes the data described above for these verified functions:
- Place the mod's deposit blockers on generated planets — static deposit definitions consumed by the game; no personal data is involved.
- Offer the mod's removal technologies and arbitrate their availability — static technology definitions consumed by the game; no personal data is involved.
- Display blocker names, technology names, and icons — static localisation strings and image assets; no personal data is involved.

## 🗄️ Storage, Retention, and Deletion

The mod's files are stored in the location selected by the player or operator when installing the mod. The project does not store personal data from players, and it contains no account, database, telemetry, or user-data storage component. The mod does not create personal-data records to retain or delete. Players control local game files and any game or launcher data, including save games that record blocker state; removing or disabling the mod is described in the [README](./README.md).

## 🔗 External Processing and Integrations

The mod has no built-in external data transfer. Steam Workshop, Paradox Mods, Nexus Mods, and GitHub are distribution channels; interactions with those platforms happen separately from mod execution and are subject to each platform's own data practices.

| Service or integration | Purpose | Data involved | Configuration or documentation |
|-----------------------|---------|---------------|--------------------------------|
| None built into the mod | No external processing by the mod | No personal data transferred by the mod | Not applicable |

## ⚙️ User Controls and Requests

The following documented controls or request procedures are available:
- Disable the mod in the Stellaris launcher or remove it from the local mod installation — the project does not retain personal data to access, export, or delete.

## 🛡️ Data Protection and Security

The mod does not process personal data and therefore has no mod-level personal-data storage or security controls. Players and operators are responsible for keeping Stellaris and their systems updated and for protecting their accounts, local files, backups, access controls, network exposure, and any logs created by the game or launcher. No system can be guaranteed absolutely secure.

The original maintainers are not responsible for changes, modifications, or privacy issues introduced by downstream forks.

## 🔄 Document Changes

Update this document when application data processes, storage, integrations, or deployment responsibilities change. The current version is published at [PRIVACY.md](./PRIVACY.md).

## 📬 Contact

For questions about application data handling, contact the project maintainers through [GitHub Issues](https://github.com/hmlendea/stellaris-ui-deposit-blockers/issues). For a self-hosted instance, contact the instance operator, unless the project explicitly manages the request. Include the mod version, distribution platform, and relevant data flow in the issue; do not send passwords, access tokens, or other secrets.
