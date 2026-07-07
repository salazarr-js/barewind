# 🍃 Barewind — project brief

## Motivation

**Beautiful styles by default, for rapid prototyping, using nothing but semantic HTML.**

When you're sketching an idea — a demo, a spike, an internal tool, a playground page — you want to write `<h1>`, `<p>`, `<form>` and see something presentable, not spend the first hour re-inventing button and input styles, copy-pasting utility chains, or installing a full UI framework just to use a fraction of its components. The prototype phase needs *zero styling decisions*.

## Problem

Tailwind's preflight resets every element to nothing — by design. That's right for app teams building design systems, but it makes Tailwind hostile to prototyping: every page starts ugly (bare headings, invisible links, unstyled buttons and forms) until you class everything. The existing options each miss:

- **Classless / minimal frameworks** (Pico, water.css, classic Skeleton) solve it beautifully — but outside Tailwind: they don't speak its theme, and their styles fight its utilities instead of composing with them.
- **Tailwind UI kits** (daisyUI, Skeleton, Ripple UI) integrate perfectly — but through *class vocabularies* (`btn`, `card`). Bare HTML stays ugly.
- **`@tailwindcss/typography`** styles bare content, but only inside a `prose` wrapper, and it covers prose — not buttons, forms, or interactive elements.
- **`@tailwindcss/forms`** normalizes form controls so utilities can style them — but it's deliberately a *reset*, not defaults: fields still look unstyled until you add classes, and it covers only forms.

**barewind fills the gap: classless defaults, native to Tailwind.**

## Concept

A thin layer that sits right after Tailwind and gives every semantic HTML element a good default look:

- **Classless.** Plain tags look right with zero classes, zero components, zero JS.
- **Native to Tailwind.** Not a parallel framework — it draws from and feeds back into Tailwind's theme, so defaults and utilities share one design language.
- **Themeable.** The whole look derives from a small set of semantic design variables; re-skinning (including dark mode) means changing values, not rules.
- **Never in the way.** Defaults lose to any utility class. As a prototype hardens into a product, you layer Tailwind on top — nothing is thrown away, nothing fights back.

## Inspiration

- [Pico CSS](https://github.com/picocss/pico) — the classless philosophy and element wiring
- [water.css](https://github.com/kognise/water.css) — pure classless drop-in defaults
- [daisyUI](https://github.com/saadeghi/daisyui) — minimal semantic token API and theme model
- [Skeleton](https://github.com/skeletonlabs/skeleton) — token machinery and modern theming
- [Skeleton (classic)](https://github.com/dhg/Skeleton) ([getskeleton.com](http://getskeleton.com/)) — "a starting point, not a framework"
- [Pure CSS](https://github.com/pure-css/pure) — staying tiny, opt-in modules
- [Ripple UI](https://github.com/Siumauricio/rippleui) — Tailwind plugin packaging reference
- [@tailwindcss/forms](https://github.com/tailwindlabs/tailwindcss-forms) — form-control normalization groundwork
- [@tailwindcss/typography](https://github.com/tailwindlabs/tailwindcss-typography) — bare-content styling precedent (scoped to `prose`)
- [Open Props](https://open-props.style/) — design tokens as plain CSS variables: the token-first, framework-agnostic spirit

## Scope — elements

Full coverage of semantic HTML — the bar is [Pico's classless showcase](https://github.com/picocss/examples/blob/master/v2-html-classless/index.html): a complete page, including a native modal and full forms, with zero classes.

### Will have

| Group | Elements |
|---|---|
| Document & landmarks | `body`, `header`, `main`, `footer`, `nav`, `section`, `article`, `hgroup` |
| Typography | `h1`–`h6`, `p`, `small`, and inline semantics: `strong`, `em`, `mark`, `abbr`, `cite`, `ins`, `del`, `s`, `u`, `sub`, `sup` |
| Links | `a` (+ `hover`/`focus` states) |
| Buttons | `button`, `input[type=submit/reset/button]`, `[role=button]` |
| Forms | `form`, `label`, **groups** (`fieldset`/`legend`, `[role=group]`), text-like inputs (`text`, `email`, `search`, `date`, `time`, …), `select`/`option`, `textarea`, `checkbox`/`radio`/`range`/`[role=switch]`, `file`, `color` |
| Form states | `:focus`, `:disabled`, `readonly`, valid/invalid via `[aria-invalid]`, loading via `[aria-busy]` |
| Content | `ul`/`ol`/`li`, `dl`/`dt`/`dd`, `details`/`summary`, `blockquote`, `hr`, `code`/`kbd`/`samp`/`pre`, `table`, `figure`/`figcaption`, `img` |
| Native interactive | `dialog` (native modal), `details`/`summary` (accordion), `progress`, `meter` |

### Won't have (non-goals)

- **No component classes.** No `btn`, no `card`. If it needs a class, it's out of scope (a tiny set of opt-in variants may be considered later — classless stays the core).
- **No JS.** Nothing to import, hydrate, or configure at runtime.
- **No layout system.** Grids, containers, and spacing scales are Tailwind's job.
- **No raw color palette.** Semantic roles only; hues come from Tailwind's own palette.
