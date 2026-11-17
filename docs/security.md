# Security

> The root [`SECURITY.md`](../SECURITY.md) defines supported versions, the reporting route, scope, and the disclosure policy. This document complements it by explaining the **threat model** and the attack surface of a declarative mod, and by stating which mitigations belong to which party.

## Trust model

| Party | Trust level | Controls |
|-------|-------------|----------|
| Repository maintainer | Trusted | The mod content itself |
| Distribution platform | Trusted for delivery, not for content integrity beyond its own checks | Hosting, checksums, malware scanning |
| Game engine | Trusted | All execution |
| Other installed mods | Untrusted by default | May collide in the shared identifier namespace |
| Arbitrary contributor fork | Untrusted | None |

The mod itself executes nothing, so the classic code-execution threat does not apply to the mod's own content. The relevant threats are **content integrity**, **namespace collision**, and **supply-chain delivery**.

## Attack surface

| Surface | Exposed to | Threat |
|---------|-----------|--------|
| `common/deposits/*.txt` | The engine's parser | Malformed syntax; a hostile fork could ship engine-exploiting script if such an exploit exists |
| `common/technology/*.txt` | Same | Same |
| `localisation/english/*.yml` | Localisation loader | Malformed encoding; text-shaping abuse in displayed strings |
| `gfx/interface/icons/*.dds` | Texture loader | Malformed texture data consumed by a native decoder |
| `.mod` descriptors | Launcher | A `path` value pointing outside the intended directory |
| `docs/` and policy files | Humans and agents | None — not consumed by the game |

The most consequential surface is the `.mod` descriptor `path` key, because a crafted value could direct the launcher to load content from an unexpected location. In this repository the value is the constant `mod/ui-deposit-blockers` in both descriptors.

## Threats and mitigations

| Threat | Mitigation present | Owner |
|--------|-------------------|-------|
| A tampered release archive delivering modified definitions | Platform-side integrity measures and checksums; no mod-side signing | Distribution platform |
| A pull request introducing a hostile script payload | Human review of the diff | Maintainer |
| A third-party mod redefining a `d_ui_*` or `tech_d_ui_*` identifier to hijack behaviour | Mod-namespaced identifiers with the `ui` infix reduce collision likelihood; collision is not impossible | Maintainer convention |
| A malformed `.dds` file exploiting a parser bug in the engine or operating system | None mod-side | Engine and operating system vendor |
| A malicious fork redistributing the mod under the project name | The root `SECURITY.md` scope statement excludes downstream forks from maintainer responsibility | Documented, not technically enforced |
| Silent drift between the repository and a distribution channel | Automated GitHub Releases, Nexus Mods, and Steam Workshop publication on a release or tag; manual Paradox Mods procedure | Maintainer |

## What is not a threat here

| Candidate threat | Why it does not apply |
|------------------|------------------------|
| Authentication bypass | No authentication, no accounts, no sessions |
| Authorisation escalation | No authorisation model; the game's own permissions govern play |
| Injection (SQL, command, template) | No query, command, or template execution |
| Data exfiltration | No network, no telemetry, no credentials; see the root [`PRIVACY.md`](../PRIVACY.md) |
| Insecure secret storage | No secrets exist in the repository |
| Denial of service against a service | No service exists |
| Cross-tenant data leakage | No tenancy, no shared data store |

## Player-side exposure

The mod's only interaction with a player's machine is that the engine reads files from the mod directory. There is:

- No network access.
- No file write.
- No process creation.
- No registry or configuration modification outside the game.
- No persistent state other than the save game.

The save game is written by the game, not the mod, and contains no mod-specific metadata beyond the identifiers of placed blockers.

## Operator-side considerations

| Concern | Guidance |
|---------|----------|
| Sourcing the mod | Prefer a distribution channel listed in the root `SECURITY.md`; avoid unofficial mirrors and repacks |
| Verifying contents | The shipped payload is text and images only; an archive containing executables, scripts, or installers is not this mod |
| Reviewing a contribution | The only files that should change are under `ui-deposit-blockers/`, plus documentation and descriptors |
| Multiplayer parity | Clients with mismatched mod versions fail the game's own checksum comparison |

## Identifier namespace as a security-relevant control

The `ui` infix in every identifier is a defensive convention:

```text
d_ui_<name>_blocker
tech_d_ui_<name>_blocker
```

Because the engine merges definitions from all active mods into shared namespaces, an identifier collision is a **silent override** whose outcome depends on load order. Namespace guarding therefore reduces both accidental and deliberate hijacking. Its guarantee is probabilistic, not absolute — see [A10](ambiguities-and-open-questions.md).

## Reporting

Follow the root [`SECURITY.md`](../SECURITY.md): use GitHub Security Advisories or contact the maintainers directly, and do not disclose publicly before the maintainers have validated and remediated.

## Where to go next

- Root policy: [`../SECURITY.md`](../SECURITY.md)
- Privacy posture: [`../PRIVACY.md`](../PRIVACY.md)
- Pipeline and supply-chain context: [`dependencies.md`](dependencies.md)
