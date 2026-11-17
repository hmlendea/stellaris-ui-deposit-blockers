# Localisation

Component: `ui-deposit-blockers/localisation/english/`

## Purpose

Maps every definition identifier to a display name and a description. Without these keys the engine shows the raw identifier, so this component is the difference between `Ashland` and `d_ui_ashland_blocker` in the interface.

## Files

| File | Keys | Content |
|------|------|---------|
| `ui-tile-blockers_l_english.yml` | 14 | Seven blocker names + seven blocker descriptions |
| `ui-tile-blockers_technologies_l_english.yml` | 14 | Seven technology names + seven technology descriptions |

Both files are UTF-8 **with BOM** — verified with `file`. The BOM is significant: the engine expects it for localisation files, and stripping it can produce an encoding error or a first line that fails to parse.

Both files declare exactly one language block, `l_english:`.

## File naming

The file names deliberately do **not** match the mod id. They use the legacy `ui-tile-blockers` prefix retained from the mod's original name, while the repository and mod id use `ui-deposit-blockers`. The engine does not care about the localisation file names — only the keys inside them matter — but contributors should not rename them on the assumption that they are inconsistent by mistake.

## Key conventions

| Key form | Purpose | Example |
|----------|---------|---------|
| `<identifier>:<version>` | Display name | `d_ui_ashland_blocker:0` |
| `<identifier>_desc:<version>` | Description | `d_ui_ashland_blocker_desc:0` |

The trailing integer is the string version. Blocker entries use `:0`; technology descriptions use `:1`, while technology names use `:0`. The version marker lets the engine detect changed strings for cached translations.

## Prefix icon convention

Every blocker name begins with the `£blocker` token:

```
d_ui_ashland_blocker:0 "£blocker  Ashland"
```

The `£` prefix references an engine icon glyph, followed by **two spaces** before the display text. All seven names use this convention. Technology names carry no icon prefix.

## Content reference

### Blocker names and descriptions

| Key | Value |
|-----|-------|
| `d_ui_ashland_blocker` | `Ashland` — "This whole region is a desolate ash wasteland." |
| `d_ui_canyon_blocker` | `Canyon` — "This entire region is a huge canyon range of extreme proportions, making it completely unworkable and uninhabitable." |
| `d_ui_floodplain_blocker` | `Floodplain` — "This region is subject to heavy seasonal floodings making it unworkable and extremely inhospitable." |
| `d_ui_giant_earthworms_blocker` | `Giant Earthworms` — "There are giant earthworms lurking in this region." |
| `d_ui_lava_lake_blocker` | `Lava Lake` — "This region is covered in a huge lava lake that makes it very dangerous and uninhabitable." |
| `d_ui_sea_monsters_blocker` | `Sea Monsters` — "This waters are lurking with dangerous sea monsters." |
| `d_ui_tarland_blocker` | `Tarland` — "This vast region is a covered in tar lakes and ponds, making it utterly unlivable and poisonous." |

### Technology names and descriptions

| Key | Value |
|-----|-------|
| `tech_d_ui_ashland_blocker` | `Ashland Removal` |
| `tech_d_ui_canyon_blocker` | `Canyon Removal` |
| `tech_d_ui_floodplain_blocker` | `Floodplains Removal` |
| `tech_d_ui_giant_earthworms_blocker` | `Giant Earthworms Removal` |
| `tech_d_ui_lava_lake_blocker` | `Lava Lake Removal` |
| `tech_d_ui_sea_monsters_blocker` | `Sea Monsters Removal` |
| `tech_d_ui_tarland_blocker` | `Tarland Removal` |

Every technology description ends with the literal escape `\n\n`, producing a trailing blank line in the in-game tooltip. This is present in all seven entries and should be preserved when editing.

## Naming inconsistency to preserve

The technology display names use `Removal` uniformly, but the Floodplains entry pluralises the blocker name (`Floodplains Removal` versus the blocker name `Floodplain`). This is a deliberate-or-historical inconsistency; changing it alters displayed text only.

## Modification impact

| Change | Also requires |
|--------|---------------|
| Add a blocker | Four new keys (two per file for the blocker, two per file for the technology) |
| Rename an identifier | Rename all four keys associated with that blocker |
| Edit display text | No other file needs changing; the change is immediate for existing games |
| Add a locale | A new directory under `localisation/` and a complete set of keys for that locale; see [A5](../ambiguities-and-open-questions.md) for the observed English-only state |
| Change the `£blocker` prefix | Verify the glyph token still exists in the target game version |

## Where to go next

- Key coupling invariant: [`../invariants.md`](../invariants.md) I8
- Encoding invariant: [`../invariants.md`](../invariants.md) I28
- Presentation component: [`presentation-assets.md`](presentation-assets.md)
