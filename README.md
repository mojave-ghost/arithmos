# The Finite Sequence
 
**A narrative-driven, interactive site that teaches algorithmic thinking through story.**
 
> "Efficiency is a form of love."
 
## What This Is
 
The Finite Sequence reimagines algorithm complexity as a character study. Arithmos — a gladiator built from computer science principles — fights not with chaos but with reasoned, step-by-step precision. The site uses that narrative as a hook to explain real concepts: time complexity, Big-O growth, and how "simple" outcomes hide layered computation underneath.
 
It's built as a single-page, client-rendered experience: no backend, no build step, just semantic HTML, CSS, and vanilla JavaScript doing the work.
 
## Educational Design Goals
 
This project was built to demonstrate the same skills that matter in any learner-facing web platform:
 
- **Responsive, accessible front-end architecture** — clean HTML structure, readable typography scaling, and layouts that hold up across screen sizes
- **Concept visualization** — an interactive "Time Dilation" module renders abstract complexity growth (O(1) → O(2^n)) as a tangible, animated clock and data readout, so learners *see* the idea instead of just reading a definition
- **Interactive narrative as a teaching device** — blog-style "writings" unpack the same idea (efficiency, cost, trade-offs) from multiple angles, reinforcing a single mental model through repetition and story rather than dry restatement
- **State-driven UI without a framework** — page navigation, live data binding, and animation timing are all handled in plain JS, showing comfort with DOM manipulation and event-driven design at the foundational level
## Tech Stack
 
- HTML5 / CSS3 (custom properties, clip-path, keyframe animation)
- Vanilla JavaScript (Canvas API for the background particle system, DOM-driven interactivity for the complexity visualizer)
- Google Fonts (Cinzel, Playfair Display, Crimson Pro, Courier Prime)
## Why It's Structured This Way
 
The goal wasn't just "make a cool page" — it was to build the kind of self-contained, front-end-only educational tool that has to work reliably across devices and explain something complex clearly, without relying on a backend or LMS integration to do the heavy lifting. That constraint is intentional: it mirrors the kind of standalone interactive component a front-end developer would be asked to build and then plug into a larger learning platform.
 
## Status
 
Actively maintained as a portfolio piece demonstrating front-end architecture, accessibility-minded markup, and interactive concept visualization for educational content.
