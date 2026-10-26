# slim

Tiny CSS foundation: reset, typography, one button, one container. Pure CSS, no build step, no dependencies. About 1.1 KB gzipped.

## What's included

| File | Does |
|---|---|
| `vars.css` | The 8 custom properties and the `.white` / `.dark` theme classes |
| `reset.css` | Box-sizing, zero margins, responsive media, form fonts inherit, reduced motion |
| `type.css` | Fluid headings, body, lists, links, `hr`, `blockquote`, `code`, `pre`, focus ring |
| `layout.css` | `.container` |
| `buttons.css` | `button`, `.btn`, `input[type=submit\|button\|reset]` |
| `index.css` | Layer order and imports |

Nothing else. No forms, tables, utilities or components. Those belong to your app.

## Install

**Vite / Laravel**

```css
/* resources/css/app.css */
@import "./slim/index.css";
```

```blade
@vite('resources/css/app.css')
```

**No bundler**

```html
<link rel="stylesheet" href="/css/slim.min.css">
```

`slim.min.css` is every file concatenated in order and minified. Use either `index.css` or `slim.min.css`, not both.

## Variables

```css
:root {
  --font: ui-sans-serif, system-ui, sans-serif;
  --font-mono: ui-monospace, Menlo, Consolas, monospace;
  --container: 72rem;
  --radius: .5rem;
}
body {
  --bg: light-dark(#fff, #111);
  --fg: light-dark(#111, #eee);
  --line: light-dark(#ddd, #333);
}
```

| Variable | Used for |
|---|---|
| `--font` | Body and heading font |
| `--font-mono` | `code`, `pre` |
| `--bg` | Page background, button text |
| `--fg` | Text, button background |
| `--line` | `hr`, blockquote rule, `code` and `pre` background |
| `--container` | Max width of `.container` |
| `--radius` | Buttons, `pre` (and half of it on `code`) |

### Overriding

Fonts, `--container` and `--radius` can be overridden on `:root`:

```css
:root {
  --font: "Inter", sans-serif;
  --radius: .25rem;
}
```

`--bg`, `--fg` and `--line` are set on `body`, so override them there. Setting them on `:root` alone has no effect.

```css
body {
  --bg: light-dark(#fafaf9, #0c0a09);
  --fg: light-dark(#1c1917, #e7e5e4);
}
```

Put overrides in unlayered CSS (outside any `@layer`). It always beats the framework, with no `!important`.

## Theme

Follows the system by default. Force one on the body:

```html
<body class="white">
<body class="dark">
```

These classes work on `<body>` only. The colors are `light-dark()` values, which resolve against the `color-scheme` of the element that defines them (`body`), not the element where they're used.

To toggle at runtime:

```js
document.body.className = document.body.className === 'dark' ? 'white' : 'dark';
```

## Usage

**Container**

```html
<div class="container">…</div>
```

Centered, `min(100% - 2rem, var(--container))`.

**Button**

```html
<button>Save</button>
<a class="btn" href="/signup">Sign up</a>
<input type="submit" value="Send">
<button disabled>Disabled</button>
```

One design. For a different look, override in your own CSS:

```css
.btn-outline {
  background: none;
  color: inherit;
  box-shadow: inset 0 0 0 1px var(--line);
}
```

**Typography**

Plain HTML gets styled: `h1`–`h6`, `p`, `ul`, `ol`, `a`, `blockquote`, `code`, `pre`, `hr`. Headings use `clamp()` and scale fluidly. Paragraphs cap at `68ch`. Top margins are applied with `:not(:first-child)`, so there's no need for spacing classes inside flowing content.

## Layers

```css
@layer vars, reset, base, layout, components;
```

Your CSS sits above all of them. To add your own layer, declare it after:

```css
@layer vars, reset, base, layout, components, app;
```

## Customizing

Delete a line from `index.css` to remove a file. Files don't depend on each other except for the variables in `vars.css`.

## Rebuilding `slim.min.css`

```bash
cat vars.css reset.css type.css layout.css buttons.css > slim.css
```

Then add `@layer vars, reset, base, layout, components;` as the first line and minify with any tool. Skip the `@import` lines from `index.css`.

## Browser support

Needs native CSS nesting, `@layer`, `light-dark()` and `color-mix()`. That means current evergreen browsers (Chrome 123+, Safari 17.5+, Firefox 120+). Older browsers get no theming and no nesting.
