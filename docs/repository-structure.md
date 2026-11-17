# Repository structure

## Top-level layout

```
.
├── .github/
│   ├── FUNDING.yml                 # Donation declaration (custom + patron/ko_fi/liberapay)
│   ├── ui_cover.png                # 1280x640 store cover image
│   └── workflows/
│       ├── nexus-mods-release.yml       # Publishes tagged releases to Nexus Mods
│       └── steam-workshop-release.yml   # Publishes tagged releases to Steam Workshop
├── .gitignore                      # Ignores *.zip (packaged release archives)
├── ARCHITECTURE.md                 # Root architecture document
├── CODE_OF_CONDUCT.md              # Conduct standards
├── CONTRIBUTING.md                 # Contribution process
├── LICENSE                         # GNU General Public License version 3
├── PRIVACY.md                      # Personal-data statement
├── README.md                       # Player-facing documentation
├── SECURITY.md                     # Security policy
├── docs/                           # This documentation tree
├── ui-deposit-blockers.mod         # Root mod descriptor
└── ui-deposit-blockers/            # Mod content directory
    ├── descriptor.mod              # In-directory descriptor (byte-identical to the root one)
    ├── thumbnail.png               # 1000x1000 store thumbnail
    ├── common/
    │   ├── deposits/
    │   │   └── ui-deposit-blockers.txt             # 7 deposit definitions + weight baselines
    │   └── technology/
    │       └── ui-deposit-blockers_technologies.txt # 7 technology definitions
    ├── gfx/
    │   └── interface/
    │       └── icons/
    │           ├── deposits/       # 7 DDS icons, 98x75, 32-bit ARGB8888
    │           └── technologies/   # 7 DDS icons, 52x52, 24-bit RGB888
    └── localisation/
        └── english/
            ├── ui-tile-blockers_l_english.yml              # Blocker names and descriptions
            └── ui-tile-blockers_technologies_l_english.yml # Technology names and descriptions
```

## Directory responsibilities

| Directory | Responsibility | Notes |
|-----------|----------------|-------|
| `.github/` | Repository hosting metadata | Holds the funding declaration, the store cover image, and the Nexus Mods and Steam Workshop publication workflows |
| `docs/` | This knowledge base | Not shipped to players; engine ignores it |
| `ui-deposit-blockers/` | The complete mod payload | The only directory a distribution package must include, alongside the root `.mod` file |
| `common/deposits/` | Deposit (blocker) definitions | Engine merges every file in this directory across all active mods |
| `common/technology/` | Technology definitions | Same merge semantics; identifiers must be unique across mods |
| `gfx/interface/icons/deposits/` | Blocker textures | Resolved by file name, not by a declared path |
| `gfx/interface/icons/technologies/` | Technology textures | Same naming-based resolution |
| `localisation/english/` | English strings | Keyed by definition identifier |

## Naming conventions

| Artefact | Convention | Example |
|----------|------------|---------|
| Mod root descriptor | `<mod-directory-name>.mod` | `ui-deposit-blockers.mod` |
| Deposit identifier | `d_ui_<name>_blocker` | `d_ui_ashland_blocker` |
| Technology identifier | `tech_d_ui_<name>_blocker` | `tech_d_ui_ashland_blocker` |
| Blocker icon file | `<deposit identifier>.dds` | `d_ui_ashland_blocker.dds` |
| Technology icon file | `<technology identifier>.dds` | `tech_d_ui_ashland_blocker.dds` |
| Localisation key (name) | `<identifier>:<version>` | `d_ui_ashland_blocker:0` |
| Localisation key (description) | `<identifier>_desc:<version>` | `d_ui_ashland_blocker_desc:0` |
| Script file | lowercase kebab-case with underscores in the mod id | `ui-deposit-blockers_technologies.txt` |

The `ui_` infix in every identifier is a deliberate namespace guard: it keeps this mod's definitions distinguishable from vanilla and third-party identifiers that the engine merges into the same namespace.

## Content files in detail

### `common/deposits/ui-deposit-blockers.txt`

366 lines. Structure:

1. Three scripted variables (`@blocker_high = 10`, `@blocker_medium = 7`, `@blocker_low = 5`).
2. Seven deposit blocks separated by `# <Name>` comments, ordered alphabetically: Ashland, Canyon, Floodplain, Giant Earthworms, Lava Lake, Sea Monsters, Tarland.

Each deposit block declares `time`, `is_for_colonizable`, `category`, `resources`, `potential`, `planet_modifier`, optionally `all_blocker_swap_types` and `on_cleared`, `drop_weight`, and `prerequisites`.

### `common/technology/ui-deposit-blockers_technologies.txt`

508 lines. Structure: seven technology blocks, alphabetical by the same `<name>` token, each declaring `cost`, `area`, `tier`, `category`, `prerequisites`, `weight`, `potential`, `weight_groups`, `mod_weight_if_group_picked`, `weight_modifier`, and `ai_weight`.

### Localisation files

Two `l_english` tables, each with 14 keys (7 names, 7 descriptions). Both files are UTF-8 **with BOM**.

## Files that are not part of the shipped mod

| File | Reason |
|------|--------|
| `docs/` | Knowledge base for maintainers and agents |
| Root policy documents | Repository hosting metadata |
| `.github/` | Repository hosting metadata |
| `.gitignore` | Development convenience |

## Where to go next

- Field-by-field specification: [`api-reference/INDEX.md`](api-reference/INDEX.md)
- How the engine consumes these files: [`architecture.md`](architecture.md)
