# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Commands

### Serve the site locally

This repository is a static single-page site. There is no build step; you can open `index.html` directly in a browser, or serve it via a simple HTTP server.

- macOS: open the file directly in your default browser:
  - `open index.html`
- Cross-platform: serve the current directory over HTTP on port 8000:
  - `python -m http.server 8000`
  - Then navigate to `http://localhost:8000/` and load `index.html`.

### Linting and tests

As of 2026-01-22 there is no package manifest, lint configuration, or automated test setup in this repository. If you introduce a toolchain (e.g. `npm`, ESLint, or a test runner), add the relevant commands here for future agents.

## Code architecture

### Single-page structure

- All markup, styling, and behavior live in `index.html`.
- The document contains three logical "pages" wrapped in `.page` containers:
  - `#home-page` – hero section and short narrative introducing Arithmos.
  - `#writings-page` – grid of short essay excerpts with dates and categories.
  - `#about-page` – conceptual explanation sections, time-dilation visualizations, and algorithm metaphors.
- Navigation is handled client-side by toggling a `hidden` class on these containers.

### Navigation and routing

- The top navigation bar and logo use `data-page` attributes to indicate which `.page` section to show.
- `pages` is a simple map from logical page names (`home`, `about`, `writings`) to their corresponding DOM elements.
- `showPage(pageName)` hides all pages, then removes `hidden` from the selected one and updates the active nav link.
- The browser URL does not change; this is a minimal SPA-style router implemented purely with DOM manipulation.

### Visual design and layout

- All CSS is in an inline `<style>` block and uses CSS variables for the color system (e.g. `--void-black`, `--circuit-cyan`, `--neon-pink`).
- Layout is mostly flexbox and CSS grid:
  - Hero section is centered vertically using flex.
  - Writings list uses a responsive grid (`.posts-grid`) that collapses to a single column on narrow screens.
  - About sections are vertically stacked and animated in with a `fadeInUp` keyframe.
- Decorative framing (clip-path polygons, borders, corner accents) is used heavily to create a stylized, "algorithmic" aesthetic.

### Algorithmic background system (canvas)

- A full-screen `<canvas id="algorithmicBackground">` sits behind all content and is driven by a custom rendering loop.
- Key pieces:
  - Global state: `width`, `height`, `time`, `complexity`, `phaseTime`, `isIncreasing`.
  - A phase system controls `complexity` over time:
    - `PHASE_DURATION` defines how long each static→dynamic or dynamic→static phase lasts.
    - `updateComplexity()` advances `phaseTime`, toggles `isIncreasing` every phase, and remaps the 0–1 complexity value through an `easeInOutCubic` easing function for smooth transitions.
  - `CircuitNode` objects represent nodes in a circuit:
    - Initialized on a jittered grid across the canvas.
    - Maintain base positions plus time-varying drift based on `complexity`.
    - Track connections to nearby nodes for drawing edges.
  - `DataPacket` objects simulate packets traveling along some of the node connections when `complexity` is sufficiently high.
- Rendering pipeline in `render()` (original version):
  - Clear with a semi-opaque fill whose alpha depends on `complexity` (higher complexity → slightly less opaque → more motion trails).
  - Draw a radial gradient overlay whose color interpolates between deep purple and cyan as `complexity` increases.
  - For each node:
    - Update position and energy.
    - Draw connections to nearby nodes with alpha and glow scaled by distance and `complexity`.
    - At higher `complexity`, connections gain curvature via quadratic Bézier control points and a glow via `shadowBlur`.
  - Draw node glows scaled by energy and `complexity`.
  - Update and draw `DataPacket`s as bright moving points along connections at higher `complexity`.
  - Overlay scanlines and occasional glitch slices when `complexity` is above configured thresholds.
- The render loop is driven by `requestAnimationFrame(render)` and continuously adjusts visuals as `complexity` oscillates between static and dynamic states.

### Time dilation UI and coupling to complexity

- The "Time Dilation Field" in the About page is implemented using SVG plus DOM text elements:
  - The clock face is drawn via SVG `<circle>` and `<line>` elements with classes like `.clock-hand.second` and `.clock-hand.minute`.
  - Associated labels display execution time (fixed 21 seconds baseline) and the current phase label (`#dilationPhase`).
- `updateTimeDilation()` ties the visual state to the global `complexity` value from the canvas system:
  - Computes a `factor = 1 + complexity * 5`, interpreted as a time dilation multiplier.
  - Updates numeric displays:
    - `#dilationFactor` – current multiplier.
    - `#subjectiveTime` and `.clock-time` – perceived duration in seconds (`21 * factor`).
    - `#operationsVisible` – number of visible operations (approximately `2^(complexity * 10)`).
  - Adjusts animation behavior:
    - Slows the SVG second hand by increasing its `animationDuration` with the factor.
    - Rotates the minute hand proportionally to `complexity`.
    - Updates the `#dilationPhase` label text and color based on ranges of `complexity` (e.g. REAL-TIME, TIME STRETCHING, DEEP DILATION, FULL EXPANSION).
  - Calls `applyGlobalTimeDilation(factor)` to propagate the dilation concept to other animated elements by scaling their `animation-duration` styles where applicable.
- Integration with the canvas loop:
  - The original `render()` is captured as `originalRender`.
  - A wrapper `renderWithTimeDilation()` calls `originalRender()` and then `updateTimeDilation()`.
  - The global `render` identifier is reassigned to the wrapper so that the animation loop both updates the background and refreshes the time dilation UI in sync.

### Content layout and semantics

- Home page: hero title and tagline plus a short narrative in `.story` paragraphs styled with a drop-cap and highlighted spans.
- Writings page: a series of `.post-card` elements representing dated entries with titles, short excerpts, and simple category tags.
- About page:
  - Several `.about-section` blocks explaining the metaphor of Arithmos as an algorithm, problem space, efficiency-as-ethics, and the "finite sequence" concept.
  - A "Time Perception Matrix" section (`.time-perception-box`) that reiterates the dilation metaphor via three numeric tiles and a narrative paragraph tying perception, complexity, and algorithmic steps together.

Use this document as the starting point for any future structural refactors (e.g. extracting CSS/JS into separate files or adding a build/test toolchain) and update the Commands and architecture sections as the project evolves.