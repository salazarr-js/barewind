# 🍃 Barewind — plan

Current phase: **research, redone properly** — *paused, last reviewed 2026-08-03*. Implementation planning comes after — first we widen the inspiration pool and re-evaluate every source with the same lens.

## 1. Find more inspiration sources

Sweep for candidates beyond the current list, in every relevant family:

- **Classless CSS frameworks** — the direct competitors of the look (e.g. MVP.css, Simple.css, Sakura, new.css, Tacit, Marx, Bahunya, and whatever else a fresh search surfaces).
- **Minimal class-light frameworks** — Milligram, Chota, Spectre-likes: small class vocabularies with strong bare-element defaults.
- **Token systems** — Open Props, Radix Colors, shadcn/ui theming conventions, Panda CSS tokens: how tokens are named, tiered, and distributed.
- **Tailwind ecosystem** — any plugin/layer styling bare elements or shipping semantic tokens we haven't covered yet.

Output: an updated candidate list with a one-line reason each; drop candidates that add nothing new over an existing source.

## 2. Re-evaluate every inspiration source

Go back to **each** source — current list and new finds — and evaluate it against one shared rubric, from its latest version and actual source code (not memory, not docs alone):

| Rubric dimension | Question |
|---|---|
| Tokens | Which variables exist, how are they named, tiered, and scoped? |
| Element coverage | Which bare elements does it style, and how completely? |
| Theming | How does re-skinning work, and how many lines does a theme cost? |
| Dark mode | What mechanism, and at what duplication cost? |
| Forms & states | How are focus, disabled, validation, and loading handled? |
| Size & modularity | Total footprint; can you take parts? |
| Take / leave | What barewind adopts from it, what it deliberately avoids |

Output: the research doc rewritten from these evaluations — every source gets a section, the comparison tables regenerated, conclusions re-derived from the full pool.

## Done when

- Every source in the brief's inspiration list (plus new finds) has been evaluated against the rubric, from primary sources.
- The research doc reflects the full pool, and the brief's inspiration list is updated with the keepers.
- We can defend every take/leave decision with a citation to the evaluated source.
