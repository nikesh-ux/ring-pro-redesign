# Ring PRO · HSA/FSA V3: changes since last night's build

**Baseline:** commit `874d16c` (pushed 28 Oct, ~23:45). That is the version the dev built from.
**Source of truth:** Figma, file `Web-Homepage` (`16KLO24dU4qII4JaMsKIfb`), page **Homepage USA – 26 Oct**.
- Desktop: `HSA/FSA – V3 (desktop)` → node `3032:6866`
- Mobile: `HSA/FSA – V3 (mobile)` → node `3032:7453`
- Other sub screens: frames named `other screens` (desktop) and `mob` (mobile)

The web prototype is `hsa-fsa-v3.html` (fixed 1600 px stage, scaled with `zoom`). It has **no responsive/mobile layout**. Mobile exists only in Figma.

The prototype was updated through item 6 below. Items 7–9 exist **only in Figma** and still need building.

---

## 1. Hero (web + Figma)

| What | Before (874d16c) | Now |
|---|---|---|
| Height | `100vh` via JS, clamped 740–1050 | **90 % of viewport**, clamped 640–1100 (`fit()` in the bottom script: `hH=Math.min(1100,Math.max(640,innerHeight*.9/z))`) |
| Frame | full-bleed, square corners | **inset 12 px** left/right/top, **28 px radius**, width 1576 (`section.hh{width:1576px;margin:12px 12px 0;border-radius:28px}`) |
| Photo bands | 3 bands sized in px (533.33 px) | **three equal thirds** (`33.3333 %`), straight cuts; the old slanted `clip-path` is removed (`--d:0px`) |
| Left photo | `hero-stretch.webp` | **`hero-bottle.webp`** (new asset), cropped below the chin so no face shows. The crop scales with hero height through the CSS variable `--bh`, which JS sets in `fit()` |
| Middle / right photo | run / latte | unchanged (`hero-run.webp`, `hero-latte.webp`) |
| Tint | black gradient .92 → .78 → .73 | unchanged |
| H1 | 84 px / 1.04 | **64 px, line-height 78 px**, letter-spacing −3.8 px (matches Figma) |
| Secondary CTA "How to pay" | underlined text link | **outlined ghost button**: 40 px high, 26 px side padding, 1 px border `rgba(255,255,255,.4)`, fill `rgba(255,255,255,.06)`, 20 px radius, same mono type as "Shop now". Gap between the two buttons is 12 px (was 28) |

Notes: `hero-bottle.webp` is 1328×1760. The CSS in `.hh .bg .p1 img` positions it using `--bh`. If the hero height logic changes, keep that variable in sync.

## 2. Route finder (web + Figma)

- Selected pill is **black** with white text, no shadow, no border (`.opts input:checked+span`). Unselected stay white with a hairline border.
- **"You'll need" block removed everywhere.** The `<p id="r-need">`, its CSS and the JS line that filled it are deleted. The `n:` strings in the `ROUTES` data are now unused and can be deleted.
- Result card, matched to the edited Figma node `3032:6923`:
  - "4 STEPS" chip removed (`#r-cnt` and its JS)
  - step 1 has no top divider (`.res li:first-child{border-top:0}`)
  - extra gap under the intro paragraph removed
  - panel padding is now `36px 48px 24px` (was `44px 48px 24px`)
  - result heading is 40 px
- Route heading "Two questions. Your route." is 48 px.

## 3. Type scale rule: only 40 or 48 for headings

Desktop section headings are **48**. The hero H1 is the only exception (**64**). Mobile equivalents are **40**.

Already changed on web: `.hd h2`, `.fd .hd h2`, `.tx h2`, `.w3 h2` (second rule), `.vt h4`, `.stx .vt h4`, `.pxs .sent`, `.res h3` (40).

### Still to change on web
These are still 64 px in `hsa-fsa-v3.html`. In Figma they are 48 now:
`.w3 h2` (line ~407), `.px h2`, `.rcd h2`, `.fq .hd h2`, `.rg h2`, `.ckh h2`, `.shp h2`, `.fq h2`.
Keep `.hh h1` at 64. Scale letter-spacing with the size (about −3 px at 64 → −2.3 px at 48).

## 4. Figma only: needs building

**Mobile hero** (`3032:7454`)
- Single full-bleed bottle photo, not three strips.
- Inset 8 px, 24 px radius, 374 × 760.
- The ring sits below the buttons.
- Headline 40, subtext 16, buttons 12.

**Mobile shop section**
- One product per row.
- Four products only: Ring PRO, M1, Home, Photon.
- Voyager charger, Cardio and Cycle are removed.
- On the web it is still a 7-card grid.

**Mobile headings**
- 40 for section headings.
- How-it-works step titles are 40.
- Pre-tax sentence (30) and card titles (28 / 32) were intentionally left alone.

**Route result on all other screens**
- Desktop (3 variants) and mobile (4) now have no "4 STEPS" chip, no step-1 divider and no extra Steps padding.

## 5. How it works card background (asset for the dev)

The animated silk is the existing WebGL shader on the page (`#vsh`).
For sharing or non-WebGL use, a seamless 10 s loop was rendered from the same shader:
- `share/silk-bg.mp4` (7 MB, H.264) and `share/silk-bg.webm` (1 MB, VP9)
- 1480 × 1320, 60 fps, square corners (round them in CSS)
- `share/silk-preview.html` plays it
- A Lottie was tried and dropped (it can't reproduce this look).

## 6. Files

| File | Status |
|---|---|
| `hsa-fsa-v3.html` | modified (all web changes above) |
| `assets/hsa/hero-bottle.webp` | new, needed by the hero |
| `share/silk-bg.mp4`, `silk-bg.webm`, `silk-preview.html` | new, optional |
| `share/ring-pro-hsa-fsa-v3.html` | **stale** (old hero). Rebuild before sharing |

None of this is committed or pushed yet.

## 7. Open questions

- Should the pre-tax sentence and card titles on mobile follow the 40 / 48 rule, or stay as sub-headings?
- The "Not sure" pill does not turn black when clicked on the web (HSA stays selected). This was already the case before today and has not been investigated.
