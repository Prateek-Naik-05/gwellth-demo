# GWellth Homepage — Master Rebuild Specification

> **Project:** GWellth Foods & Beverages Pvt. Ltd. — Homepage from scratch  
> **Aesthetic:** Dark Luxury Natural — premium artisan apothecary meets Himalayan mountain retreat  
> **Stack:** Single-file HTML + CSS + Vanilla JS (or React/Next.js)  
> **Version:** 1.0 — March 2026

---

## Table of Contents

1. [Design System Foundations](#1-design-system-foundations)
2. [Section 01 — Navigation Bar](#2-section-01--navigation-bar)
3. [Section 02 — Hero](#3-section-02--hero)
4. [Section 03 — Stats Marquee Strip](#4-section-03--stats-marquee-strip)
5. [Section 04 — Popular Categories](#5-section-04--popular-categories)
6. [Section 05 — Featured Products Grid](#6-section-05--featured-products-grid)
7. [Section 06 — Brand Story](#7-section-06--brand-story)
8. [Section 07 — Health Benefits](#8-section-07--health-benefits)
9. [Section 08 — Tabbed Products Collection](#9-section-08--tabbed-products-collection)
10. [Section 09 — Testimonials](#10-section-09--testimonials)
11. [Section 10 — Newsletter + Footer](#11-section-10--newsletter--footer)
12. [Interactions Master Table](#12-interactions-master-table)
13. [The Complete Copy-Paste Build Prompt](#13-the-complete-copy-paste-build-prompt)

---

## 1. Design System Foundations

### 1.1 Brand Color Tokens

Define all colors as CSS custom properties at `:root`. Never hardcode hex values anywhere else.

```css
:root {
  /* Primary Palette */
  --deep-earth:   #1C1208;  /* darkest — backgrounds, headers */
  --amber:        #C47A2A;  /* primary brand — buttons, accents */
  --honey-gold:   #F2B84B;  /* display headlines, highlights */
  --forest:       #2F5930;  /* secondary — nature/story sections */
  --sage:         #6B8F6B;  /* muted forest, supporting elements */

  /* Neutral Palette */
  --parchment:    #F9F2E5;  /* primary page background */
  --cream:        #FDF6EC;  /* card backgrounds */
  --rust:         #8B2E0F;  /* urgency, spice/condiments accent */
  --ink:          #1A1209;  /* body text */
  --muted:        #8A7A65;  /* secondary/supporting text */
  --border:       #E8DDD0;  /* dividers, card borders */

  /* Semantic Aliases */
  --bg-page:      var(--parchment);
  --bg-dark:      var(--deep-earth);
  --text-primary: var(--ink);
  --cta-primary:  var(--amber);
  --cta-hover:    #D4892F;
}
```

**Color usage rules:**
- Alternate section backgrounds between `--parchment` and `--cream` for light sections
- Use `--deep-earth` and `--forest` for high-contrast dark sections
- **Never use plain white (#ffffff)** — always use `--cream` or `--parchment`
- `--honey-gold` is reserved for display headlines and key highlight moments only
- `--rust` is used exclusively for discount badges, urgency CTAs, and spice product accents

---

### 1.2 Typography System

Import from Google Fonts:

```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700;1,900&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
```

| Role | Font | Weight | Size | Line Height | Notes |
|---|---|---|---|---|---|
| Display / H1 | Playfair Display | 900 Italic | 72–96px | 0.95 | Brand hero moments only |
| Section Heading / H2 | Playfair Display | 700 | 40–56px | 1.1 | All section headings |
| Sub-heading / H3 | Playfair Display | 400 Italic | 28–36px | 1.2 | Supporting headings |
| Body Copy | DM Sans | 300 | 15–16px | 1.7 | All paragraph text |
| UI / Buttons | DM Sans | 500–600 | 13–15px | 1 | CTAs, nav links |
| Labels / Tags / Data | Space Mono | 400 | 10–12px | 1 | Eyebrows, badges, prices |

> **Critical:** Never substitute with Arial, Inter, Roboto, or system fonts. The Playfair Display italic is the single most important typographic decision — it communicates warmth, craftsmanship, and premium heritage.

---

### 1.3 Global Atmosphere & Texture

**Grain Overlay**
Apply a subtle SVG noise texture as a fixed overlay on `<body>`:
```css
body::after {
  content: '';
  position: fixed;
  inset: 0;
  background-image: url("data:image/svg+xml,..."); /* SVG feTurbulence noise */
  opacity: 0.025;
  pointer-events: none;
  mix-blend-mode: overlay;
  z-index: 9999;
}
```

**Ambient Orbs (Dark Sections)**
In dark sections (hero, testimonials, footer), add 2–3 large radial-gradient orbs:
```css
.section-dark::before {
  content: '';
  position: absolute;
  width: 600px; height: 600px;
  background: radial-gradient(circle, rgba(196,122,42,0.18) 0%, transparent 70%);
  filter: blur(80px);
  top: -200px; right: -100px;
  pointer-events: none;
}
```

**Section Dividers**
Between major sections, use an SVG wave or organic curve shape (60–80px height) in the receiving section's background color — never hard horizontal lines.

---

### 1.4 Custom Cursor

Two-element cursor system (desktop only):

- **Main dot:** 10×10px circle, `background: var(--amber)`, `mix-blend-mode: difference`, tracks pointer exactly with no lag
- **Follower ring:** 36×36px hollow circle, `border: 1.5px solid rgba(196,122,42,0.4)`, follows cursor with 80ms ease-out lag

**State changes:**

| Context | Follower Ring Behavior |
|---|---|
| Default | 36px, amber outline |
| Hover: links/buttons | Scales to 60px, amber fill at 10% opacity |
| Hover: product images | Scales to 72px, shows "VIEW" in Space Mono 9px centered |
| Hover: draggable/carousel | Shows "DRAG" text instead |

---

## 2. Section 01 — Navigation Bar

**Height:** 72px fixed  
**Position:** `position: fixed; top: 0; width: 100%; z-index: 1000`

### 2.1 Announcement Bar

- **Height:** 36px, sits above navbar
- **Background:** `var(--forest)`
- **Content:** Infinite CSS marquee — `"🌿 Free Shipping Above ₹499 · 100% Natural Products · Sourced from Uttarakhand · Ships to 35+ Countries"`
- **Font:** DM Sans 400 12px, white
- **Animation:** 30s linear infinite scroll
- **Behavior:** Slides up and hides when user scrolls 10px (transition: 300ms ease)

### 2.2 Navbar States

**Initial (over hero):**
```css
nav {
  background: transparent;
  border-bottom: none;
  transition: all 400ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

**Scrolled (after 80px):**
```css
nav.scrolled {
  background: rgba(28, 18, 8, 0.95);
  backdrop-filter: blur(20px) saturate(180%);
  border-bottom: 1px solid rgba(242, 184, 75, 0.12);
}
```

### 2.3 Logo

- Font: Playfair Display 700, parchment
- "Wellth" portion: italic, `var(--honey-gold)`
- Small lotus/leaf SVG icon (24px) to the left
- Total width: ~160px

### 2.4 Center Navigation Links

Links: **Home · Shop · Our Story · Benefits · Blog**

- Font: DM Sans 500, 14px, `var(--parchment)` at 80% opacity
- Hover: full opacity + 2×2px amber dot appears beneath, centered, 200ms ease
- Active page: 2px amber underline, 6px offset below text

### 2.5 Right Zone

Three elements in a flex row with 16px gap:
1. **Search icon** — magnifier SVG, 20px, parchment
2. **Wishlist icon** — heart SVG with count bubble (amber background, white Space Mono 9px)
3. **Cart pill** — `background: var(--amber)`, border-radius 8px, padding 8px 18px, DM Sans 500 13px white, shows item count. Hover: `scale(1.04)` + background → `--cta-hover`

### 2.6 Mobile Navigation

- **Hamburger:** Three 24px lines, 1.5px height, amber, 5px gap. Animates to X on click (300ms ease)
- **Drawer:** Slides from left, 85vw max 360px, `background: var(--deep-earth)`. Contains logo, large nav links (Playfair Display 28px, 60ms staggered fade-in), social icons at bottom
- **Backdrop:** Semi-transparent overlay behind drawer, click to close

---

## 3. Section 02 — Hero

**Height:** `min-height: 100vh`  
**Background:** `var(--deep-earth)`

### 3.1 Background Layers (bottom to top)

1. **Base color:** `#1C1208`
2. **Radial gradient 1:** `radial-gradient(ellipse 800px 600px at 80% 20%, rgba(196,122,42,0.22) 0%, transparent 70%)`
3. **Radial gradient 2:** `radial-gradient(ellipse 500px 400px at 10% 90%, rgba(47,89,48,0.15) 0%, transparent 70%)`
4. **Product photography:** Right-positioned (right: 0, width: 55%), fading into dark via `mask-image: linear-gradient(to left, rgba(0,0,0,0.9) 60%, transparent 100%)`
5. **Grain noise:** Fixed SVG noise at 3% opacity, `mix-blend-mode: overlay`

### 3.2 Text Content (Left Column)

**Eyebrow badge:**
```css
.hero-badge {
  background: rgba(242, 184, 75, 0.12);
  border: 1px solid rgba(242, 184, 75, 0.3);
  color: var(--honey-gold);
  font-family: 'Space Mono', monospace;
  font-size: 10px;
  letter-spacing: 3px;
  text-transform: uppercase;
  padding: 8px 18px;
  border-radius: 99px;
}
```
Text: `"✦ NATURAL · PREBIOTIC · HIMALAYAN"`

**H1 Headline (3 separate `<span>` elements for individual animation):**
- Line 1: `"Nature's Most"` — Playfair Display 900, 80–96px, `var(--parchment)`
- Line 2: `"Powerful Gut"` — Playfair Display 900 **italic**, 80–96px, `var(--honey-gold)`
- Line 3: `"Foods."` — Playfair Display 900, 80–96px, `rgba(249,242,229,0.25)`

**Subheadline:**
> "Prebiotic honey, Himalayan jaggery, ancient millets — handpicked from Uttarakhand's pristine altitude farms to fuel your gut, immunity, and energy."

DM Sans 300, 18px, `rgba(249,242,229,0.6)`, max-width 480px, line-height 1.7

**CTA Buttons (16px gap):**

| Button | Style |
|---|---|
| Primary: "Shop Our Products" | `background: var(--amber)`, white text, DM Sans 500 15px, padding 16px 36px, border-radius 6px. Hover: `translateY(-2px)` + `box-shadow: 0 8px 32px rgba(196,122,42,0.4)` |
| Secondary: "Our Story →" | Transparent, `border: 1px solid rgba(249,242,229,0.25)`, parchment text. Hover: border → `rgba(249,242,229,0.6)` |

**Social proof row** (below CTAs, 24px gap):
`"⭐ 4.8/5 Rating"` · `"1,000+ Customers"` · `"35+ Countries"`  
DM Sans 400 13px, `rgba(249,242,229,0.5)`. Numbers in `var(--honey-gold)`.

### 3.3 Page Load Animation Sequence

All elements start at `opacity: 0`. Easing: `cubic-bezier(0.16, 1, 0.3, 1)` (spring).

| Element | From | To | Delay | Duration |
|---|---|---|---|---|
| Eyebrow badge | `opacity:0; translateY(16px)` | `opacity:1; translateY(0)` | 0ms | 600ms |
| H1 Line 1 | `opacity:0; translateY(40px)` | `opacity:1; translateY(0)` | 150ms | 800ms |
| H1 Line 2 | `opacity:0; translateY(40px)` | `opacity:1; translateY(0)` | 260ms | 800ms |
| H1 Line 3 | `opacity:0; translateY(40px)` | `opacity:1; translateY(0)` | 370ms | 800ms |
| Subheadline | `opacity:0; translateY(20px)` | `opacity:1; translateY(0)` | 500ms | 700ms |
| CTA buttons | `opacity:0; translateY(16px)` | `opacity:1; translateY(0)` | 650ms | 600ms |
| Social proof | `opacity:0` | `opacity:1` | 800ms | 500ms |
| Product image | `opacity:0; translateX(60px) scale(1.04)` | `opacity:1; translateX(0) scale(1)` | 200ms | 1000ms |

**Post-load floating animation (product image):**
```css
@keyframes float {
  0%, 100% { transform: translateY(-10px); }
  50%       { transform: translateY(10px); }
}
.hero-product { animation: float 4s ease-in-out infinite; }
```

**Scroll indicator** (bottom-center of hero):
- 2px × 48px amber vertical line, pulsing opacity 1→0.3→1 on 1.8s loop
- Below: "SCROLL" in Space Mono 9px, 3px letter-spacing, parchment 40%
- Hides when user scrolls past 100px

---

## 4. Section 03 — Stats Marquee Strip

**Height:** 64px  
**Background:** `var(--amber)`  
**Layout:** Two stacked rows, edge-to-edge

### 4.1 Content

Stats displayed: `100% Natural Products · 35+ Countries Shipped · 1,000+ Happy Customers · 50+ Premium SKUs · 0 Artificial Additives · 3+ Years of Wellness`

- Numbers: Playfair Display 700 italic, 22px
- Labels: DM Sans uppercase, 11px, letter-spacing 2px
- Separator: `◆` character, 8px, `rgba(255,255,255,0.35)`
- All text: white

### 4.2 Animation

```css
@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-33.33%); }
}

.strip-row-1 { animation: marquee 28s linear infinite; }
.strip-row-2 {
  animation: marquee 38s linear infinite reverse;
  opacity: 0.8;
}

/* Pause on hover */
.marquee-container:hover .strip-row-1,
.marquee-container:hover .strip-row-2 {
  animation-play-state: paused;
}
```

Duplicate content 3× inside each row for seamless looping.

---

## 5. Section 04 — Popular Categories

**Background:** `var(--parchment)`  
**Padding:** 100px vertical, 60px horizontal  
**Max-width:** 1200px, centered

### 5.1 Section Header

- Eyebrow: Space Mono 10px uppercase, 4px letter-spacing, `var(--amber)` — text: `"EXPLORE OUR WORLD"`
- Heading: Playfair Display 700 italic, 52px, ink — text: `"Four Worlds of Wellness"`

### 5.2 Grid Layout

```css
.categories-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

@media (max-width: 900px)  { grid-template-columns: repeat(2, 1fr); }
@media (max-width: 480px)  { grid-template-columns: 1fr; }
```

### 5.3 Card Design

**Base state:**
- Aspect ratio: 3/4
- Border-radius: 16px
- Overflow: hidden
- Position: relative
- Cursor: pointer

**Layer stack:**
1. Full-card product photography
2. Gradient overlay: `linear-gradient(to top, rgba(28,18,8,0.85) 0%, rgba(28,18,8,0.2) 50%, transparent 100%)`
3. Category name (Playfair Display 700, 26px, white) + item count (Space Mono 10px, white 60%) + amber `→` arrow — all pinned to bottom with 24px padding

**Per-category accent tint added to gradient overlay:**

| Category | Tint Color |
|---|---|
| Prebiotic Honey | `rgba(196,122,42,0.3)` — amber |
| HimGur Jaggery | `rgba(139,46,15,0.3)` — rust |
| HimMillets | `rgba(47,89,48,0.3)` — forest |
| South Indian | `rgba(139,46,15,0.35)` — rust/spice |

### 5.4 Hover State

```css
.category-card:hover .category-photo {
  transform: scale(1.08);
  transition: transform 600ms ease;
}

/* Overlay deepens */
.category-card:hover .overlay { opacity: 1; }

/* Description text reveals */
.category-description {
  transform: translateY(20px);
  opacity: 0;
  transition: all 400ms ease;
}
.category-card:hover .category-description {
  transform: translateY(0);
  opacity: 1;
}

/* Amber border-inset */
.category-card:hover {
  box-shadow: inset 0 0 0 2px var(--amber);
  transition: box-shadow 300ms ease;
}
```

Also reveals a "Shop Now →" button at bottom (amber background, 12px DM Sans 500).

### 5.5 Scroll-Reveal Animation

Use `IntersectionObserver` with `threshold: 0.15`. Stagger per card:

| Card | Delay |
|---|---|
| Card 1 | 0ms |
| Card 2 | 120ms |
| Card 3 | 240ms |
| Card 4 | 360ms |

From: `opacity:0; translateY(40px)` → To: `opacity:1; translateY(0)` — 700ms ease-out.

---

## 6. Section 05 — Featured Products Grid

**Background:** `var(--deep-earth)` with 2 ambient orb gradients  
**Padding:** 100px vertical

### 6.1 Section Header

- Two-column flex: heading left, "View All →" link right
- Heading: `"Our Bestselling Prebiotic Honey"` — Playfair Display 700 italic, 52px, parchment

### 6.2 Grid

```css
.products-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
@media (max-width: 900px) { grid-template-columns: repeat(2, 1fr); }
@media (max-width: 480px) { grid-template-columns: 1fr; }
```

### 6.3 Product Card Anatomy

**Card container:**
```css
.product-card {
  background: rgba(249, 242, 229, 0.05);
  border: 1px solid rgba(249, 242, 229, 0.08);
  border-radius: 16px;
  overflow: hidden;
  transition: transform 400ms ease, box-shadow 400ms ease;
}
```

**Image zone (top 65%):**
- Background: `var(--cream)` — warm off-white
- Product image centered at 80% width with drop-shadow
- Image floats in warm background (no hard edges)
- **Discount badge:** top-left corner, `background: var(--rust)`, white Space Mono 10px (e.g., `"−23%"`), border-radius `0 0 6px 0`
- **Wishlist heart:** top-right, 24px outlined white heart at 50% opacity

**Info zone (bottom 35%):**
- Padding: 20px 22px
- Product name: DM Sans 500, 16px, parchment
- Variant pills: small `border-radius: 99px` pills (amber/forest/sage per variant)
- **Price row:** strikethrough original (Space Mono 12px muted) + sale price (Playfair Display 700, 22px, honey-gold) + Add to Cart icon button (36px circle, amber bg, white bag icon, far right)

### 6.4 Hover Interactions

```css
.product-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 24px 64px rgba(0,0,0,0.3), 0 0 0 1px rgba(196,122,42,0.3);
}

/* Product image scales */
.product-card:hover .product-image { transform: scale(1.06); }

/* Quick View overlay */
.quick-view {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 300ms ease;
}
.product-card:hover .quick-view { opacity: 1; }
.quick-view-btn {
  background: rgba(28, 18, 8, 0.7);
  backdrop-filter: blur(10px);
  color: white;
  padding: 10px 20px;
  border-radius: 99px;
  font-size: 13px;
}

/* Add to Cart expands */
.add-to-cart {
  width: 36px;
  transition: width 300ms cubic-bezier(0.4, 0, 0.2, 1);
  overflow: hidden;
  white-space: nowrap;
}
.product-card:hover .add-to-cart { width: 140px; }
```

**Wishlist heart click:**
- Outlined → filled amber (instant)
- Scale pulse: `scale(1.4)` → `scale(1)` in 300ms
- "+1" text floats upward and fades out (translateY -20px, opacity 0, 500ms)

---

## 7. Section 06 — Brand Story

**Background:** `var(--forest)` with botanical SVG leaf pattern at 3% opacity  
**Layout:** Two equal columns (text left, visual right)  
**Padding:** 120px vertical

### 7.1 Visual Column (Right)

- Large nature/product photography
- Clipped into an organic SVG shape (flowing, brushstroke-like clip-path — not a rectangle)
- Image filter: `sepia(10%) saturate(110%)` for warm, earthy feel
- CSS clip-path example:
```css
.story-image {
  clip-path: path('M 0,80 Q 40,0 200,20 Q 360,40 380,160 Q 400,280 300,360 Q 200,440 80,400 Q -40,360 0,240 Z');
}
```

### 7.2 Text Column (Left)

- **Eyebrow:** Space Mono 10px, uppercase, honey-gold — `"OUR STORY"`
- **Heading:** Playfair Display 700 italic, 52px, parchment — `"Born from the Heart of the Himalayas"`
- **Drop-cap:** First letter of first paragraph in Playfair Display 900, 80px, honey-gold, float left
- **Body:** DM Sans 300, 16px, `rgba(249,242,229,0.7)`, 3 paragraphs on brand origin and mission
- **CTA link:** `"Read Our Full Story →"` in amber, DM Sans 500 14px. Hover: arrow moves 4px right

### 7.3 Floating Stat Cards (3 cards)

Positioned overlapping the boundary between text and image columns:

```css
.stat-card {
  background: rgba(249, 242, 229, 0.08);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(249, 242, 229, 0.12);
  border-radius: 10px;
  padding: 16px 20px;
}
```

Content per card: large number (Playfair Display 700, 28px, honey-gold) + label (Space Mono 10px, white 60%)

Stats: `"35+"` Countries · `"1,000+"` Customers · `"100%"` Natural

### 7.4 Parallax

```javascript
window.addEventListener('scroll', () => {
  const scrollY = window.scrollY;
  const storySection = document.querySelector('.story-section');
  const offsetTop = storySection.offsetTop;
  const relativeScroll = scrollY - offsetTop;

  // Background image moves at 0.4x scroll speed
  storyBg.style.transform = `translateY(${relativeScroll * 0.4}px)`;

  // Stat cards move at 0.2x speed (feel physically closer)
  statCards.style.transform = `translateY(${relativeScroll * 0.2}px)`;
});
```

---

## 8. Section 07 — Health Benefits

**Background:** `var(--parchment)`  
**Padding:** 100px vertical

### 8.1 Section Header (Centered)

- Eyebrow: `"SCIENCE-BACKED WELLNESS"` — Space Mono, amber
- Heading: `"Why GWellth Works"` — Playfair Display 700 italic, 52px

### 8.2 Grid

```css
.benefits-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}
```

6 total cards. The **center card of row 1** is a "hero card" — dark background variant.

### 8.3 Standard Card

```css
.benefit-card {
  background: white;
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 36px 28px;
}
```

**Zones:**
1. **Icon box:** 72×72px, border-radius 16px, color-coded background tint. SVG icon 36px in matching accent color
2. **Benefit name:** Playfair Display 700, 22px, ink. Margin top: 20px
3. **Description:** DM Sans 300, 14px, `#6B5C4E`, line-height 1.7. 2–3 sentences max
4. **"Explore →" link:** amber, DM Sans 500 13px, arrow moves 4px right on hover

**Icon color mapping:**

| Benefit | Icon Box Tint | Icon Color |
|---|---|---|
| Gut Health | `rgba(47,89,48,0.1)` | `var(--forest)` |
| Natural Energy | `rgba(196,122,42,0.1)` | `var(--amber)` |
| Immunity Boost | `rgba(139,46,15,0.1)` | `var(--rust)` |
| Antioxidant | `rgba(242,184,75,0.1)` | `var(--honey-gold)` |
| Heart Health | `rgba(139,46,15,0.1)` | `var(--rust)` |
| Skin & Energy | `rgba(196,122,42,0.1)` | `var(--amber)` |

### 8.4 Hero Card (Center, Row 1)

```css
.benefit-card.hero {
  background: var(--deep-earth);
  border-color: rgba(242, 184, 75, 0.2);
}
.benefit-card.hero .benefit-name { color: var(--honey-gold); }
.benefit-card.hero .benefit-desc { color: rgba(249, 242, 229, 0.65); }
```

### 8.5 Scroll Reveal

Two waves — row 1 triggers first, row 2 triggers when it enters viewport.

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const row = entry.target.dataset.row;
      const col = entry.target.dataset.col;
      const delay = parseInt(col) * 120;
      setTimeout(() => {
        entry.target.classList.add('revealed');
      }, delay);
    }
  });
}, { threshold: 0.15 });
```

From: `opacity:0; translateY(50px)` → To: `opacity:1; translateY(0)` — 700ms ease-out.

---

## 9. Section 08 — Tabbed Products Collection

**Background:** `var(--cream)`  
**Padding:** 100px vertical

### 9.1 Section Header (Centered)

- Eyebrow: `"⚡ OUR COLLECTIONS"` — Space Mono, amber
- Heading: `"Explore Everything GWellth"` — Playfair Display 700, 56px
- Sub: `"Four distinct product lines. One mission: your wellness."` — DM Sans 300, 18px, muted

### 9.2 Tab Bar

```css
.tab-bar {
  display: inline-flex;
  background: rgba(28, 18, 8, 0.08);
  border-radius: 99px;
  padding: 6px;
  gap: 0;
  position: relative;
}

.tab-btn {
  padding: 12px 28px;
  border-radius: 99px;
  font-family: 'DM Sans', sans-serif;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  position: relative;
  z-index: 1;
  transition: color 300ms ease;
}

/* Sliding active pill — positioned absolutely */
.tab-indicator {
  position: absolute;
  background: var(--deep-earth);
  border-radius: 99px;
  transition: left 300ms cubic-bezier(0.4, 0, 0.2, 1), width 300ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

**Active tab:** DM Sans 500, white text. The dark pill indicator slides/morphs (not fades) between tabs.

Tab labels: `🍯 Prebiotic Honey · 🏔 HimGur Jaggery · 🌾 HimMillets · 🌶 South Indian`

### 9.3 Content Transition

```javascript
function switchTab(newTab, direction) {
  // Exit old content
  currentContent.style.transform = `translateX(${direction === 'left' ? '-20px' : '20px'})`;
  currentContent.style.opacity = '0';

  setTimeout(() => {
    // Enter new content
    newContent.style.transform = `translateX(${direction === 'left' ? '30px' : '-30px'})`;
    newContent.style.opacity = '0';
    newContent.style.display = 'flex';

    requestAnimationFrame(() => {
      newContent.style.transition = 'all 300ms ease';
      newContent.style.transform = 'translateX(0)';
      newContent.style.opacity = '1';
    });
  }, 200);
}
```

### 9.4 Product Carousel

- Desktop: 3 visible cards + peek of 4th (overflow visible, container clipped)
- Mobile: 1.2 cards visible, native touch swipe
- Left/right arrow buttons appear on hover over container
- Dot indicator row below, active dot: 20px amber pill, inactive: 6px circle at 30% opacity

---

## 10. Section 09 — Testimonials

**Background:** `var(--deep-earth)` with grain overlay  
**Padding:** 100px vertical

### 10.1 Section Header (Centered)

- Eyebrow: `"WHAT OUR CUSTOMERS SAY"` — Space Mono, honey-gold
- Heading: `"Loved Across India and Beyond"` — Playfair Display 700 italic, 52px, parchment. "Beyond" in honey-gold italic
- Rating row: 5 amber star emojis + `"Rated 4.8 out of 5 from 1,000+ reviews"` — DM Sans 300, 14px, parchment muted

### 10.2 Carousel Layout

3 visible cards with depth effect:

| Position | Scale | Opacity | Effect |
|---|---|---|---|
| Left (−1) | 0.93 | 0.5 | dimmed |
| Center (active) | 1.0 | 1.0 | full focus |
| Right (+1) | 0.93 | 0.5 | dimmed |

### 10.3 Testimonial Card

```css
.testimonial-card {
  background: rgba(249, 242, 229, 0.06);
  border: 1px solid rgba(249, 242, 229, 0.1);
  border-radius: 20px;
  padding: 36px;
  position: relative;
}

/* Large opening quote mark */
.testimonial-card::before {
  content: '"';
  font-family: 'Playfair Display', serif;
  font-size: 80px;
  font-weight: 900;
  color: rgba(196, 122, 42, 0.2);
  position: absolute;
  top: 20px;
  left: 28px;
  line-height: 1;
}
```

**Card content stack:**
1. Review text — DM Sans 300, 16px, parchment 80%, italic, line-height 1.7
2. **Reviewer row:** 40px avatar circle + name (DM Sans 600, 14px, parchment) + location (Space Mono 10px, amber) + 5 amber stars
3. **Verified badge (bottom):** `"Verified Purchase · [Product Name]"` — Space Mono 10px, forest background, white text, border-radius 99px

### 10.4 Auto-scroll Behavior

```javascript
let autoScroll = setInterval(advanceCarousel, 4500);

carouselContainer.addEventListener('mouseenter', () => clearInterval(autoScroll));
carouselContainer.addEventListener('mouseleave', () => {
  autoScroll = setInterval(advanceCarousel, 4500);
});
```

Transition: `600ms cubic-bezier(0.4, 0, 0.2, 1)`. Loops infinitely.

### 10.5 Navigation

- Arrow buttons: 48px circles, `background: rgba(249,242,229,0.08)`, hover: amber border + `rgba(249,242,229,0.15)` background
- Dot indicators below, centered
- Active dot: 20px wide amber pill with border-radius 99px
- Inactive: 6px circle, parchment at 30% opacity

---

## 11. Section 10 — Newsletter + Footer

### 11.1 Newsletter Section

**Background:** Full-bleed earthy photography + `var(--amber)` overlay at 88% opacity  
**Padding:** 80px vertical

**Layout:** Centered, max-width 680px

- **Eyebrow:** `"JOIN THE GWELLTH COMMUNITY"` — Space Mono 10px uppercase, white 70%
- **Heading:** `"Get Weekly Wellness Tips, Recipes & Exclusive Offers"` — Playfair Display 700 italic, 44px, white
- **Sub:** `"Join 1,000+ subscribers who start their week with GWellth."` — DM Sans 300, 16px, white 70%

**Email form:**
```css
.email-form { display: flex; border-radius: 8px; overflow: hidden; }

.email-input {
  flex: 1;
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-right: none;
  color: white;
  padding: 16px 24px;
  font-family: 'DM Sans', sans-serif;
}

.email-input::placeholder { color: rgba(255, 255, 255, 0.5); }

.email-submit {
  background: var(--deep-earth);
  color: white;
  padding: 16px 28px;
  font-family: 'DM Sans', sans-serif;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: all 300ms ease;
}

.email-submit:hover { background: rgba(28, 18, 8, 0.9); box-shadow: inset 0 0 0 1px rgba(249,242,229,0.2); }
```

On submit: button morphs to checkmark with scale-in animation (scale 0.8 → 1, 400ms spring).

**Micro-copy below:** `"🔒 No spam. Unsubscribe anytime. Max 2 emails/week."` — DM Sans 300, 12px, white 50%

---

### 11.2 Footer

**Background:** `var(--deep-earth)`  
**Top border:** `1px solid rgba(249, 242, 229, 0.08)`  
**Padding:** 80px top, 40px bottom

**Decorative watermark:** Large "G" letterform — Playfair Display 900, 320px, `rgba(249,242,229,0.02)`, positioned top-right

**Top zone — 4-column grid:**

| Column | Content |
|---|---|
| Brand (wider) | GWellth logo + 2-line brand statement + social icons (Facebook, YouTube, Instagram, LinkedIn) in 36px circle buttons |
| Quick Links | Home · About Us · Shop · Blog · Contact. DM Sans 400 14px parchment 65% |
| Our Products | HimGur Jaggery · HimMillets · Prebiotic Honey · South Indian Condiments. Each with colored dot |
| Support | Terms · Privacy Policy · Shipping Policy · Returns · FAQ + certification badge |

**Link hover state:** amber color + `translateX(4px)` — 200ms ease

**Social icon hover:** border → amber, icon fill → amber — 200ms

**Divider:** `1px solid rgba(249, 242, 229, 0.07)` full-width

**Bottom zone** (flex, space-between):
- Left: `"© 2025 GWellth Foods and Beverages Pvt. Ltd."` — Space Mono 10px, parchment 35%
- Center: Payment logos (Visa, Mastercard, UPI, Razorpay) at 40% opacity
- Right: `"Made with 🍯 for India"` — DM Sans 400 13px, parchment 35%

---

## 12. Interactions Master Table

| Trigger | Element | Response | Timing |
|---|---|---|---|
| Page Load | Hero content | Staggered slide-up reveal (8 elements, 150ms apart) | 600–1000ms / spring |
| Scroll 80px | Navbar | Frosted glass dark background + amber border appears | 400ms / ease |
| Scroll into view | Category cards | Staggered rise from 40px, 120ms between cards | 700ms / ease-out |
| Scroll into view | Benefit cards | Row-by-row staggered rise | 700ms / ease-out |
| Scroll into view | Stat numbers | Count up from 0 to final value | 1500ms / ease-out |
| Hover | Nav link | Amber dot appears below, full opacity | 200ms / ease |
| Hover | Category card | Image scale + deeper overlay + reveal description + amber border | 400–600ms / ease |
| Hover | Product card | Card lifts `translateY(-8px)` + image scale + Quick View overlay | 400ms / ease |
| Hover | Add to Cart | Expands from 36px icon to 140px text button | 300ms / spring |
| Click | Wishlist heart | Fills amber + scale pulse + "+1" floats up | 300ms / spring |
| Click | Tab button | Pill slides to tab + old content exits left + new enters right | 300ms / ease |
| Click | Hamburger | Lines animate to X + drawer slides from left | 300ms / ease |
| Click | Newsletter submit | Button morphs to checkmark with scale-in | 400ms / spring |
| Hover | Marquee strip | `animation-play-state: paused` | instant |
| Auto | Testimonials carousel | Advances 1 card every 4.5s, pauses on hover | 600ms / ease |
| Scroll | Brand story image | Parallax: image at 0.4×, stat cards at 0.2× scroll speed | real-time |
| Mouse move | Custom cursor | Main dot instant, follower ring 80ms lag ease-out | 80ms lag |

---

## 13. The Complete Copy-Paste Build Prompt

> Copy everything between the horizontal rules below and paste into Claude, v0, Cursor, Lovable, or any agentic coding tool.

---

```
Build a complete, single-file HTML/CSS/JavaScript homepage for GWellth Foods — an Indian natural food D2C brand selling prebiotic honey, Himalayan jaggery (HimGur), millets (HimMillets), and South Indian condiments. The aesthetic is "Dark Luxury Natural" — premium artisan apothecary meets Himalayan mountain retreat.

--- DESIGN SYSTEM ---

Use this exact color palette as CSS variables:
--deep-earth: #1C1208
--amber: #C47A2A
--honey-gold: #F2B84B
--forest: #2F5930
--sage: #6B8F6B
--parchment: #F9F2E5
--cream: #FDF6EC
--rust: #8B2E0F
--ink: #1A1209
--muted: #8A7A65
--border: #E8DDD0

Import these Google Fonts:
- Playfair Display (400, 400 italic, 700, 700 italic, 900, 900 italic) — all display/heading text
- DM Sans (300, 400, 500, 600) — all body copy and UI
- Space Mono (400, 700) — all labels, tags, price data, monospace elements

Never use Arial, Inter, Roboto, or any system font. Apply a subtle SVG grain texture as a fixed pseudo-element on body with opacity 0.025 and mix-blend-mode: overlay. Use ambient radial-gradient orbs (blur 80px) in amber and forest colors in all dark sections.

--- 10 SECTIONS, TOP TO BOTTOM ---

1. STICKY NAVBAR (72px fixed)
Transparent over hero. After 80px scroll: background rgba(28,18,8,0.95) + backdrop-filter:blur(20px) + bottom border 1px solid rgba(242,184,75,0.12). Transition 400ms ease. Above the navbar: 36px forest-green announcement bar with infinite CSS marquee: "🌿 Free Shipping Above ₹499 · 100% Natural Products · Sourced from Uttarakhand · Ships to 35+ Countries". Announcement bar slides up on first scroll. Logo left: Playfair Display 700, "G" parchment, "Wellth" honey-gold italic, leaf SVG icon beside it. Center links: Home · Shop · Our Story · Benefits · Blog — DM Sans 500 14px parchment, amber dot beneath on hover. Right: search icon + wishlist + amber cart pill button (shows item count, scale 1.04 on hover). Mobile: hamburger to X animation (300ms), full-height left drawer (85vw, deep-earth), nav links in Playfair 28px with 60ms stagger.

2. HERO (min-height: 100vh)
Background layers: (1) base #1C1208, (2) radial-gradient amber orb top-right at 22% opacity, (3) radial-gradient forest orb bottom-left at 15% opacity, (4) product photography right-side (width 55%) fading left via mask-image linear-gradient, (5) grain noise overlay 3% opacity mix-blend-mode overlay. Left column text stack: eyebrow pill badge (Space Mono 10px amber, rgba border) → H1 three lines each on separate spans (Line1: "Nature's Most" parchment 900, Line2: "Powerful Gut" honey-gold 900 italic, Line3: "Foods." parchment 25% opacity 900) → DM Sans 300 18px subheadline rgba(249,242,229,0.6) → two CTA buttons (primary amber, secondary transparent border) → social proof row (⭐4.8/5 · 1,000+ Customers · 35+ Countries, numbers in honey-gold). ALL text elements start opacity:0 and animate in on page load with staggered delays using cubic-bezier(0.16,1,0.3,1): badge 0ms/600ms, H1 line1 150ms/800ms, H1 line2 260ms/800ms, H1 line3 370ms/800ms, subheadline 500ms/700ms, CTAs 650ms/600ms, social proof 800ms/500ms, product image 200ms/1000ms (from translateX 60px + scale 1.04). Product image has a gentle infinite float animation (translateY -10px to +10px, 4s ease-in-out alternate). Scroll indicator at bottom-center: 2px × 48px amber line pulsing opacity 1→0.3→1 on 1.8s loop + "SCROLL" Space Mono 9px below. Hides after 100px scroll.

3. STATS MARQUEE (64px, amber background)
Two rows stacked. Row 1 scrolls left 28s linear infinite. Row 2 scrolls right 38s linear reverse. Content: "100% Natural Products ◆ 35+ Countries Shipped ◆ 1,000+ Happy Customers ◆ 50+ Premium SKUs ◆ 0 Artificial Additives ◆ 3+ Years of Wellness". Numbers in Playfair Display italic 22px, labels DM Sans uppercase 11px letter-spacing 2px. All white. Separator ◆ at 35% opacity. Duplicate content 3x for seamless loop. Hover: animation-play-state paused.

4. POPULAR CATEGORIES (parchment background, 100px padding)
4-column grid (→ 2-col tablet → 1-col mobile, gap 20px). Cards: aspect-ratio 3/4, border-radius 16px, overflow hidden. Each card: full-card photo + gradient overlay (linear-gradient to top, deep-earth 85% at 0% to transparent at 100%) + per-category amber/rust/forest tint added to gradient + category name Playfair 700 26px white pinned bottom + item count Space Mono 10px white 60% + amber arrow. On hover: photo scales 1.08 (600ms ease), overlay deepens, hidden description text reveals sliding up from translateY 20px (400ms), amber box-shadow inset 0 0 0 2px appears, "Shop Now →" button fades in. Scroll-reveal on IntersectionObserver: each card staggered 120ms, from opacity:0 translateY(40px) to opacity:1 translateY(0), 700ms ease-out.

5. FEATURED PRODUCTS (deep-earth dark section, ambient orbs)
3-column grid (→ 2-col → 1-col). Section header: heading left, "View All →" link right. Cards: cream image zone (product floating in warm background, drop-shadow, no hard edges) with rust discount badge top-left + wishlist heart top-right. Info zone below: product name DM Sans 500 16px parchment + variant pills + strikethrough price Space Mono muted + honey-gold sale price Playfair 700 22px + add-to-cart icon button (36px amber circle). On hover: card translateY(-8px) + box-shadow 0 24px 64px rgba(0,0,0,0.3) + image scale 1.06 + Quick View frosted glass overlay appears (backdrop-filter blur 10px, dark 70% bg, border-radius 99px, "Quick View" white DM Sans 13px) + add-to-cart button expands width from 36px to 140px revealing text (300ms spring). Wishlist heart: click fills amber, scale pulse 1.4→1 (300ms), "+1" text floats up and fades.

6. BRAND STORY (forest background + botanical SVG leaf texture 3% opacity)
Split layout: text left, image right. Image clipped into organic flowing SVG clip-path shape (brushstroke-like). Image filter sepia(10%) saturate(110%). Text: eyebrow "OUR STORY" Space Mono honey-gold, heading "Born from the Heart of the Himalayas" Playfair 700 italic 52px parchment, drop-cap first letter Playfair 900 80px honey-gold float left, 3 paragraphs DM Sans 300 16px rgba(249,242,229,0.7), "Read Our Full Story →" amber link (arrow moves 4px right on hover). Three floating stat cards overlapping the column boundary: backdrop-filter blur 12px, rgba(249,242,229,0.08) bg, amber border, stat number Playfair 700 28px honey-gold + label Space Mono 10px white 60%. Parallax: background image moves at 0.4x scroll speed, stat cards at 0.2x speed.

7. HEALTH BENEFITS (parchment section, 100px padding)
3x2 grid of benefit cards. Standard cards: white bg, border-radius 20px, 1px border. Each: color-coded icon box 72px (border-radius 16px, soft tint background + SVG icon 36px in accent color) + Playfair 700 22px benefit name + DM Sans 300 14px description + "Explore →" amber link. Center card row 1 is HERO CARD: deep-earth background, honey-gold name text, parchment 65% description. All cards reveal on scroll (IntersectionObserver threshold 0.15): row 1 first, row 2 second, each card 120ms staggered within row, from opacity:0 translateY(50px) to opacity:1 translateY(0), 700ms ease-out.

8. TABBED COLLECTION (cream background, 100px padding)
Centered heading. Below: pill-shaped tab bar (inline-flex, rgba(28,18,8,0.08) bg, border-radius 99px, 6px padding). 4 tabs: 🍯 Prebiotic Honey · 🏔 HimGur Jaggery · 🌾 HimMillets · 🌶 South Indian. DM Sans 500 14px, padding 12px 28px each. Active state: deep-earth pill indicator slides between tabs (NOT a fade — the pill translates, positioned absolutely, 300ms cubic-bezier(0.4,0,0.2,1)). Active tab text: white. Each tab panel: horizontal scrollable row of product cards (3 visible + 4th peek, gap 20px). Tab switch: old content fades + translates 20px in exit direction (200ms), new content enters from opposite side translateX(30px)→0 + opacity 0→1 (300ms). Dot row below indicates position.

9. TESTIMONIALS (deep-earth dark section, grain overlay)
Centered header with star rating. 3-card depth carousel: center card scale 1.0 opacity 1.0, adjacent cards scale 0.93 opacity 0.5. Cards: rgba(249,242,229,0.06) bg, border-radius 20px, 36px padding. Large opening quote mark (Playfair 900 80px, rgba(196,122,42,0.2), absolute top-left). Review text DM Sans 300 16px parchment 80% italic. Reviewer row: 40px avatar + DM Sans 600 name + Space Mono 10px amber location + 5 amber stars. Verified badge bottom: Space Mono 10px, forest bg, white, border-radius 99px. Auto-advances every 4.5s (600ms ease transition), pauses on hover. ←/→ arrow buttons (48px circles, amber border on hover). Dot indicator below (active: 20px amber pill, inactive: 6px circle 30% opacity).

10. NEWSLETTER + FOOTER
Newsletter: amber-toned section (nature photo + var(--amber) overlay 88%). Centered form: eyebrow + Playfair 700 italic 44px white heading + DM Sans 300 sub. Inline email input (rgba white 15% bg, white text, border white 25%) + submit button (deep-earth bg, white text). Submit morphs to checkmark (scale-in 400ms spring). Trust micro-copy below. Footer: deep-earth, 4-column grid: (1) logo + brand statement + social icon circles (hover: amber border + fill), (2) Quick Links DM Sans 14px parchment 65% + translateX 4px on hover, (3) Our Products with colored category dots, (4) Support links + certification badge. Divider line. Bottom bar: copyright Space Mono 10px parchment 35% | payment logos 40% opacity | "Made with 🍯 for India" DM Sans 13px parchment 35%. Giant faint watermark "G" Playfair 900 320px rgba(249,242,229,0.02) top-right of footer.

--- GLOBAL INTERACTIONS ---

Custom cursor (desktop): 10px amber dot (instant follow, mix-blend-mode difference) + 36px follower ring (1.5px amber border, 80ms ease-out lag). On link/button hover: ring scales to 60px fills amber 10%. On product image hover: ring scales to 72px shows "VIEW" Space Mono 9px white centered.

All scroll-triggered reveals use IntersectionObserver. Stat numbers count up from 0 when in view (1500ms ease-out). All CTAs and buttons: translateY(-2px) + shadow on hover. All text links with arrows: arrow translateX(4px) on hover. Smooth, intentional transitions throughout — nothing jarring.

--- TECHNICAL REQUIREMENTS ---

Single self-contained HTML file. CSS in <style> tag in <head>. JavaScript in <script> tag before </body>. Use placeholder images from picsum.photos with warm/earthy seeds (e.g., picsum.photos/seed/honey/800/600). Fully responsive: mobile-first, hamburger nav on mobile, stacked layouts below 768px, touch swipe on carousels, mobile bottom nav bar (Home/Shop/Cart/Account icons). Target sub-3 second load. No external JavaScript libraries except for fonts. Production-quality semantic HTML. WCAG AA accessible color contrast on all text. No generic AI aesthetics — this should look like a premium, bespoke brand website.
```

---

*End of GWellth Homepage Rebuild Specification — v1.0*