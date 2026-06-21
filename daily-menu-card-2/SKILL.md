---
name: daily-menu-card
description: Turns a casual description of a meal (what someone ate today, at a restaurant, or cooked at home) into a polished, fine-dining-style menu card — organized into starters/mains/dessert/beverages with elegant typography, like a real restaurant menu. Outputs as a standalone, printable HTML file. Use this skill whenever the user describes what they ate or are eating ("我今天吃了...", "today I had...", "晚饭吃了..."), asks to turn a meal into a menu, wants to record or journal a meal, or wants something shareable/printable to show what a meal looked like — even if they never say the word "menu". Also trigger if the user wants to design what they're COOKING/serving (e.g. "帮我把今天要做的菜做成一个菜单") as a shareable plan. Do not trigger for restaurant business menus meant for actual pricing/ordering use, or recipe instructions — this is for personal, expressive "meal-as-menu" cards.
---

# Daily Menu Card

## What this actually automates

The fun (and the joke) of this skill is treating an ordinary meal — including fast food, a quick home-cooked dinner, a single snack — with the visual seriousness of a Michelin tasting menu. Someone says "我今天吃了麦当劳的薯条和可乐" and gets back a card that reads like it belongs at a white-tablecloth restaurant. That contrast is the whole charm. Don't undersell humble food by writing it plainly — the elevated language IS the joke and the point.

The repeatable part of this workflow — the part worth automating — is:
1. Taking freeform, unstructured meal descriptions (typed or voice-transcribed, so expect run-on sentences, no punctuation, casual phrasing)
2. Classifying each item into a course category
3. Writing a short ingredient line under each dish name
4. Rendering it all into a consistent, good-looking HTML card

Everything else (style choice, language, restaurant name) is a thin layer of personalization on top — keep that layer as light as possible. Don't turn this into a 10-question intake form.

## Step 1: Ask exactly one round of quick questions

Before building, ask these three things together in a single turn (use `ask_user_input_v0` if available — this is a textbook elicitation case: ambiguous open preferences, not a yes/no the user already answered):

1. **Language**: 中文 / English / 双语 Bilingual / Other (let them type a language)
2. **Visual style**: Use a preset style / I'll upload my own background image
3. **Restaurant or meal name** (optional, free text or "skip"): if they skip, don't block — fall back to a generic header (see Step 6).

If the user already answered any of these in their original message (e.g. "帮我做个双语的"), don't ask again for that one — only ask what's still unknown.

**If they chose "upload my own background image"**, ask one follow-up once the image is in hand: use it directly as the card background with text overlaid on top, or only pull its colors/texture as a design reference and build a fresh background around that palette? (Direct overlay only works well if the image is calm enough to read text over — if it's busy/high-contrast, say so and lean them toward the palette-extraction option.)

That's it. Don't ask about price, dish descriptions, aspect ratio, or fonts — those are handled automatically below.

## Step 2: Parse the meal into courses

Sort every item the user mentioned into:

- **前菜 Starters** — cold dishes, salads, snacks, appetizers, anything light eaten first
- **主菜 Mains** — the substantial part of the meal: rice, noodles, proteins, hot dishes, sandwiches, a fast-food combo's burger
- **甜品 Dessert** — sweets, fruit, ice cream
- **饮品 Beverages** — drinks of any kind: coffee, tea, soda, boba, alcohol, juice

**Only include a category if there's something in it.** A single sandwich-and-coffee lunch becomes a two-section card (Mains + Beverages) — don't pad it out with empty Starters/Dessert headers just to hit four sections. A forced four-course structure on a two-item meal looks awkward, not fancy.

When an item is ambiguous (a soup, a side of fries, a snack-sized portion), use judgment based on context — what role did it play in this meal? Fries next to a burger are part of the main; fries alone as an afternoon snack might be the whole "course." Don't ask the user to clarify this — just make a reasonable call.

## Step 3: List ingredients under each dish (not a tasting-note paragraph)

**Exception — beverages get no ingredient line.** Items in 饮品/Beverages never get a `.dish-ingredients` sub-line, even for branded or composite drinks (e.g. 可乐, 杏仁露, iced americano). Just the dish name (primary + secondary language). This applies regardless of language setting.

Default behavior: every dish gets a short line underneath its name listing what's actually in it — comma/顿号-separated components, the way real menus (including upscale ones) actually do it. This is not a poetic sentence and not a sales pitch; it's a plain, restrained ingredient breakdown. That restraint is what reads as "real menu" rather than a list with captions.

If the user's description doesn't specify what's in something (e.g. they just said "薯条"), use general knowledge of what that dish typically contains rather than asking them to itemize it — don't block on this.

If the user explicitly says they only want dish names with no sub-line at all, that's a valid opt-out — but it's not the default.

**Examples** (don't reuse verbatim — generate fresh ones based on what was actually described):

| Input | Dish name on card | Ingredient line |
|---|---|---|
| 麦当劳巨无霸 | 巨无霸 Big Mac | 牛肉饼、特制酱汁、生菜、芝士、酸黄瓜、芝麻面包 |
| 麦当劳薯条 | 黄金薯条 Golden Fries | 马铃薯、海盐 |
| iced americano | Iced Americano | Espresso, water, ice |

Keep it to real components, not flowery adjectives — see `references/style-presets.md` for why restraint reads as expensive (the actual EMP menu famously runs about 28 words for its entire food menu).

## Step 4: Price — opt-in only, never asked

Don't ask about price. Only include it if the user's original message volunteers a number or a clear cost cue ("花了38块", "199 for the tasting menu", "$12 combo"). If nothing was mentioned, the card has no price column at all — a price-free menu reads more elevated anyway, which works in this skill's favor.

## Step 5: Brand and logo handling

If the meal involves a real, named brand (麦当劳, Starbucks, 海底捞, etc.), you can absolutely lean into it for atmosphere — but never reproduce an actual trademarked logo, wordmark, or brand typeface. Instead:
- Reference the brand by name in elegant type, the same way any restaurant name would appear
- You can nod to brand-associated colors as an accent (e.g. a warm red accent line for a burger-chain meal) without it reading as a logo recreation
- Treat it as "a menu inspired by a McDonald's meal," not "the McDonald's menu" — the distinction matters

## Step 6: Header / title block

- If the user gave a restaurant or meal name: use it as the card's title (e.g. "新荣记" or "Sunday Brunch at Home")
- If they skipped it: fall back to the date in a clean format, or a generic line like "今日菜单 Today's Menu" — never block generation on a missing name

## Step 7: Build the HTML

Read `references/style-presets.md` for the two full style specs (colors, fonts, CSS) before writing any code, and start from `assets/template.html` as your base rather than writing the structure from scratch — it already has the print rules, the responsive container, and both theme variants wired up via CSS variables.

Key technical points:
- **This is a real deliverable, not a quick inline visual.** Build it with the file-creation tools and save to `/mnt/user-data/outputs/`, then present it — don't use the inline visualizer for this; the user wants a file they can keep, print, and share.
- **Default layout is centered, on a clean white background** — think minimalist wedding/party menu cards, not an ornate restaurant booklet. Typography and spacing carry the elegance; there's no texture, no price-leader dotted lines, no boxed sections. Left-alignment is a fine variant too (it suits cards that include prices, where a left name / right price layout reads naturally) — use judgment, but centered-on-white is the default unless something about the content calls for left alignment.
- **Aspect ratio**: default canvas is a 3:4-proportioned card (portrait, like a real menu) at a fixed width — but let height grow naturally with content rather than forcing a hard aspect-ratio lock. A 3-item meal and an 8-item feast should both look proportioned, not stretched or cramped. Only ask the user about ratio if they bring it up themselves.
- **Print-friendly**: include `@media print` rules — remove any UI chrome, set sensible margins, make sure any background colors actually print (`print-color-adjust: exact`).
- **Bilingual layout**: when both languages are requested, default convention is the primary language larger/bolder with the second language in smaller italic directly beneath each line (dish names and category headers both). Pick whichever language the user led with as "primary" if unspecified.
- **Fonts**: load via Google Fonts CDN per the chosen preset (see references). Always set a solid fallback stack (serif/sans system fonts) in case the file is opened offline, since this is meant to be saved and reopened later.

## Step 8: Show it and ask for reactions

After generating, briefly point out the language/style/header choices you went with so the user can sanity-check at a glance, rather than silently dropping a file. Keep this short — the card speaks for itself.
