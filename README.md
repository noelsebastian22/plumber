# PLUMBER. — Plumbing & Handyman Services Landing Page

Single-page marketing site for a New York-based plumbing and handyman company. Built to convert renovation-planning homeowners into booked appointments — not just landing and bouncing.

**Stack:** Astro 7 + Tailwind CSS v4 + TypeScript. Fully static output.

## Quick start

```sh
npm install
npm run dev        # starts at localhost:4321
npm run build      # static build to dist/
npm run preview    # preview the production build locally
```

## Sections

All sections live in `src/components/` and compose onto a single page in `src/pages/index.astro`:

| Component       | ID anchor    | Description                                            |
| :-------------- | :----------- | :----------------------------------------------------- |
| Hero            | `#home`      | Headline, customer count, hero video, contact CTA      |
| FeatureCards    | —            | Three selling-point cards (expertise, 24/7, pricing)   |
| About           | `#about`     | Company story, photo collage, 25-year experience badge |
| Services        | `#services`  | Three image tiles with slide-up hover overlays         |
| Solution        | —            | Split layout — image + testimonial, copy + CTA        |
| PerfectSolutions | —           | Two dark feature cards + photo collage                |
| Stats           | —            | Animated count-up stats band (IntersectionObserver)    |
| FAQ             | `#faq`       | Accordion FAQ + contact CTA                            |
| Projects        | `#projects`  | Three project cards with tags                          |
| Appointment     | `#contact`   | Dark section with booking form card                    |
| Testimonials    | —            | Three review cards with star ratings                   |
| Header          | —            | Sticky nav bar with mobile hamburger menu              |
| Footer          | —            | Four-column footer with newsletter signup              |

## Project structure

```
├── public/
│   ├── images/
│   │   ├── unsplash/          # Photo assets
│   │   └── placeholders/      # SVG fallbacks
│   └── videos/
│       └── hero.mp4           # Hero background video
├── src/
│   ├── components/            # 13 Astro components (one per section)
│   ├── layouts/
│   │   └── BaseLayout.astro   # Shell with metadata, favicon, OG tags
│   ├── pages/
│   │   └── index.astro        # Single page composing all sections
│   └── styles/
│       └── global.css         # Tailwind v4 theme tokens + base styles
├── astro.config.mjs
└── package.json
```

## Design tokens

Custom properties defined in `src/styles/global.css` via Tailwind v4 `@theme`:

- **Colors:** `--color-ink`, `--color-accent`, `--color-surface`, `--color-surface-alt`, `--color-muted`, `--color-line`, `--color-star`
- **Typography:** Plus Jakarta Sans Variable
- **Easing:** `--ease-out-expo`, `--ease-out-quart`
- **Radius:** `--radius-4xl` (2rem)

All body text uses `--color-ink` or `--color-muted`. Accent blue (`#2B9FE3`) is reserved for interactive elements and large text — it passes AA on white only at large sizes, so `--color-accent-text` (`#1677B3`) is used for inline accent text.

## Technical notes

- **Build output:** Fully static — no SSR, no API routes, no client-side hydration beyond the count-up stats and mobile menu toggle.
- **Form:** Rendered with `novalidate` — no backend yet. Intended to wire up to Netlify Forms, Formspree, or similar once a deploy target is finalized.
- **Motion:** All animations respect `prefers-reduced-motion` via `motion-reduce:transition-none` utility classes.
- **React:** Included in dependencies but not currently used — ready if client-side interactivity is needed later.
- **Images:** Uses Unsplash photos with attribution; SVG placeholders exist as build-safe fallbacks.

## Content note

All branding and contact details are placeholder:

- **Name:** PLUMBER.
- **Phone:** (001) 123 456 789
- **Email:** hello@plumber.com
- **Address:** 123 Pipe Street, Brooklyn, NY 11201

Replace these with the client's real details before launch.
