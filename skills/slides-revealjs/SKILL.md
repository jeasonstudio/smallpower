---
name: slides-revealjs
description: Use when creating or updating presentation decks with reveal.js, including setup, slide authoring, plugins, animation, navigation, and export.
---

# Slides Skill for reveal.js

## Overview

This skill guides AI to create and update reveal.js slides quickly and reliably. Prefer documented reveal.js capabilities over guessed APIs, config keys, or plugin behavior.

## When to Use

- Creating a reveal.js presentation from scratch.
- Migrating existing content into reveal.js slides (HTML or Markdown).
- Enabling advanced behaviors like Fragments, Auto-Animate, Speaker View, Math, Code Highlight, PDF export, or Scroll View.
- Integrating reveal.js into npm/ESM or React projects.

## Core Workflow

1. Choose the integration mode: Static HTML (fastest) or npm/ESM (engineering).
2. Build the minimal deck structure: `.reveal > .slides > section`.
3. Register only required plugins (Markdown, Highlight, Notes, Math, Search, Zoom, etc.).
4. Author content and interactions: horizontal/vertical slides, fragments, backgrounds, transitions, media.
5. Configure runtime behavior: navigation, keyboard, `hash`, `slideNumber`, `autoSlide`, `view: 'scroll'`.
6. Validate and ship: local preview, Speaker View, print/PDF export, and optional API control code.

## Implementation Templates

### 1) Static HTML (UMD)

```html
<html>
  <head>
    <link rel="stylesheet" href="dist/reveal.css" />
    <link rel="stylesheet" href="dist/theme/black.css" />
    <link rel="stylesheet" href="plugin/highlight/monokai.css" />
  </head>
  <body>
    <div class="reveal">
      <div class="slides">
        <section>Slide 1</section>
        <section>
          <section>Vertical 1</section>
          <section>Vertical 2</section>
        </section>
      </div>
    </div>

    <script src="dist/reveal.js"></script>
    <script src="plugin/markdown/markdown.js"></script>
    <script src="plugin/highlight/highlight.js"></script>
    <script src="plugin/notes/notes.js"></script>
    <script>
      Reveal.initialize({
        hash: true,
        slideNumber: 'c/t',
        plugins: [RevealMarkdown, RevealHighlight, RevealNotes],
      });
    </script>
  </body>
</html>
```

### 2) npm/ESM

```js
import Reveal from 'reveal.js';
import Markdown from 'reveal.js/plugin/markdown/markdown.esm.js';
import Highlight from 'reveal.js/plugin/highlight/highlight.esm.js';
import Notes from 'reveal.js/plugin/notes/notes.esm.js';
import 'reveal.js/dist/reveal.css';
import 'reveal.js/dist/theme/black.css';

const deck = new Reveal({
  hash: true,
  slideNumber: 'c/t',
  plugins: [Markdown, Highlight, Notes],
});

await deck.initialize();
```

### 3) Markdown Slide Block

```html
<section data-markdown>
  <textarea data-template>
    ## Slide 1
    ---
    ## Slide 2
  </textarea>
</section>
```

## Feature Playbook

- Markdown: Enable the Markdown plugin; external markdown requires a local web server.
- Code: Enable Highlight and use `data-line-numbers` plus step highlights via `|`.
- Math: Use `RevealMath.KaTeX` by default (or MathJax when required).
- Speaker Notes: Enable Notes and press `S` for presenter view.
- Auto-Animate: Use adjacent `<section data-auto-animate>` and `data-id` for precise matching.
- Backgrounds: Use `data-background-*` (color/image/video/iframe).
- Scroll View: Use `view: 'scroll'` with optional `scrollSnap` and `scrollLayout`.
- PDF: Use `?print-pdf` then browser print to PDF.

## Reveal.js Tips

The items below should be preferred in this skill.

### 30-Second Design Review Checklist

Run this before considering a deck "done":

- Aesthetic direction is explicit and consistent (not mixed styles).
- Typography has clear hierarchy (display vs body) and readable line lengths.
- Palette has one dominant tone and one intentional accent color.
- At least one memorable visual motif exists (layout move, background treatment, or motion sequence).
- Motion supports narrative pacing (not scattered decorative effects).
- Slides stay readable on both desktop and mobile widths.

### 1) Title slide with custom background

Use an explicit first `<section>` as a title slide and style it with reveal data attributes.

```html
<section
  id="title-slide"
  data-background-image="./img/background.jpg"
  data-background-size="cover"
  data-background-opacity="0.9"
>
  <h1>My Slide Show</h1>
</section>
```

### 2) Move/resize a logo after leaving title slide

Use CSS classes plus the `slidechanged` event.
Add a persistent logo element in your HTML, e.g. `<img class="slide-logo" src="./images/my-logo.svg" alt="Logo" />`.

```css
.reveal .slide-logo {
  position: fixed;
  display: block;
  max-width: none !important;
}

.reveal .slide-logo-bottom-right {
  right: 12px !important;
  bottom: 0 !important;
  left: auto !important;
  max-height: 2.2rem !important;
}

.slide-logo-max-size {
  top: 5px;
  left: 12px;
  right: auto !important;
  bottom: auto !important;
  height: 100px !important;
  max-height: none !important;
}
```

```js
function syncLogoForSlide(currentSlide) {
  const logos = document.querySelectorAll('.slide-logo');
  const onTitle = currentSlide && currentSlide.id === 'title-slide';
  logos.forEach((el) => {
    el.classList.toggle('slide-logo-max-size', onTitle);
    el.classList.toggle('slide-logo-bottom-right', !onTitle);
  });
}

Reveal.on('ready', (event) => syncLogoForSlide(event.currentSlide));
Reveal.on('slidechanged', (event) => syncLogoForSlide(event.currentSlide));
```

### 3) Background image sizing (`cover` vs `contain`)

`cover` fills the slide and may crop. `contain` preserves the entire image.

```html
<section data-background-image="images/2024.jpg" data-background-size="cover"></section>
<section data-background-image="images/2024.jpg" data-background-size="contain"></section>
```

### 4) Slide structure control (replacement for Quarto `slide-level`)

Pure reveal.js does not use Pandoc `slide-level`. Control structure explicitly:

- HTML mode: one `<section>` per slide, nested `<section>` for vertical slides.
- Markdown mode: use configured separators (`---`, `--`) to split slides.

### 5) Emoji support

Quarto's `from: markdown+emoji` is not a reveal.js feature. In reveal.js:

- Use native Unicode emoji directly.
- Or render emoji through your markdown/HTML pipeline before reveal initializes.

### 6) Fit large text and stretch media

`r-fit-text` and `r-stretch` are reveal classes and work directly.

```html
<section>
  <div class="r-fit-text">Big Text</div>
</section>

<section>
  <p>Here is an image:</p>
  <img class="r-stretch" src="image.webp" alt="Demo image" />
  <p>Some text after the image.</p>
</section>
```

### 7) Reveal content on key press with fragments

```html
<section>
  <h2>It's a candy dog</h2>
  <p style="font-size: 2em; color: #75aadb;">Would you like to see a candy dog?</p>
  <img class="fragment fade-up" src="./images/dog.webp" alt="Candy dog" />
</section>
```

### 8) Two-column and 4-quadrant layouts

Quarto `::: columns` is not native reveal syntax. Use HTML/CSS layout wrappers.

```html
<section>
  <div class="cols">
    <div class="col col-70"><img src="./images/image_1.webp" alt="Left" /></div>
    <div class="col col-30">
      <img src="./images/image_2.webp" alt="Right top" />
      <img src="./images/image_3.webp" alt="Right bottom" />
    </div>
  </div>
</section>
```

```css
.reveal .cols {
  display: flex;
  gap: 1.5rem;
  align-items: flex-start;
}

.reveal .col-70 { flex: 0 0 70%; }
.reveal .col-30 { flex: 0 0 30%; }
```

For 4 quadrants, use a 2x2 CSS grid and reveal each cell with fragment classes (`fade-in-then-semi-out` or similar).

### 9) Custom inline short-code transform (`==text==` -> `<mark>text</mark>`)

```js
function convertMarkedTextInSlide(slide) {
  if (!slide) return;
  slide.innerHTML = slide.innerHTML.replace(/==([^=]+)==/g, '<mark>$1</mark>');
}

Reveal.on('ready', (event) => convertMarkedTextInSlide(event.currentSlide));
Reveal.on('slidechanged', (event) => convertMarkedTextInSlide(event.currentSlide));
```

### 10) Inline style and custom CSS

In reveal markdown, use inline HTML for precise text styling:

```html
<p>
  Make this <span style="color: red;">red</span> and this
  <span style="background: yellow;">highlighted</span>.
</p>
```

Load custom CSS in static HTML:

```html
<link rel="stylesheet" href="./assets/custom.css" />
```

Or import it in ESM:

```js
import './assets/custom.css';
```

### 11) Hide captions and style callouts when your theme emits them

Reveal core does not auto-generate Quarto callouts/captions, but many pipelines output similar markup:

```css
.reveal p.caption,
.reveal figcaption {
  display: none;
}

.reveal .callout-title {
  display: none;
}
```

### 12) Vertical flow, slide IDs, menu labels, numbering, and notes

- Vertical chapters: nest sections (`<section><section>Child</section></section>`).
- Navigation behavior: use `navigationMode: 'default' | 'linear' | 'grid'`.
- Stable slide URL names: set section `id` and enable `hash: true`.
- Menu labels (when using the menu plugin): set `data-menu-title`.
- Slide number with total: `slideNumber: 'c/t'`.
- Speaker notes: `<aside class="notes">...</aside>` and press `S`.
- Overview mode shortcut: `Esc`.

```html
<section id="intro" data-menu-title="Introduction">
  <h2>Intro</h2>
  <aside class="notes">Presenter-only notes</aside>
</section>
```

```js
Reveal.initialize({
  hash: true,
  slideNumber: 'c/t',
  navigationMode: 'default',
  plugins: [RevealNotes],
});
```

### 13) Choose one bold aesthetic direction before styling

Do not start from random colors and utility classes. Define a single visual direction (for example: editorial, brutalist, retro-futuristic, luxury minimal, playful) and keep every design decision aligned with it.

Practical rule:

- One deck, one dominant visual idea.
- Keep typography, palette, motion, and background treatment coherent with that idea.

### 14) Use deck-level design tokens (CSS variables)

Create a token layer first, then style components/slides with those tokens.

```css
:root {
  --deck-bg: #f6f3ea;
  --deck-surface: rgba(255, 255, 255, 0.72);
  --deck-text: #1b1a18;
  --deck-accent: #c0392b;
  --deck-muted: #6f6a61;
  --deck-shadow: 0 20px 50px rgba(20, 16, 10, 0.18);
  --deck-radius: 18px;
  --deck-gap: 1.2rem;
}

.reveal {
  color: var(--deck-text);
  background:
    radial-gradient(circle at 12% 18%, rgba(192, 57, 43, 0.12), transparent 45%),
    radial-gradient(circle at 88% 82%, rgba(27, 26, 24, 0.1), transparent 42%),
    var(--deck-bg);
}
```

### 15) Build typography hierarchy intentionally

Use a distinctive display face for headings and a separate body face for text-heavy slides. Define consistent scale and line length.

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display:opsz@9..40&family=Manrope:wght@400;500;700;800&display=swap');

.reveal {
  font-family: 'Manrope', 'Segoe UI', sans-serif;
}

.reveal h1,
.reveal h2,
.reveal h3 {
  font-family: 'DM Serif Display', Georgia, serif;
  letter-spacing: 0.01em;
  line-height: 1.05;
}

.reveal p,
.reveal li {
  line-height: 1.45;
  max-width: 58ch;
}
```

### 16) Prefer dominant palette + sharp accent over evenly mixed colors

A strong deck usually has:

- One dominant background family.
- One primary text color.
- One high-contrast accent for emphasis, links, and key annotations.

```css
.reveal a,
.reveal strong,
.reveal mark {
  color: var(--deck-accent);
}

.reveal mark {
  background: transparent;
  border-bottom: 0.2em solid color-mix(in srgb, var(--deck-accent) 35%, transparent);
  padding: 0 0.08em;
}
```

### 17) Compose slides with atmosphere (not flat backgrounds)

Avoid plain single-color canvases by default. Add depth with subtle gradients, textures, overlays, or layered shadows that match the chosen aesthetic.

```css
.reveal .panel {
  background: var(--deck-surface);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: var(--deck-radius);
  box-shadow: var(--deck-shadow);
  backdrop-filter: blur(6px);
  padding: calc(var(--deck-gap) * 1.2);
}
```

### 18) Use motion as choreography, not decoration

In reveal.js, prioritize:

- One meaningful entry sequence per slide (`fade-up`, `fade-in`, `zoom-in` fragments with stagger).
- Smooth global transition settings.
- Minimal micro-animations elsewhere.

```js
Reveal.initialize({
  transition: 'slide',
  backgroundTransition: 'fade',
  autoAnimateEasing: 'ease-out',
  autoAnimateDuration: 0.6,
});
```

```html
<section>
  <h2 class="fragment fade-in">Problem</h2>
  <p class="fragment fade-up">Constraint</p>
  <p class="fragment fade-up">Decision</p>
  <p class="fragment fade-up">Outcome</p>
</section>
```

### 19) Break predictable layouts with controlled asymmetry

Avoid centering everything. Mix alignment, width, overlap, and whitespace intentionally.

```css
.reveal .asym {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 2rem;
  align-items: end;
}

.reveal .asym .media {
  transform: translateY(1rem);
}
```

### 20) Final anti-generic design pass

Before shipping, quickly verify:

- Font choice is intentional and not default/generic.
- Color system has a dominant tone plus a clear accent.
- At least one memorable visual motif exists (background treatment, type lockup, layout move, or motion sequence).
- Slide density is controlled; no wall-of-text pages.
- Mobile viewport still preserves hierarchy and readability.

## Guardrails

- Do not invent reveal.js config keys; only use documented options.
- Do not use plugin syntax before registering the corresponding plugin (Markdown/Math/Notes/Highlight, etc.).
- For multi-instance pages, prefer `new Reveal(rootEl, config)` with `embedded: true`.
- In React, avoid duplicate initialization and call `destroy()` on unmount.
- Treat Quarto/Pandoc-only syntax as source material, not runtime reveal.js syntax (`::: columns`, `::: notes`, `slide-level`, `markdown+emoji`, callout blocks).
- When migrating from Quarto to reveal.js, preserve behavior first (fragment timing, navigation flow, note visibility), then adjust visual styling.
- Do not ship visually generic decks. Each deck must have a deliberate aesthetic direction, typography system, palette strategy, and motion intent.

## Delivery Checklist

- First slide renders correctly with no obvious console errors.
- Plugin registration matches the content syntax used (Markdown/Notes/Math/Highlight/etc.).
- Navigation and interactions match requirements (keyboard, `hash`, `slideNumber`, `autoSlide`, touch).
- If export is requested, verify print/PDF workflow end-to-end.

## References

### Getting Started

| Topic        | Description                                                                               | Reference                                    |
| ------------ | ----------------------------------------------------------------------------------------- | -------------------------------------------- |
| Home         | Entry page for reveal.js docs, covering core concepts and navigation to all major guides. | [Home](./references/index.md)                |
| Course       | A structured tutorial path that teaches reveal.js basics step by step.                    | [Course](./references/course.md)             |
| Installation | Explains how to install reveal.js in different environments and project setups.           | [Installation](./references/installation.md) |

### Content

| Topic            | Description                                                                    | Reference                                            |
| ---------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------- |
| Markup           | Shows the core HTML slide structure and how nested sections define navigation. | [Markup](./references/markup.md)                     |
| Markdown         | Describes writing slides in Markdown and configuring the Markdown plugin.      | [Markdown](./references/markdown.md)                 |
| Backgrounds      | Documents slide background options such as color, image, video, and iframe.    | [Backgrounds](./references/backgrounds.md)           |
| Media            | Covers embedding and controlling media content inside slides.                  | [Media](./references/media.md)                       |
| Lightbox         | Explains how to present media in an enlarged lightbox-style experience.        | [Lightbox](./references/lightbox.md)                 |
| Code             | Introduces code block rendering and syntax highlighting patterns for slides.   | [Code](./references/code.md)                         |
| Math             | Shows how to render mathematical formulas with KaTeX or MathJax integrations.  | [Math](./references/math.md)                         |
| Fragments        | Describes progressive reveal effects to show slide elements in sequence.       | [Fragments](./references/fragments.md)               |
| Links            | Explains internal and external linking behavior in reveal.js presentations.    | [Links](./references/links.md)                       |
| Layout           | Covers layout techniques to organize content cleanly within each slide.        | [Layout](./references/layout.md)                     |
| Slide Visibility | Describes controls for showing, hiding, or excluding specific slides.          | [Slide Visibility](./references/slide-visibility.md) |

### Customization

| Topic             | Description                                                                    | Reference                                              |
| ----------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------ |
| Themes            | Explains built-in themes and how to style presentations with custom theming.   | [Themes](./references/themes.md)                       |
| Transitions       | Documents slide and element transition effects and their configuration.        | [Transitions](./references/transitions.md)             |
| Config Options    | Full reference for initialization and runtime configuration settings.          | [Config Options](./references/config.md)               |
| Presentation Size | Shows how to control deck dimensions, scaling behavior, and responsive sizing. | [Presentation Size](./references/presentation-size.md) |

### Features

| Topic            | Description                                                                   | Reference                                            |
| ---------------- | ----------------------------------------------------------------------------- | ---------------------------------------------------- |
| Vertical Slides  | Explains vertical stacks for subtopics under a horizontal slide flow.         | [Vertical Slides](./references/vertical-slides.md)   |
| Auto Animate     | Describes automatic tweening between similar elements across adjacent slides. | [Auto Animate](./references/auto-animate.md)         |
| Auto Slide       | Covers timed autoplay behavior and controls for automatic slide progression.  | [Auto Slide](./references/auto-slide.md)             |
| Speaker View     | Shows presenter tools, notes view, and speaker-screen workflow.               | [Speaker View](./references/speaker-view.md)         |
| Scroll View      | Explains continuous scroll-based presentation mode and related settings.      | [Scroll View](./references/scroll-view.md)           |
| Slide Numbers    | Documents numbering formats and display options for slide counters.           | [Slide Numbers](./references/slide-numbers.md)       |
| Jump to Slide    | Shows how to quickly navigate to a specific slide during presentation.        | [Jump to Slide](./references/jump-to-slide.md)       |
| Touch Navigation | Describes gesture and touch interactions on mobile and touch devices.         | [Touch Navigation](./references/touch-navigation.md) |
| PDF Export       | Explains the print-to-PDF workflow and export-specific adjustments.           | [PDF Export](./references/pdf-export.md)             |
| Overview Mode    | Documents the zoomed overview interface for fast deck navigation.             | [Overview Mode](./references/overview.md)            |
| Fullscreen Mode  | Covers fullscreen presentation behavior and toggling options.                 | [Fullscreen Mode](./references/fullscreen.md)        |

### API

| Topic              | Description                                                                  | Reference                                                |
| ------------------ | ---------------------------------------------------------------------------- | -------------------------------------------------------- |
| Initialization     | Explains deck bootstrapping, config injection, and startup lifecycle.        | [Initialization](./references/initialization.md)         |
| API Methods        | Reference for imperative methods used to control reveal.js programmatically. | [API Methods](./references/api.md)                       |
| Events             | Lists emitted events and shows how to listen for deck state changes.         | [Events](./references/events.md)                         |
| Keyboard           | Documents keyboard shortcuts and custom key-binding configuration.           | [Keyboard](./references/keyboard.md)                     |
| Presentation State | Explains capturing, restoring, and sharing presentation state data.          | [Presentation State](./references/presentation-state.md) |
| PostMessage        | Describes cross-window communication using the postMessage interface.        | [PostMessage](./references/postmessage.md)               |

### Initialization

| Topic            | Description                                                              | Reference                                            |
| ---------------- | ------------------------------------------------------------------------ | ---------------------------------------------------- |
| Using Plugins    | Shows how to register and configure official or third-party plugins.     | [Using Plugins](./references/plugins.md)             |
| Creating Plugins | Guide to authoring custom reveal.js plugins and integrating them safely. | [Creating Plugins](./references/creating-plugins.md) |
| Multiplex        | Explains synchronized multi-client presentations for remote audiences.   | [Multiplex](./references/multiplex.md)               |

### Plugins

| Topic         | Description                                                                                    | Reference                                                  |
| ------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Animate       | Adds SVG.js-based slide animations that can be coordinated with reveal.js flow.                | [Animate](./references/plugin-animate.md)                  |
| Chart         | Integrates Chart.js so presentations can render configurable data charts.                      | [Chart](./references/plugin-chart.md)                      |
| Fullscreen    | Provides fullscreen layout behavior to maximize available slide space.                         | [Fullscreen](./references/plugin-fullscreen.md)            |
| D3 (reveald3) | Embeds JavaScript visualizations (including D3-based content) with fragment-aware transitions. | [Reveal.js-d3 (reveald3)](./references/plugin-d3.md)       |
| Mermaid       | Adds Mermaid diagram rendering support directly inside reveal.js slides.                       | [reveal.js-mermaid-plugin](./references/plugin-mermaid.md) |

### Other

| Topic                  | Description                                                                     | Reference                                                                      |
| ---------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| React Framework        | Covers integration patterns and usage details for reveal.js in React apps.      | [React Framework](./references/react.md)                                       |
| Upgrading Instructions | Migration notes for updating reveal.js versions without breaking decks.         | [Upgrading Instructions](./references/upgrading.md)                            |
