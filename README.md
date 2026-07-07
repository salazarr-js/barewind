# 🍃 Barewind

> Classless base styles for Tailwind CSS — rapid prototyping with nothing but semantic HTML.

Tailwind resets every element to nothing; barewind puts beautiful defaults back. Write plain semantic HTML and it just looks good — **zero classes, zero components, zero JS** — then customize with Tailwind utilities when (and only when) you need to.

```html
<!-- A prototype in nothing but semantic HTML. No classes. Looks good anyway. -->
<h1>Hello</h1>
<p>Some text with a <a href="#">link</a>.</p>
<button>Do the thing</button>
<input type="email" aria-invalid="true" />
```

## Install

> 🚧 **Early development** — not published to npm yet.

```bash
pnpm add -D barewind
```

```css
@import 'tailwindcss';
@import 'barewind';
```

That's it — no plugin config, no JS. Themeable through CSS variables, dark mode included.

## Docs

- [**Brief**](./docs/brief.md) — what barewind is and why: the problem, the concept, inspirations, and scope.
- [**Research**](./docs/research.md) — the deep dive into the inspiration sources that shaped the design.
- [**Plan**](./docs/plan.md) — the current phase and its steps.

## License

[MIT](./LICENSE)
