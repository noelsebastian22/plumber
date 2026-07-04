# PLUMBER. — Website Build Plan

**Stack:** Astro 5 (static output) · Tailwind CSS v4 · React (islands only where interactivity is required) · shadcn/ui (only if a component genuinely benefits) · TypeScript

**Goals:** Pixel-faithful to the provided designs · Lighthouse 95+ across the board · Semantic, SEO-optimized HTML · Minimal JS shipped (Astro islands, zero-JS by default)

**Design references (in repo root):**
| File | Covers |
|---|---|
| `Full Design.jpeg` | Entire page, section order, spacing rhythm |
| `Hero.jpeg` | Nav + hero + feature cards (video placement) |
| `About us.png` | About/company, services intro, solutions sections |
| `Services.png` | Services cards (Commercial / Residential / Emergency) |
| `contact.png` | Dark appointment form section + testimonials |
| `footer.png` | Footer + newsletter + copyright bar |
| `A_tradesman_in_a_blue_plaid_sh.mp4` | Hero video (needs compression + poster) |

---

## Design System (extracted from mockups)

- **Colors:**
  - `ink` near-black `#0A0A0A` (headings, dark bands, footer bar)
  - `accent` sky blue `≈ #2B9FE3` (CTAs, icons, links, stars)
  - `surface` white `#FFFFFF`, `surface-alt` light blue-gray `≈ #EEF3F6`
  - `muted` gray body text `≈ #5A6570`
- **Typography:** Modern grotesque sans (e.g., **Bricolage Grotesque** or **Plus Jakarta Sans** for headings — hero mixes regular + bold weights within one line; body: same family or Inter). Self-hosted via `@fontsource`, `font-display: swap`, preload the heading weight.
- **Shape language:** Pill buttons (`rounded-full`), cards `rounded-xl`+, generous white space, thin borders on light cards.
- **Buttons:** Primary = blue pill; secondary = black pill ("Book Now"); tertiary = dark pill ("About More", "Our Services").

---

## Phase 0 — Project Scaffold ✅ checkpoint: `npm run dev` renders a styled page

1. `npm create astro@latest` (empty template, TS strict) in this directory.
2. Add integrations: `@astrojs/react`, `@astrojs/sitemap`, Tailwind v4 (`@tailwindcss/vite`).
3. `src/styles/global.css` — Tailwind import + `@theme` design tokens (colors, fonts, radii above).
4. Base layout `src/layouts/BaseLayout.astro`: HTML shell, meta/OG/Twitter tags, canonical URL, favicon, font preloads.
5. Folder structure:
   ```
   src/
     components/   (Header, Hero, Features, About, Services, Solution,
                    Stats, Faq, Projects, Appointment, Testimonials, Footer)
     layouts/
     pages/
     styles/
     data/         (services, faqs, projects, testimonials as ts/json)
   public/
     images/  videos/  favicon
   ```

## Phase 1 — Asset Pipeline ✅ checkpoint: optimized assets in `public/` & `src/assets/`

1. **Video:** `ffmpeg` re-encode `A_tradesman_in_a_blue_plaid_sh.mp4` → H.264 CRF 28, 1280w, `-movflags +faststart`, strip audio (~1MB) into `public/videos/hero.mp4`; extract poster frame → `hero-poster.webp`.
2. **Images:** Source royalty-free plumber/stock imagery matching mockup compositions (or placeholders awaiting client images). All through `astro:assets` `<Image>` / `<Picture>` → AVIF/WebP, explicit dimensions (no CLS), lazy-load below the fold, `fetchpriority="high"` only on hero media.
3. Favicon + OG image (1200×630) in brand style.

## Phase 2 — Header + Hero + Feature Cards (`Hero.jpeg`) ✅ checkpoint: matches mockup at 1440/768/375px

1. **Header:** wordmark left, center nav (Home, About Us, Services, Blogs, Contact), black "Book Now" pill. Mobile: hamburger → slide-down panel (tiny vanilla JS or React island).
2. **Hero:** two-column top (H1 with mixed weights | avatar group + "18k+ Satisfied Customers", paragraph, "Contact Us" blue pill), full-width media panel overlapping a black band below; video with circular play overlay — poster shown, video lazy-loaded (`preload="none"`, autoplay-muted-loop on visibility OR click-to-play, decide in implementation).
3. **Feature cards:** 3 white cards (Trusted Expertise / 24-7 Availability / Upfront Pricing) with blue circular icons, sitting on the black band.

## Phase 3 — About + Services (`About us.png`, `Services.png`)

1. **About:** image collage left with "25 Years Experience" vertical badge, right heading "Our Company Provide The Best Plumbing Service" + copy + "About More" dark pill.
2. **Services:** heading row "From Leaking Faucet To Gushing Pipes" + right-aligned copy; 3 cards — Commercial (image, blue tag), Residential (dark card with checklist: Plumbing Repairs & Installation, Drain & Sewer Cleaning, Water Heaters, Water Line Installation, Sump & Sewage Pumps), Emergency (image, blue tag); "View All Services" CTA.

## Phase 4 — Solution + Solutions/Perfect + Stats + FAQ

1. **Ultimate Solution band** (light blue bg): kitchen image left with floating quote card (Braydon Butler), right heading + copy + "Our Services" pill.
2. **Perfect Solutions:** heading + copy left with two icon rows (Affordable Price — dark, Expert Plumber — light), plumber image right.
3. **Stats strip:** 1.25k Successful Projects · 1.24k Satisfied Customers · 250+ Expert Plumbers · 100% Quality Products. Optional count-up on scroll (IntersectionObserver, no library).
4. **FAQ:** left heading + copy + "Ask Questions" pill; right accordion (4 items). Use native `<details>`/`<summary>` styled with Tailwind — zero JS — unless animation demands a React/shadcn Accordion island.

## Phase 5 — Projects + Appointment + Testimonials (`contact.png`)

1. **Latest Projects:** centered heading + 3-card grid, middle card with dark overlay caption ("Leak Detection Project").
2. **Appointment section:** dark rounded container — heading "Saving Your Money With Efficient Plumbing Technologies", image left, white form card right (Full Name, Email, Telephone, Date, Select Services dropdown, Message, "Make Appointment" blue pill). React island with client-side validation. **Form backend TBD (see open questions).**
3. **Testimonials:** "What Our Customer Says?" + 2-up cards (avatar, name, role, copy, 5 blue stars) with prev/next arrows — small React island or CSS scroll-snap.

## Phase 6 — Footer (`footer.png`)

Logo + blurb + phone/email/address with icons · Quick Links · Useful Links · Newsletter (email input + black "Send" button) · social icons (Facebook blue-filled, Twitter/X, LinkedIn, Instagram outlined) · black copyright bar "Copyright © 2024 Plumber. All rights reserved" (year auto-generated).

## Phase 7 — SEO, Accessibility & Performance Hardening

1. Semantic landmarks (`header/main/section/footer`), single H1, logical heading order.
2. Meta title/description, canonical, OG/Twitter cards, `robots.txt`, sitemap.
3. **JSON-LD:** `LocalBusiness`/`Plumber` schema (name, phone, address, hours), `FAQPage` schema from FAQ data.
4. A11y pass: focus states, aria labels on icon buttons/carousel, contrast check, reduced-motion guard for video autoplay & count-ups.
5. Perf pass: Lighthouse ≥95, zero CLS, JS budget check (islands only: mobile nav, form, carousel), preconnect/preload audit.

## Phase 8 — Final QA & Build

1. Cross-viewport review vs mockups (375 / 768 / 1024 / 1440).
2. `astro build` + preview; validate HTML, test form validation states, broken-link check.
3. Git commit history per phase; deployment config (target TBD).

---

## Decisions (confirmed 2026-07-04)

1. **Structure:** Single-page landing; nav items scroll to anchor sections (`#about`, `#services`, `#projects`, `#contact`); Blogs is a placeholder link.
2. **Forms:** Stubbed — client-side validation + success state only; real backend wired later. Site stays fully static.
3. **Content:** Real, SEO-optimized plumbing copy (headings kept from mockups). Business details remain placeholder (PLUMBER., `(001) 123 456 789`, `5241 Elgin st. Celina, 10258`) until real ones are provided.
4. **Deployment:** Vercel, static output (no adapter needed); placeholder site URL in `astro.config.mjs` until domain is known.
