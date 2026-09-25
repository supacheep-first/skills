<!-- diagram-design-profile
name: butter|green
slug: butter-green
source-url: none
created: 2026-09-12
updated: 2026-09-12
notes: butter paper, forest-green accent, Sarabun/Bai Jamjuree Thai-capable type; Thai label rules
-->
# Style Guide

**The single source of truth for colors, typography, and tokens.** Every diagram draws from this — not from hex values inlined in other reference files. If you want to change the visual skin of Diagram Design, change this file.

Active skin is **butter|green** — butter paper, near-black green ink, forest-green accent, sage-grey muted, olive-gold link. Dark variant is deep olive paper with butter-toned text. Swap these values (or run [`onboarding.md`](onboarding.md)) and every new diagram inherits the new skin without touching any type-specific logic.

To generate your own from a website URL, see [`onboarding.md`](onboarding.md).

---

## Tokens

### Semantic roles

Every token is referred to by **semantic role**, not by its hex value. Type references (`type-*.md`) and SKILL.md say `accent`, not `#f7591f`.

| Role | Purpose | Default (light) | Default (dark) |
|---|---|---|---|
| `paper` | Page background, default node fill | `#fbf6e4` (butter) | `#1e2a1c` (deep olive) |
| `paper-2` | Diagram container bg, secondary fill | `#f3ecd3` | `#26342a` |
| `ink` | Primary text, primary stroke | `#1f2a1e` (near-black green) | `#f3eccf` (butter cream) |
| `muted` | Secondary text, default arrow stroke | `#596856` (sage-grey) | `#b9c2ae` |
| `soft` | Sublabels, boundary labels | `#75836f` | `#93a08e` |
| `rule` | Hairline borders | `rgba(31,42,30,0.12)` | `rgba(243,236,207,0.12)` |
| `rule-solid` | Stronger borders, baselines | `#e6dec2` (dry straw) | `#3a4a3c` |
| `accent` | Focal / 1–2 max per diagram | `#2f6b3a` (forest green) | `#8fc79a` |
| `accent-tint` | Fill for accent-bordered boxes | `rgba(47,107,58,0.08)` | `rgba(143,199,154,0.10)` |
| `link` | HTTP/API calls, external arrows | `#75672a` (olive gold) | `#e3cf7a` |
| `node` | Backend / API / step node fill (the skin's "white") | `#fffdf5` (butter-white) | `#26342a` |

> **Brand palette source:** butter|green — `butter #fbf6e4`, `butter-white #fffdf5`, `near-black green #1f2a1e`, `forest green #2f6b3a`, `sage-grey #596856`, `olive gold #75672a`. `muted`, `soft`, and `link` are darkened from the HTML-artifact palette so they clear WCAG AA on butter at diagram sizes (muted 5.5:1, link 5.2:1, soft 3.7:1). Semantic colours for status/severity (red = critical, amber = warning) stay a separate layer and never count as `accent`.

> **Note:** The pre-baked example HTML files in `assets/` were built under an earlier skin. Regenerating them against the current `style-guide.md` is a v5.1 task. New diagrams the skill produces will use the tokens above.

### Inversion rule (light → dark)

Any `rgba(31,42,30, X)` in light becomes `rgba(243,236,207, X)` in dark. Same opacities, RGB flipped. The accent lightens to `#8fc79a` to read on dark olive paper.

### Series palette (multi-series chart types only)

A small set of desaturated, editorial-tone colors for chart types that genuinely need to distinguish multiple overlapping entities (currently: **radar**). The "1-focal" rule still holds — `accent` is reserved for the focal series; the palette below covers the rest.

| Token | Light | Dark | Notes |
|---|---|---|---|
| `series-1` | `#7d6b8f` (heather) | `#a898b8` | Non-focal series — sage was dropped: it collides with the green `accent` and `muted` |
| `series-2` | `#5e7a9b` (dusty-blue) | `#82a0c0` | Non-focal series |
| `series-3` | `#b8915a` (mustard) | `#d3ad7a` | Non-focal series |
| `series-4` | `#9c6b50` (rust-brown) | `#b88670` | Non-focal series |
| `series-5` | `#6e6479` (slate) | `#8d8298` | Non-focal series |

Fills sit at `0.18` opacity light, `0.22` dark; strokes use the full color. **Don't backfill these tokens to non-chart types** — architecture, swimlane, etc. continue to use muted-ink variants. The series palette is opt-in for diagrams where overlapping shapes demand distinguishable color, not a license to add color elsewhere.

### Terminal skin (opt-in alternate)

A self-contained palette for the terminal-window primitive (see [primitive-terminal.md](primitive-terminal.md)) — a CLI-chrome register for dev-tool posts and technical social cards. It does not replace the default skin above and isn't affected by onboarding; it's a second, fixed skin you opt into per-diagram.

| Token | Hex | Purpose |
|---|---|---|
| `terminal-page` | `#0a0a0a` | Page background behind the window |
| `terminal-paper` | `#141414` | Window body, node fill |
| `terminal-bar` | `#1b1b1b` | Titlebar strip |
| `terminal-border` | `#2b2b2b` | Window border, hairlines |
| `terminal-ink` | `#f5f5f5` | Primary text, primary stroke (same white-smoke as default `ink`) |
| `terminal-muted` | `#9a9a9a` | Secondary text, sublabels, ring stroke |
| `terminal-soft` | `#5c5c5c` | Tertiary — inactive dots, spokes |
| `terminal-accent` | `#ff5a36` | The one accent — focal station, prompt sign, active dot |
| `terminal-accent-tint` | `rgba(255,90,54,0.12)` | Fill for accent-bordered boxes |

**1-accent rule still holds.** Everything that isn't `terminal-ink` or `terminal-muted`/`terminal-soft` should be `terminal-accent` — never introduce a second hue.

---

## Typography

| Role | Family | Size | Weight | Usage |
|---|---|---|---|---|
| `title` | Bai Jamjuree | 1.75rem | 500 | Page H1 |
| `node-name` | Sarabun (sans) | 12px | 600 | Human-readable labels — Thai and Latin in one face |
| `sublabel` | Geist Mono | 9px | 400 | Port, protocol, URL, field type |
| `eyebrow` | Geist Mono | 7–8px | 500, tracked 0.18em, uppercase | Type tags, axis labels |
| `arrow-label` | Geist Mono | 8px | 400, tracked 0.06em | Arrow annotations |
| `callout` | Sarabun *italic* | 14px | 400 | Editorial asides only |

### Font stack

```html
<link href="https://fonts.googleapis.com/css2?family=Bai+Jamjuree:wght@500;700&family=Sarabun:ital,wght@0,400;0,500;0,600;1,400&family=Geist+Mono:wght@400;500;600&family=Noto+Sans+KR:wght@400;500;600&family=Noto+Serif+KR:wght@400&family=Noto+Sans+TC:wght@400;500;600&family=Noto+Serif+TC:wght@400&display=swap" rel="stylesheet">
```

### Thai labels

Sarabun and Bai Jamjuree carry Thai and Latin in one face, so a Thai `<text>` element needs no family extension — the skin's own stack resolves it. End the stack with generic `sans-serif`, not a named platform face (`Thonburi`, `Tahoma`): the generic already resolves Thai on every OS, and a named fallback fails the skin linter:

```svg
<text font-family="'Sarabun', sans-serif">ขอเปลี่ยนแปลงสิทธิ</text>
```

Geist Mono carries no Thai. Five rules follow from Thai metrics:

- **Floor of 12px.** Tone marks and upper vowels sit above the x-height and vanish below 12px. If a Thai name doesn't fit at 12px, cut the words — don't shrink the type.
- **Sublabels stay Latin.** Ports, URLs, field types, and table/column identifiers are Latin anyway — keep `Geist Mono` there and don't translate them. A Thai sublabel is prose, not a value, and switches register by the next rule.
- **Arrow labels, eyebrows, and legend text switch register.** Those slots are 7–8px Geist Mono, uppercase and tracked; Thai has no case, no mono face, and no legibility there. A Thai label in one of those slots becomes 12px Sarabun at weight 500 with no tracking and no uppercase transform, and its mask rect grows to match (16px tall, width from the budget above, still rounded to a multiple of 4). Latin labels in the same diagram keep the mono treatment.
- **Vertical room.** Thai stacks up to two marks above and one below the baseline. In HTML use `line-height` ≥ 1.5; in SVG give a Thai label 4px more clearance above its baseline than a Latin one — a 12px Thai label with 8px above it clips the tone mark on the box edge. Two-line Thai labels sit 20px baseline-to-baseline, not 16.
- **No automatic wrapping.** Thai has no inter-word spaces, so a renderer cannot find a break. A label that must span two lines gets an explicit `<tspan>` per line at a break the author chooses — never rely on `textLength`, `foreignObject`, or CSS wrapping.

**Width budget** — the per-character formula above already fits Thai. Spacing Thai letters cost the Latin advance (Sarabun ≈ 0.55em, so 0.60em is a safe ceiling); combining vowels and tone marks (U+0E31, U+0E34–U+0E3A, U+0E47–U+0E4E) are nonspacing and cost nothing. `ขอเปลี่ยนแปลงสิทธิ` is 18 code points but 14 spacing characters — size the box for 14. Mixed Thai/Latin in one name stays in Sarabun; don't switch to Geist mid-label.

### Korean labels

Sarabun and Bai Jamjuree carry no Hangul. A Korean `<text>` element extends its own family — never swap the skin:

```svg
<text font-family="'Sarabun', 'Noto Sans KR', 'Apple SD Gothic Neo', 'Malgun Gothic', sans-serif">결제 서비스</text>
```

Both Noto faces ship in the font link above, so the web font resolves before any locally installed one and the same file renders identically on macOS, Windows, and a reviewer's browser. The local families follow it for offline viewing. Page titles extend the title face the same way — `'Bai Jamjuree', 'Noto Sans KR', sans-serif` — or a mixed Latin/Korean title resolves Hangul through the platform's generic sans and the two halves disagree. Google's `css2` endpoint slices Korean by unicode-range, so a diagram with a handful of Korean labels downloads only the slices it touches. The four templates carry both faces because a new diagram may contain Hangul; the shipped Latin-only examples keep the shorter link, since a file with no Hangul has nothing to resolve.

**Width budget.** Measure per character, not per script: **every Unicode wide or full-width character costs 1em, every other character costs its face's Latin advance** (0.60em sans, 0.62em mono), and nonspacing/enclosing marks cost nothing. Sum over the string and multiply by the font size for the text width, then add padding and round the box up to the next multiple of 4. `verify-treemap.py` enforces exactly this text width for treemap cell labels; the padding and rounding are authoring convention, and no other type carries an automatic check, so on those the budget is yours to hold.

Counting by script is the trap. `주문 v2.1` is two full-width syllables and five narrow characters; a formula that tallies Hangul, Latin letters, and spaces silently drops `2`, `.`, and `1` and sizes the box for four of its seven characters. Every rendered character costs something — measure per character, never per script.

Three rules follow from Hangul metrics:

- **Sublabels stay Latin.** Ports, protocols, field types, and URLs are Latin anyway — keep `Geist Mono` there and don't translate them. Hangul in a 9px mono sublabel is unreadable and has no mono face to fall back to.
- **Floor of 12px.** Hangul goes muddy below 12px. If a Korean name doesn't fit at 12px, cut the name — don't shrink the type.
- **Arrow labels, eyebrows, and legend text switch register.** Those slots are 7–8px Geist Mono, uppercase and tracked, which Hangul has neither a face nor legibility for. A Korean label in one of those slots becomes 12px sans at weight 500 with no tracking and no uppercase transform, and its mask rect grows to match (16px tall, width from the budget above, still rounded to a multiple of 4). Latin labels in the same diagram keep the mono treatment.

**Load-bearing rule:** Mono is for *technical* content (ports, commands, URLs, field types). Names go in Sarabun. Page title is Bai Jamjuree. Italic Sarabun is reserved for annotation callouts (see [primitive-annotation.md](primitive-annotation.md)). **Never JetBrains Mono** as a blanket "dev" font, and never Instrument Serif or Geist sans — this skin replaced them.

### Traditional Chinese labels

Sarabun and Bai Jamjuree carry no Han. A Traditional Chinese `<text>` element extends its own family — never swap the skin:

```svg
<text font-family="'Sarabun', 'Noto Sans TC', 'PingFang TC', 'Microsoft JhengHei', sans-serif">請求項比對</text>
```

Both Noto TC faces ship in the font link above, so the web font resolves before any locally installed one and the same file renders identically on macOS, Windows, and a reviewer's browser. The local families follow it for offline viewing. Page titles extend the title face the same way — `'Bai Jamjuree', 'Noto Sans TC', sans-serif` — or a mixed Latin/Han title resolves Han through the platform's generic sans and the two halves disagree. Google's `css2` endpoint slices Chinese by unicode-range, so a diagram with a handful of Chinese labels downloads only the slices it touches.

**Width budget.** The per-character contract above is unchanged: every Unicode wide or full-width character costs 1em, every other character costs its face's Latin advance, and nonspacing marks cost nothing. Full-width punctuation — `（）「」，。：` — is wide and costs 1em as well, which is the part most often dropped.

Counting by script is the trap. `請求項 v2.1` is three full-width characters and five narrow ones; a formula that tallies Han and Latin letters silently drops `2`, `.`, and `1` and sizes the box for six of its nine characters.

Three rules follow from Han metrics, mirroring the Hangul ones:

- **Sublabels stay Latin.** Ports, protocols, field types, and URLs are Latin anyway — keep `Geist Mono` there and don't translate them. Han in a 9px mono sublabel is unreadable and has no mono face to fall back to. A sublabel that is prose rather than a value may be Chinese, but it then switches register by the third rule below.
- **Floor of 12px.** Han packs more strokes than Hangul into the same em box, so the 12px floor binds at least as hard here. If a Chinese name doesn't fit at 12px, cut the name — don't shrink the type.
- **Arrow labels, eyebrows, and legend text switch register.** Those slots are 7–8px Geist Mono, uppercase and tracked, which Han has neither a face nor legibility for. A Chinese label in one of those slots becomes 12px sans at weight 500 with no tracking and no uppercase transform, and its mask rect grows to match (16px tall, width from the budget above, still rounded to a multiple of 4). Latin labels in the same diagram keep the mono treatment.

Simplified Chinese takes the same three rules with the Simplified stack (`'Noto Sans SC'`, `'PingFang SC'`, `'Microsoft YaHei'`). That face does not ship in the link, so Simplified labels still resolve through whatever the viewer has locally.

---

## Stroke, radius, spacing

| Token | Value | Use |
|---|---|---|
| `stroke-thin` | `0.8` | Tag-box outlines, leaf nodes |
| `stroke-default` | `1` | Most strokes |
| `stroke-strong` | `1.2` | Emphasis strokes |
| `radius-sm` | `4` | Small tags |
| `radius-md` | `6` | Node boxes |
| `radius-lg` | `8` | Containers, rings |
| `grid` | `4` | Every coord, size, and gap is divisible by 4 (hard rule) |

---

## Node type → treatment

Semantic role combinations — reference these by name in type specs.

| Type | Fill | Stroke |
|---|---|---|
| `focal` (1–2 max) | `accent-tint` | `accent` |
| `backend` | `node` | `ink` |
| `store` | `ink @ 0.05` | `muted` |
| `external` | `ink @ 0.03` | `ink @ 0.30` |
| `input` | `muted @ 0.10` | `soft` |
| `optional` | `ink @ 0.02` | `ink @ 0.20` dashed `4,3` |
| `security` | `accent @ 0.05` | `accent @ 0.50` dashed `4,4` |

---

## Customizing the skin

Four options:

1. **Run onboarding** — see [`onboarding.md`](onboarding.md). Drop a URL; the skill extracts the palette + fonts and rewrites this file.
2. **Edit by hand** — change the hex values in the tables above. Run the pre-output taste gate afterward to verify the accent still reads as "focal" against the new paper color.
3. **Brand handoff** — paste your existing design-token JSON into a new section here and map its tokens to the semantic roles above.
4. **Client profiles** — save and switch named skins, or bind one to a project, using [`profiles.md`](profiles.md).

### Constraints (don't break these)

- **Contrast**: `ink` must hit WCAG AA on `paper`. `muted` must hit AA on `paper` for 11px+ text.
- **One accent**: pick one color for `accent`. Two accents erases the focal signal.
- **No rainbow palette**: if your brand ships 8 colors, pick 3 (paper, ink, accent). The rest become `muted` variants.
- **Display + text + mono**: three families, not more. This skin is all sans on purpose — Bai Jamjuree (geometric display) for `title`, Sarabun (text cut, Thai-capable) for everything human-readable, Geist Mono for technical values. The display/text contrast does the work a serif would; don't reintroduce Instrument Serif, it has no Thai.
- **Paper is warm-neutral, not pure white**: pure white turns the design sterile. Pick a cream, bone, or light grey with a hint of warmth.
- **Dot pattern is optional, not default**: the 22×22 dot pattern is an opt-in "dotted paper" variant (good for long-form editorial hero diagrams). The default background is a clean `paper` fill, no pattern. When the pattern is enabled, it should sit at ~10% opacity of `ink` on `paper` — visible but quiet.
- **Container is clean by default**: the diagram sits directly on the page paper, no secondary container background or border. A framed variant (`paper-2` bg + `rule` border + 8px radius + padding) is available as an opt-in for card-heavy layouts, but don't reach for it by default — the extra chrome fights the figure.
