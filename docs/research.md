# 🍃 Barewind — research

The learnings and extracted data behind barewind's decisions: a deep dive into how **Pico**, **daisyUI**, and **Skeleton** model design tokens and style elements, plus findings from the incubator experiment where the approach was first proven.

## The three token systems, side by side

| Dimension | **Pico** (classless) | **daisyUI** | **Skeleton** |
|---|---|---|---|
| Prefix | `--pico-*` | `--color-*`, `--radius-*` (Tailwind namespaces) | Tailwind namespaces + `--typo-{role}--{prop}` |
| Raw palette tier | ❌ none (hex inline) | ❌ none (oklch inline) | ✅ 50–950 ramp per family |
| Semantic colors | `primary/secondary/contrast` + surface/text/muted + form-state | `primary/secondary/accent/neutral` + `base-100/200/300` + `info/success/warning/error` | `primary…tertiary/success/warning/error/surface` |
| Foreground token | `-inverse` | `-content` | `-contrast-{shade}` |
| Color space | hex/rgb (v2) | **oklch** | **oklch** |
| Radius tokens | 1 (`--border-radius`) + local overrides | 3 roles: `selector/field/box` | 2 roles: `base/container` |
| Typography tokens | ✅ full (families, h1–h6 scale, spacing) | ❌ (delegates to Tailwind) | ✅ (base/heading/anchor roles + `--text-*`) |
| Dark mode | 3 duplicated blocks, gated on `:not([data-theme])` | themes as `[data-theme]` blocks, `--prefersdark` | **`light-dark()` + `color-scheme`** (one switch, no duplication) |
| Theme surface | ~80 vars, re-declared per theme | **~28 vars** per theme (minimal) | ~11 inputs/family → derives ~400 |

## What each does uniquely well

### daisyUI — minimal semantic API

~28 variables define an entire theme: ~4 role colors, a 3-step `base-100/200/300` surface ramp with a single `base-content` text color, status colors, and shape tokens. No raw palette is exposed at all — oklch literals live inline and are only ever referenced through semantic names.

Its shape system buckets components into **three families** — `selector` (checkbox, badge), `field` (button, input), `box` (card, modal) — with one radius token each, so "make all my inputs less rounded" is a one-line change.

Themes are flat `[data-theme]` variable blocks; one is flagged `--prefersdark` to follow the OS. Each theme also sets native `color-scheme` so form controls and scrollbars match.

### Skeleton — derivation machinery and modern theming

Author ~11 ramp values per color family; contrasts, light/dark pairings, and the type scale are all *derived*. Three registration layers: raw values in `:root`, static tokens in `@theme`, computed tokens in `@theme inline` (so `var()` chains resolve per-element).

Its dark mode is the cleanest of the three: every theme-sensitive token is written **once** as `light-dark(lightValue, darkValue)`, and flipping the native `color-scheme` property swaps the entire palette — no `.dark:` variants, no duplicated blocks.

Two more high-leverage ideas: **pairing tokens** (`surface-100-900` = "light 100, dark 900", self-inverting across modes) and **global multipliers** (`--text-scaling`, `--spacing`) that rescale all type/space from one variable.

### Pico — classless element wiring

Bare elements paint from a small set of *generic role vars* (`--pico-color`, `--pico-background-color`, `--pico-border-color`, `--pico-box-shadow`); every variant/state (`:hover`, `[type=reset]`, `[aria-invalid]`) only **re-points** those vars at a different semantic token. One line — `h1 { --pico-color: var(--pico-h1-color) }` — restyles a whole element.

Other patterns worth keeping:

- **`aria-invalid` as the validation switch** — `[aria-invalid=true|false]` drives red/green borders + focus rings. Classless and accessibility-correct; no `.error` class.
- **Focus as a box-shadow ring** (`outline: none` + `box-shadow: 0 0 0 <w> <color>`) — follows `border-radius`, animates with the transition token. Inputs get a thinner ring than buttons.
- **One spacing unit + `calc()`** instead of a token zoo — even input height is computed from line-height + spacing + border tokens.
- **Element rules wrapped in `:where()`** — zero specificity, so any user rule or utility class overrides the defaults without `!important`.
- **Family aliasing** — `--primary-border: var(--primary-background)` — so one base color cascades through a whole family.

## Principles all three agree on

1. **Role-based tokens, never hue names** at the call site (`primary`/`surface`/`error`, not `blue-500`). daisyUI and Pico don't even expose a raw palette.
2. **A foreground token paired with every fill** (`-content` / `-contrast` / `-inverse`) — the contrast decision lives in the token, decided once by the theme author.
3. **OKLCH** for color (daisyUI + Skeleton; Tailwind's own palette is already oklch).
4. **A handful of radii by role**, not an `sm/md/lg/xl` scale.
5. **One spacing base unit + `calc()`/utilities**, not a token zoo.
6. **`aria-invalid` as the validation switch** — classless and a11y-correct.

## Incubator findings

The approach was first built and proven in a working incubator experiment — a Tailwind v4 project styling bare elements through semantic tokens, with a styleguide page as the living test. What the experiment established:

- **Two-tier tokens work.** Runtime theme vars in `:root` (the only place values live) + `@theme inline` mapping them to semantic names gives *live* Tailwind utilities (`bg-primary`, `text-fg`, `ring-error`) that re-theme when the `:root` vars change.
- **`@theme inline` is load-bearing.** With plain `@theme`, Tailwind resolves the intermediate variable at `:root` and utilities freeze to the computed value — overriding the source var later does nothing. With `inline`, utilities emit `var(--token)` directly and resolve per element, so theme overrides cascade.
- **Pico's role-var indirection ports cleanly to Tailwind.** Form fields paint `border-color: var(--field-border)` once; `:focus-visible`, `[aria-invalid=true|false]`, and `:disabled` only re-point `--field-border`. One paint rule, many states.
- **Native controls are cheap to theme.** `accent-color: var(--primary)` covers checkbox/radio/range in one line — no `appearance-none` re-rendering needed for prototype-grade fidelity.
- **A passing Tailwind build doubles as a token test.** Tailwind v4 errors on any unknown utility inside `@apply`, so compiling proves every token resolved.
- **Classless discipline keeps the surface small:** only add a token when a bare element consumes it. The experiment shipped ~12 color/shape tokens total — in the same ballpark as daisyUI's ~28 and far from Skeleton's ~400.

## Prior art — what we take, what we leave

| Project | Take | Leave |
|---|---|---|
| Pico CSS | Classless philosophy; role-var indirection; `aria-invalid`; focus rings; `:where()` | Its own token namespace (`--pico-*`); non-Tailwind distribution; hex/rgb colors |
| water.css | "Just drop it in" bar for simplicity | No theming surface beyond light/dark |
| daisyUI | Tiny semantic token API (~28 vars/theme); `-content` pairs; shape families; `[data-theme]` model | Component class vocabulary (`btn`, `card`) |
| Skeleton | `light-dark()` + `color-scheme` theming; `@theme` / `@theme inline` split | 50–950 ramps (400+ vars); component classes |
| [Skeleton (classic)](http://getskeleton.com/) | "Starting point, not a framework" attitude; bare typography/forms/buttons styled by default | Its 2014-era CSS itself |
| Pure CSS | Size discipline; opt-in modules | Class-based grid/forms |
| Ripple UI | Plugin packaging reference | daisyUI-style classes |
| [@tailwindcss/forms](https://github.com/tailwindlabs/tailwindcss-forms) | Cross-browser form-control normalization (`appearance-none`, consistent select/checkbox rendering) | Reset-only philosophy — barewind ships opinionated defaults |

## Sources

- Pico — [CSS variables](https://picocss.com/docs/css-variables) · [`pico.classless.css`](https://github.com/picocss/pico/blob/main/css/pico.classless.css) · [classless showcase](https://github.com/picocss/examples/blob/master/v2-html-classless/index.html)
- daisyUI — [colors](https://daisyui.com/docs/colors/) · [themes](https://daisyui.com/docs/themes/) · [utilities](https://daisyui.com/docs/utilities/) · [`light.css`](https://github.com/saadeghi/daisyui/blob/master/packages/daisyui/src/themes/light.css)
- Skeleton — [core API](https://www.skeleton.dev/docs/svelte/get-started/core-api) · [`theme.css`](https://github.com/skeletonlabs/skeleton/blob/main/packages/skeleton/src/base/theme.css) · [`globals.css`](https://github.com/skeletonlabs/skeleton/blob/main/packages/skeleton/src/base/globals.css)
- [MDN — `light-dark()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark)
- [Tailwind v4 — theme variables](https://tailwindcss.com/docs/theme)
