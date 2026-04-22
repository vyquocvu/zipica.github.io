# Design Brief: Rebuild Zipica Homepage (index.html)

## 1. Feature Summary

A complete redesign of the Zipica marketing homepage. The page must communicate what Zipica is, build trust, and drive app downloads — for everyday phone users who want effortless photo management, not a technical tool. Every element should feel calm, refined, and intentional — never flashy or aggressive.

## 2. Primary User Action

**Download the app.** The page flows from curiosity → understanding → confidence → action. Primary CTA is "Download Free" (Lite tier).

## 3. Design Direction

### Aesthetic: Refined Editorial Minimal
Think: a well-lit gallery wall in a Scandinavian apartment. Clean surfaces, warm materials, nothing competing for attention. Not brutalist (too cold), not luxury (too exclusive) — approachable premium.

**Reference**: A cross between Apple's product page restraint and Monocle magazine's editorial warmth.

### How This Feature Expresses the Brand Context
- **Light, clean, effortless** — nothing fights for attention; everything earns its place
- **Warm neutrals** — no cold gray, no flat pure white; surfaces have a subtle blue tint from the brand
- **Typography-led hierarchy** — size and weight carry meaning; color is used with extreme restraint
- **Restrained motion** — scroll reveals are subtle, fast (200ms), purposeful; no ambient floating or gradient animation
- **No AI defaults** — explicitly absent: gradient text, glassmorphism cards, animated gradients, decorative blur glows, purple/cyan color schemes, floating animations

### Color Palette (OKLCH)

**Light Mode**
- Background: `oklch(98% 0.005 250)` — near-white with subtle blue tint
- Surface (cards, navbar): `oklch(96% 0.008 250)` — slightly deeper, same blue family
- Text primary: `oklch(15% 0.02 250)` — near-black with blue cast
- Text secondary: `oklch(45% 0.015 250)` — muted, readable
- Border: `oklch(88% 0.01 250)` — very subtle, visible
- Brand accent (blue): `oklch(52% 0.17 250)` — the Zipica blue, used SPARINGLY (10% rule)
- Success: `oklch(58% 0.14 145)` — OKLCH green
- Caution: `oklch(72% 0.13 80)` — warm amber

**Dark Mode**
- Background: `oklch(14% 0.015 250)` — near-black with blue cast, NOT pure black
- Surface: `oklch(18% 0.015 250)` — slightly lighter than bg for elevation
- Text primary: `oklch(94% 0.005 250)` — near-white with blue cast
- Text secondary: `oklch(68% 0.01 250)` — muted
- Border: `oklch(28% 0.015 250)` — subtle
- Brand accent: `oklch(58% 0.16 250)` — slightly lighter on dark for vibrance
- Same success/caution hues, slightly lighter

### Typography
- **Display font**: Nunito Sans — warm, rounded, approachable, distinctive without being "designed". NOT in reflex list. (Fallback: system-ui)
- **Body font**: Nunito Sans — single family creates cohesion; vary weight and size for hierarchy
- **NO**: gradient text for emphasis — use weight (`font-bold`) and size instead
- Type scale (marketing page, fluid):
  - Hero headline: `clamp(2.5rem, 5vw, 4.5rem)`, weight 800, leading 1.05
  - Section headline: `clamp(2rem, 4vw, 3rem)`, weight 800, leading 1.1
  - Subheading: `1.25rem`–`1.5rem`, weight 600
  - Body: `1rem`, weight 400, leading 1.65, max-width 70ch
  - Caption/label: `0.8125rem`, weight 600, tracking 0.04em

### Motion (purposeful only)
- **Page load**: No stagger reveals — content is visible immediately, not hidden
- **Scroll reveals**: `opacity 0→1, translateY(16px→0)`, 300ms ease-out, triggered at 15% viewport entry
- **Hover states**: 150ms color/shadow transitions — fast, snappy
- **Dark mode toggle**: 200ms color transition on all themed properties
- **Reduced motion**: Respect `prefers-reduced-motion` — disable all transforms and opacity transitions, keep color-only changes

## 4. Layout Strategy

### Visual Pacing
- **Hero** (loud) — large headline, generous space, single focused CTA
- **Features** (medium) — 3-column grid, tight internal rhythm
- **Gallery preview** (quiet) — minimal chrome, the image speaks
- **Feature spotlights** (loud→medium) — alternating layout, image + text, alternating left/right
- **Tech stack** (quiet) — sparse pills, no container
- **Pricing** (loud) — clear two-column comparison, Pro gets subtle elevation
- **Download CTA** (loud) — brand blue background, high contrast
- **Footer** (quiet) — minimal, text-only

### Spatial Rhythm
- Page max-width: 1200px centered
- Section vertical padding: `clamp(4rem, 8vw, 7rem)` top and bottom
- Card gap: 24px
- Content gap within sections: 48px (lg), 32px (md), 24px (sm)
- No uniform padding everywhere — hero has more vertical space than feature cards

### Asymmetry
- Feature spotlights alternate image left / image right — breaks grid monotony
- Hero CTA is left-aligned on desktop (not centered) — asymmetric feels more designed
- Pricing: Pro card is elevated but not with a garish scale transform; use shadow and border

## 5. Key States

### Static States
- **Default (light mode)**: Full palette as defined above
- **Dark mode**: Inverted surface hierarchy, same blue accent, no shadows (depth from lightness)
- **Reduced motion**: All transforms disabled, instant color transitions

### Interactive Elements
- **Nav links**: Underline on hover (not color change), 150ms transition
- **CTA buttons**: Background darkens 8% on hover, slight shadow lift, 150ms
- **Feature cards**: Subtle border color shift on hover, 150ms — NO translateY lift (feels dated)
- **Pricing cards**: Border color shifts to brand on hover
- **Theme toggle**: Icon swaps (sun↔moon), 200ms fade

## 6. Interaction Model

- **Scroll**: Smooth scroll for nav links
- **Nav**: Fixed top, transparent background with subtle border-bottom; on scroll past hero, background solidifies (blur removed for performance)
- **Theme toggle**: Instant localStorage + class toggle; no FOUC (block on `<html>` class)
- **Scroll reveals**: IntersectionObserver at 15% threshold, one-shot (unobserve after trigger)
- **Links**: External links to App Store/Play Store are placeholder `#` hrefs

## 7. Content Requirements

All existing content is preserved — headlines, descriptions, feature names, pricing copy. Only the visual treatment changes.

**Copy to keep (do not rewrite):**
- Hero headline: "Your photos, blazingly organised."
- Hero subtext: about reclaiming storage, cleaning duplicates, vault
- 6 feature cards: Secure Vault, Video Compression, Duplicate Detection, Batch Operations, Zip Archiving, Image Merging
- Gallery section headline: "A gallery that keeps up with you."
- 4 feature spotlights: Cleanup, Vault, Compress Video, Merge Images
- Tech stack: TypeScript, Vite, TailwindCSS, Capacitor, FFmpeg.wasm, Framer Motion, Lucide Icons, IndexedDB
- Pricing: Lite ($0 free) and Pro ($3.99 one-time)
- CTA: "Get Zipica today." / "Available on iOS, Android."

**Content to remove:**
- "blazingly" should NOT be gradient text — use bold weight only
- Stats row (Smooth Scrolling / Responsive Columns) — feels like placeholder fluff, remove
- Feature pills emoji — 🔷⚡🎨📱🎬✨🖼️🗄️ — replace with clean text or inline SVG icons only
- "Blazingly small" in Lite pricing — remove, replace with actual value prop

## 8. Recommended References

- `spatial-design.md` — for the 4pt spacing scale, asymmetric layout, visual rhythm
- `typography.md` — for fluid type scale, weight hierarchy, character limit on body
- `color-and-contrast.md` — for OKLCH palette construction, tinted neutrals, dark mode surface hierarchy
- `motion-design.md` — for 100-150ms hover transitions, reduced-motion, purpose-driven animation
- `interaction-design.md` — for button hierarchy (primary/ghost/secondary), focus states

## 9. Open Questions

| Question | Decision |
|----------|----------|
| Phone mockup images | Keep — they demonstrate the app. Style them cleanly: no CSS phone frame, no glow backdrop, just clean bordered frame |
| Tech stack pills with emoji | Replace emoji with simple inline SVG icons or text-only |
| Gallery preview image | Keep, but remove the decorative blur glow behind it |
| Feature section background | Light gray tint (`oklch(96% 0.008 250)`) alternating with white |
| Navbar blur effect | Remove backdrop blur — just solid or semi-transparent background |
| Hero background blobs | Remove — pure clean background, no decorative blobs |
| Logo mark (Z in gradient box) | Keep the Z mark but replace animated gradient with solid brand blue |
| Pricing Pro badge ("Most Popular") | Keep as text label with brand color background (no gradient) |
| Download section background | Solid brand blue, not animated gradient — `oklch(52% 0.17 250)` |

## 10. What Gets Cut (vs. current page)

| Removed | Replacement |
|---------|------------|
| `bg-gradient-to-r` + `bg-clip-text` on hero | Bold weight + solid brand color |
| `animated-gradient` keyframes/background | Solid `oklch(52% 0.17 250)` |
| `float` keyframe animation | None — static, dignified |
| `glass` class (glassmorphism) | Solid card with subtle border |
| `backdrop-blur` | Solid surface |
| Decorative blur glows (`blur-3xl`) | None |
| Purple background blobs | None |
| `reveal` animation on page load | Content visible immediately |
| Shimmer animation | None |
| Emoji in tech pills | Text labels only |
| Stats row | Remove |
| CSS phone frame with notch pseudo-element | Simple bordered image container |