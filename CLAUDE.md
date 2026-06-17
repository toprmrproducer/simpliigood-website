# SimpliiGood Spirulina — Website (client: Alon Yardeni)

Marketing site for **SimpliiGood** ("Simplii Green" division) — fresh-frozen spirulina cubes.
Single-page, scroll-based, brand-led. Launch target: week of 15 Jun 2026.

## Stack
- Static HTML + inline CSS + vanilla JS + **GSAP/ScrollTrigger via CDN**. No framework on the page.
- Built with **Vite** (multi-page config, but only `index.html` is an input now).
- Deployed on **Vercel** from GitHub (`ShreyasRajthe1st/Alon-s-website`), config in `vercel.json`
  (`buildCommand: npm run build`, `outputDirectory: dist`). Vercel serves `dist/`.

## Structure
- `index.html` — THE site. Self-contained: brand tokens + all sections + scripts inline.
- `public/images/` — chosen, clean-named product/lifestyle photography (copied as-is into `dist/images/`).
- `public/fonts/` — Franklin Gothic URW `.otf` (the real brand typeface), `@font-face` in index.html.
- `Fonts/` — original brand font drop from the client (source of the public/fonts copies).
- `_index.linoxa.bak.html` — the ORIGINAL Webflow "Linoxa" real-estate template build (residue-ridden).
  Kept only as reference. Do NOT deploy or copy from it.
- `about/service/technology/pricing.html` + loose root images — legacy Linoxa pages, NOT built/deployed.

## Brand system (from the client's SimpliiGood Brand Guidelines — non-negotiable)
- **Simplii Yellow `#FEE62D`** = PRIMARY. **Spirulina Green `#154048`** = lead dark.
  **Starburst Green `#31B278`** = secondary. Fruit colors (raspberry `#EF467B`, blueberry, strawberry,
  lime, cherry) = ACCENTS only (CTAs/illustration), never section fills.
- Type: **Franklin Gothic URW** — Heavy for display, Demi for body, Condensed for labels/marquee.
- Voice: hopeful, honest, easy. Tagline **REAL. SUPER. FOOD.** "Sunshine in a cube." Brand name is
  always titlecase **SimpliiGood**.
- Hard rules: yellow + dark green must lead. No black template footers. No "design/spaces" copy.
  No third-party build watermark. Keep packaging shots a single consistent scale (no mismatched composites).

## Page sections (in order)
nav → hero (yellow, image) → marquee → benefits/stats (yellow) → fresh-vs-dried (green) →
"Drop it. Done." use cases (green) → **"See it in action" video band (cream, 2 UGC videos)** →
shop/D2C (cream, **product-spin video**) → sustainability (starburst) → **gallery (green, 8 imgs)** →
testimonials (cream) → B2B/food-service form (green) → store locator (yellow) → footer (green).

## Media (all from Sujal's original repo, optimized; deploy is Netlify only)
- `public/videos/` — `product-spin.mp4` (hero-video.mp4, shop section, CSS-zoomed via `.shop-media.has-video`),
  `in-action.mp4` + `in-action-2.mp4` (UGC Ultra/UgC, the video band). All re-encoded with ffmpeg:
  scaled, audio stripped, `+faststart`; `<video autoplay muted loop playsinline poster=…>`.
- All photos optimized with Pillow to ≤1600px (webp q82 / png). Total deployed weight ~5MB.
- Source originals (5MB+ mp4s, 3-4MB pngs) stay in repo root; only optimized `public/` copies ship.
- If regenerating imagery: use **Google Nano Banana 2** via the funded GEMINI key (see 17 Jun PM update; Magnific MCP is out of credits). Pass `public/images/hero-pack.webp` as a reference so the pouch stays on-brand; keep packaging a single consistent scale.

## Conventions / gotchas
- Reveal animations are **progressive enhancement**: content is visible by default; `.js-on .reveal`
  hides then GSAP/IO adds `.in`. There is a load-time safety net so nothing is ever stuck at opacity 0.
- Counter numbers live in `.count` spans INSIDE `.stat b` / `.bigstat b`. Scope generic description
  rules to `.stat>span` / `.bigstat>span` so they don't shrink the counters.
- The footer watermark is `.footer-wm-wrap .wm` (it's a sibling of `.footer-bottom`, not a child).
- Image guard swaps any failed `<img>` to a brand-gradient SVG data-URI — never ships a broken image.
- B2B form has no backend yet — it prevents default and shows an inline confirmation. Wire to a real
  endpoint (or Formspree/Netlify/Shopify) before relying on leads.

## Dev / build / test
- `npm install && npm run dev` → http://localhost:8085
- `npm run build` → emits `dist/`. Test the **built** `dist/` (that's what deploys):
  `cd dist && python3 -m http.server 8091` then drive it with Playwright (desktop + mobile).
- Before declaring done: 0 console errors, 0 broken images, no `.reveal` stuck at opacity 0,
  and grep `dist/index.html` for residue: `shape your next space`, `Fascreek`, `1430 Broadway`,
  `Disccover`, `purposeful spaces` → all must be 0.

## Update 15 Jun 2026 (PM) — multi-page + scroll-scrubbing + cursor (client approved)
Alon approved the direction and asked for scroll-scrubbing animations. Now a 4-page site:
- `index.html` — Simplii Green home. Adds: Lenis smooth scroll + GSAP ScrollTrigger scrub,
  PINNED HORIZONTAL "Drop it" use-cases (desktop; stacks on mobile via gsap.matchMedia),
  scroll-progress bar, film grain, hero aura + bg word + parallax, custom cursor + magnetic
  buttons, a Simplii Texture teaser (#texture), and the new flatlay image in the in-action band.
- `texture.html` / `about.html` / `food-service.html` — built on shared `public/site.css` +
  `public/site.js` (design system + Lenis/reveals/cursor/forms). Vite builds all 4 (vite.config inputs).
- Footer: "Designed & built by RapidX AI" credit (`.footer-credit`) on every page.
- Nav (all pages): Why Spirulina · How to Use · Food Service · Simplii Texture · Find a Store · About · Shop.
- Images: Texture uses drive "Smoked salmon" photos (texture-*.webp). Sojil's flatlay = flatlay-tray.webp.
- GOTCHA / lesson: never drive a CSS marquee's animationDuration from a scroll onUpdate — it restarts
  the animation each frame and VIBRATES. Removed. Also: don't gate above-the-fold text visibility on a
  JS tween (a masked-line hero entrance left the headline hidden) — hero entrance is CSS-only now.
- Sub-page forms post nowhere yet (data-confirm graceful confirm). Wire to a real endpoint before launch.

## Update 15 Jun (late) — scroll/cursor/nav fixes + per-page enrichment
- SCROLL: removed the mix-blend-mode grain + the custom blend-mode cursor (both forced full-page repaints = "horrendous" scroll jank). Sub-pages now use NATIVE scroll (Lenis removed from public/site.js); home keeps Lenis. Grain is opacity-only now.
- CURSOR: reverted to the native cursor. Instead, CARDS react to the cursor (3D tilt + lift on hover) + magnetic buttons. Logic in public/site.js and index.html inline script (fine-pointer only).
- NAV: dark-hero pages (texture.html, food-service.html) get class "nav nav-light" so the top/non-scrolled nav is readable (yellow brand + cream links). Defined in site.css.
- PLAINNESS: texture/about/food-service enriched (per-page <style> blocks after the site.css link): colored accent cards, full-bleed image bands, numbered process steps, dark stat bands, full-bleed division cards, menu tiles. Built by 3 parallel sub-agents, one per page.
- Atlas Cloud image API (key for Nano Banana 2) rejected every image format (Gemini *-image 400s, gpt-image router down). $0 spent. **RESOLVED 17 Jun PM** — the real cause was the wrong model id in gemini.env (`gemini-3.1-flash-live-preview`, a live model). The working image path is the funded GEMINI key with model `gemini-3-pro-image-preview` (see 17 Jun PM update).

## Update 15 Jun (late PM) — store locator + stats fix
- Store locator (#stores on index.html): real interactive Leaflet map + searchable directory built from Alon's 264-store CSV. Data in `public/stores.js` (window.STORES + window.STORE_STATES, regenerate from the CSV with the python in the daily note). Map = CartoDB Positron tiles + circleMarkers per state (sized by count, click to filter). Panel = search box + state chips + directory with Google Maps "Get directions" links.
- LEAFLET GOTCHA: another global `L` clobbers Leaflet's namespace at runtime, so `window.L` becomes a bare function. FIX: capture `window.LMAP=window.L` in an inline <script> immediately after the Leaflet <script>, and use LMAP. Map is lazy-init via IntersectionObserver (container is inside a .reveal, so init on scroll-in + invalidateSize).
- STATS FIX: site.css `.statrow .st span` was shrinking the `.count` number (same bug pattern as `.stat>span`). Scoped to `.statrow .st>span`. Fixes texture/about/food-service stat numbers.

## Update 17 Jun 2026 — GSAP scroll-scrub blender sequence (Alon's #1 ask, built)
Canonical repo is now **toprmrproducer/simpliigood-website** (Netlify only; old ShreyasRajthe1st/Alon-s-website is dead/outdated). Deploy: `netlify deploy --prod --dir=dist --site=ebef461c-4bd5-4ee6-957f-90bf83f69057`.

NEW: Apple-style canvas image-sequence scroll-scrub inserted right after the hero (`<section id="scrolly" class="scrollyseq">`). Narrative = sun (yellow) -> zoom -> frozen spirulina cube (green) -> cube drops into a blender of fruit (yellow). Exactly Alon's Visual Flow Map storyboard ("1st scroll animation").
- FRAMES: built from Alon's two Veo frame folders in ~/Downloads ("First Video Frames" 254 + "2nd Frames Video"/grinder 102 = 356). Processed via `/tmp/build-seq.sh` (ffmpeg): crop bottom 56px to kill the "Veo" watermark, scale, q-jpeg. Output to `public/images/seq/`:
  - `seq_0001..0356.jpg` (960w desktop, ~33KB ea), `m/m_0001..0119.jpg` (560w, every-3rd, mobile), `poster.jpg` (last blender frame), `manifest.json`. ~9MB total.
- COMPONENT: CSS `.scrollyseq{height:440vh}` + `position:sticky` pin (NO ScrollTrigger pin — avoids Lenis conflict). JS player at bottom of index.html: preloads frames (mobile uses the 119-set), draws cover-fit to canvas on a `ScrollTrigger {scrub:0.5}` keyed to section progress, 3 scroll-synced captions (REAL SUNSHINE / SUNSHINE IN A CUBE / DROP IT IN. BLEND IT UP), load bar, scroll hint.
- PROGRESSIVE ENHANCEMENT: `poster.jpg` <img> is visible by default; canvas fades in (`.ready`) once frame 0 decodes, then poster hides. prefers-reduced-motion keeps the static poster. Never blank.
- PENDING (Alon's 2nd scroll animation): a glass filling with spirulina juice in the "SPIRULINA IS A SOLAR-POWERED NUTRIENT FACTORY" stats section. No frames for it yet — needs a pour clip/sequence.

## Update 17 Jun 2026 (PM) — Nano Banana 2 imagery + fly-in system + footer v2 (client friend approved)
**AI IMAGERY (working path, finally).** Google Nano Banana 2 / Pro via the funded GEMINI key — NOT Magnific (that MCP has Nano Banana 2 but is out of credits: 75 cr/image, ~12 left).
- Model `gemini-3-pro-image-preview` (SOTA, best brand/text fidelity), `generateContent`, ~1300 img-tokens ≈ **$0.13/image** (under the $0.50 cap). Verify the key's image models with `GET .../v1beta/models?key=$GEMINI_API_KEY` (filter name~image).
- Reusable generator: `/tmp/nb.py` (urllib; args: out ratio prompt [refs…]; body = responseModalities IMAGE + imageConfig{aspectRatio,imageSize:2K}, falls back to bare body on 400). `set -a; source ~/.claude/credentials/gemini.env; set +a` before running (vars need exporting). Pass `hero-pack.webp` as a reference for any pouch shot so the brand pack stays faithful.
- 5 new immaculate shots added (Pillow-opt to webp ≤1600/q82): `footer-pack.webp` (4:5 pedestal pack — footer), `cube-macro.webp` (1:1 frozen cube), `smoothie-pour.webp` (4:5 green pour), `flatlay-fresh.webp` (3:2 flatlay), `breakfast-bowl.webp` (3:2 lifestyle). Gallery now shows 4 of them; old slots (pack-splash/wood-product/flatlay-tray/smoothie-bowl) swapped out.

**FLY-IN ANIMATIONS (Alon's ask "why can't images fly in").** New `data-reveal="fly"` + `data-fly="left|right|up|down|zoom"` system in the index.html inline script: a per-element `ScrollTrigger.create` → `gsap.from(el,{x/y/rotation/scale,autoAlpha:0,immediateRender:false,once})`. Content is visible by default (no CSS hide), so JS-off / GSAP-fail still shows everything. Applied to: gallery figures (alternating dirs), the big section media (fresh/shop/sustain/b2b/tt), and the footer copy+media.
- HERO entrance is now a CSS-only fly-in: `@keyframes heroFly{translateX(54px)→0 + fade}` on `.js-on .hero-visual`. The conflicting gsap `.hero-visual` yPercent parallax was REMOVED (a CSS transform animation + gsap transform on the same element fight and the `both` fill freezes the parallax). The inner pack img keeps its `data-speed` parallax (different element). Respects prefers-reduced-motion (existing media query).

**FOOTER v2.** Closing band (`.footer-hero`) now leads with the immaculate `footer-pack.webp` (portrait 4:5, `max-height:564px`) + bigger headline (`clamp(2.9rem,6vw,6rem)`) + Shop CTA + "10 cubes · ships frozen" badge; newsletter band slimmed below; giant animated wordmark unchanged. Inspired by footer.design refs Alon shared (editorial, big type, strong product image).
- Verified live (webkit Playwright): 0 console errors, 0 broken images, 15 fly elements, none stuck invisible, hero `heroFly` active. Deployed commit pending.
