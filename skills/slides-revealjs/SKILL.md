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

## Guardrails

- Do not invent reveal.js config keys; only use documented options.
- Do not use plugin syntax before registering the corresponding plugin (Markdown/Math/Notes/Highlight, etc.).
- For multi-instance pages, prefer `new Reveal(rootEl, config)` with `embedded: true`.
- In React, avoid duplicate initialization and call `destroy()` on unmount.

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
