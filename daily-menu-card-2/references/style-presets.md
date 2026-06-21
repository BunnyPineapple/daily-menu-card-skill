# Style Presets

Two preset directions, plus guidance for the user-uploaded-image path. Preset A is the validated default — use it unless the user's chosen mood clearly calls for something warmer (Preset B), or they upload their own image.

Both presets share the same underlying philosophy: **restraint reads as expensive.** Eleven Madison Park's entire food menu famously runs to about 28 words. No flowery adjectives, no decoration competing with the typography, no clutter. When in doubt, remove an element rather than add one.

---

## Preset A — 极简白 Clean Minimal (default)

Inspired by minimalist wedding/party menu cards: pure white background, centered text, typography doing all the work. This is the validated default — start here unless there's a clear reason to reach for Preset B.

**Palette**
- Background: pure white or near-white, `#FFFFFF`–`#FDFCFA` — no visible texture
- Ink: warm charcoal, `#2A2622` range — not pure `#000`, but should read as crisp, not muddy
- Muted tone (labels, secondary language, rules): soft warm gray, `#A39A8C` range
- No accent color by default. If the meal involves a strong brand identity, a single hairline rule in a brand-adjacent tone is permissible (see Step 5 in SKILL.md) — but it's the exception, not the rule.

**Typography**
- Dish names (Chinese): `Noto Serif SC`, medium weight
- Dish names (English / secondary language): `Cormorant Garamond`, italic, smaller, set directly beneath the Chinese line
- Category labels: small, wide letter-spacing (0.3–0.35em), uppercase, sans-serif (`Jost` or `Noto Sans SC`), muted tone — these should feel like quiet section markers, not headlines
- Ingredient lines: smallest text on the card, muted tone, no italics needed

**Layout**
- Everything centered: title, date, category labels, dish names, ingredient lines
- Title block: dish-language name large, secondary language small and letter-spaced beneath it, date in small caps below that
- A single thin horizontal rule (28–32px wide, centered) separates the title block from the courses — that's the only ornamental element on the whole card
- Category label, then each dish stacked beneath with generous vertical spacing (margin, not borders, separates dishes)
- No price-leader dotted lines in this preset's centered layout — if a price exists, it's fine to place it as a small line directly under the ingredient line rather than forcing a left/right leader convention designed for left-aligned layouts

---

## Preset B — 暖调质感 Warm Textured (alternate mood)

For when the user wants something cozier/more "fine dining booklet" than the stark minimal default — e.g. an actual nice restaurant meal rather than an everyday/ironic one, or they say they want "more texture" or "more atmosphere." Offer this as an alternate rather than defaulting to it.

**Palette**
- Background: warm ivory/cream, `#F6F1E7`–`#F0E9DA`, with very subtle paper/linen grain (low-opacity radial gradients or fine noise — should read as "nice paper," not be visible as an obvious pattern)
- Ink: deep warm charcoal-brown, `#3A3128` range — avoid pure black and avoid purple
- Accent (rules, leaders): muted gold or terracotta, used sparingly

**Typography**
- Dish names / headers: `Noto Serif SC`, generous letter-spacing on category labels
- Ingredient lines: `Noto Sans SC`, smaller, muted tone
- English subtitles: smaller + lighter sans, beneath the primary line

**Layout**
- This preset suits **left alignment** well, especially if prices are present: dish name left, optional price right, connected by a dotted leader line (classic menu convention) — omit the leader entirely (not just the price) when no price exists, since a dotted line trailing to nothing looks unfinished
- Centered title block at top regardless of body alignment
- Category headers centered, letter-spaced, with a thin rule on either side

---

## Alignment note (applies to both presets)

Centered is the default reading for this skill's overall "minimal party card" identity, but left-alignment is a legitimate variant — particularly useful when prices are involved, since left/right alignment reads more naturally for a name-price pairing than centering does. Use judgment based on content rather than treating alignment as fixed per preset.

---

## User-uploaded background image

Two modes, decided by the one follow-up question in SKILL.md Step 1:

**Direct overlay**: Use the image as a full-bleed background, then add a translucent panel (semi-transparent white, ~85-92% opacity) behind the text block so it stays legible regardless of what's in the photo. Don't put text directly on raw image pixels — busy photos make menus unreadable fast.

**Palette extraction only**: Don't use the image itself in the output. Instead, look at its dominant colors/mood (warm vs cool, bright vs moody, colorful vs muted) and build a fresh background in that spirit using Preset A's centered structure as the base, substituting the palette. Tell the user briefly what palette you pulled out, so it doesn't feel like a black box.

If an uploaded image is visually busy/high-contrast and the user asked for direct overlay anyway, it's fine to proceed — just make the text panel more opaque (closer to 92-95%) to protect legibility rather than refusing or re-asking.
