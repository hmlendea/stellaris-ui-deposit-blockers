# Presentation assets

Component: `ui-deposit-blockers/gfx/interface/icons/` and store listing imagery

## Purpose

Supplies the icons the engine resolves for each blocker and technology, plus the store listing imagery. The engine resolves the icon files **by name**, not by an explicitly declared path, so the file name is the entire contract.

## Deposits icons

Location: `ui-deposit-blockers/gfx/interface/icons/deposits/`

| File | Size | Format |
|------|------|--------|
| `d_ui_ashland_blocker.dds` | 98x75 | 32-bit ARGB8888 |
| `d_ui_canyon_blocker.dds` | 98x75 | 32-bit ARGB8888 |
| `d_ui_floodplain_blocker.dds` | 98x75 | 32-bit ARGB8888 |
| `d_ui_giant_earthworms_blocker.dds` | 98x75 | 32-bit ARGB8888 |
| `d_ui_lava_lake_blocker.dds` | 98x75 | 32-bit ARGB8888 |
| `d_ui_sea_monsters_blocker.dds` | 98x75 | 32-bit ARGB8888 |
| `d_ui_tarland_blocker.dds` | 98x75 | 32-bit ARGB8888 |

All seven share the same 98x75 geometry and the same 32-bit format, so they are interchangeable in layout terms. The alpha channel carries the transparency used around the irregular tile shape.

## Technology icons

Location: `ui-deposit-blockers/gfx/interface/icons/technologies/`

| File | Size | Format |
|------|------|--------|
| `tech_d_ui_ashland_blocker.dds` | 52x52 | 24-bit RGB888 |
| `tech_d_ui_canyon_blocker.dds` | 52x52 | 24-bit RGB888 |
| `tech_d_ui_floodplain_blocker.dds` | 52x52 | 24-bit RGB888 |
| `tech_d_ui_giant_earthworms_blocker.dds` | 52x52 | 24-bit RGB888 |
| `tech_d_ui_lava_lake_blocker.dds` | 52x52 | 24-bit RGB888 |
| `tech_d_ui_sea_monsters_blocker.dds` | 52x52 | 24-bit RGB888 |
| `tech_d_ui_tarland_blocker.dds` | 52x52 | 24-bit RGB888 |

Technology icons are square and opaque, matching the society tree's icon frame.

## Store listing imagery

| File | Size | Purpose |
|------|------|---------|
| `ui-deposit-blockers/thumbnail.png` | 1000x1000 | Store listing thumbnail inside the mod directory |
| `.github/ui_cover.png` | 1280x640 | Repository cover image; not part of the shipped mod payload |

## Naming contract

The naming rule is absolute:

```
<identifier>.dds
```

| Definition identifier | Required file |
|----------------------|---------------|
| `d_ui_ashland_blocker` | `gfx/interface/icons/deposits/d_ui_ashland_blocker.dds` |
| `tech_d_ui_ashland_blocker` | `gfx/interface/icons/technologies/tech_d_ui_ashland_blocker.dds` |

There is no manifest, `.gfx` index, or path declaration in the repository. Renaming an icon file without renaming its definition — or the reverse — silently produces a blank icon with no error.

## Historical note

Git history contains a commit titled "Fixed missing icons", indicating this naming contract has been violated before and that the failure mode is a missing-icon report from a player rather than a script error.

## Modification impact

| Change | Also requires |
|--------|---------------|
| Add a blocker | Two `.dds` files, one per icon directory, named after the two identifiers |
| Rename an identifier | Rename both `.dds` files |
| Update artwork | Nothing else, provided the file name and dimensions are preserved |
| Change dimensions | Verify the engine's expected geometry for the icon slot; other sizes may be scaled or cropped |
| Change format | Prefer matching the existing format per directory; deposit icons carry alpha, technology icons do not |

## Verification

Run the icon cross-check from [`../testing.md`](../testing.md):

```bash
for d in $(grep -o '^d_ui_[a-z_]*_blocker' ui-deposit-blockers/common/deposits/ui-deposit-blockers.txt); do test -f "ui-deposit-blockers/gfx/interface/icons/deposits/$d.dds" || echo "missing icon: $d"; done
```

Silence means every deposit identifier has a matching icon file.

## Where to go next

- Related invariant: [`../invariants.md`](../invariants.md) I9, I26, I27
- Localisation companion: [`localisation.md`](localisation.md)
