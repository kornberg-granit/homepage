# Image Slider Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the single static hero image on `index.html` with a 3-image ultra-minimal fade slider driven by line indicators.

**Architecture:** CSS handles all animation via `opacity` transitions on absolutely-positioned `<img>` elements. A small vanilla JS closure wires click events on indicator `<button>` elements to toggle the active class. No external libraries.

**Tech Stack:** Vanilla HTML/CSS/JS — no build step, no dependencies.

---

## Files

- Modify: `style.css` — replace `.hero-image` / `.hero-image img` rules with `.hero-slider` family
- Modify: `index.html` — replace `.hero-image` div with `.hero-slider` markup; append slider JS to existing inline script

---

### Task 1: Update CSS

**Files:**
- Modify: `style.css` (lines around the `/* --- Hero Image ---*/` block, currently ~587–599)

- [ ] **Step 1: Replace the `.hero-image` CSS block**

Find and replace the entire `/* --- Hero Image --- */` section in `style.css`:

```css
/* --- Hero Image --- */
.hero-image {
  display: flex;
  justify-content: center;
  padding: 5.5rem 2rem 0;
}

.hero-image img {
  width: min(720px, 88vw);
  aspect-ratio: 16 / 10;
  object-fit: cover;
  display: block;
  box-shadow: 0 20px 80px rgba(0,0,0,0.10);
}
```

…with:

```css
/* --- Hero Slider --- */
.hero-slider {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 5.5rem 2rem 0;
}

.hero-slider__track {
  position: relative;
  width: min(720px, 88vw);
  aspect-ratio: 16 / 10;
  box-shadow: 0 20px 80px rgba(0,0,0,0.10);
}

.hero-slider__slide {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  opacity: 0;
  transition: opacity 0.6s ease;
}

.hero-slider__slide--active {
  opacity: 1;
}

.hero-slider__dots {
  display: flex;
  gap: 8px;
  margin-top: 1.25rem;
}

.hero-slider__dot {
  width: 24px;
  padding: 5px 0;
  background: transparent;
  border: none;
  cursor: pointer;
  position: relative;
}

.hero-slider__dot::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  height: 1px;
  background: var(--faint);
  transition: background 0.3s;
}

.hero-slider__dot--active::after,
.hero-slider__dot:hover::after {
  background: var(--ink);
}
```

- [ ] **Step 2: Commit CSS changes**

```bash
git add style.css
git commit -m "feat: add hero slider CSS"
```

---

### Task 2: Update HTML structure

**Files:**
- Modify: `index.html` (the `.hero-image` div, currently lines 22–30)

- [ ] **Step 1: Replace the `.hero-image` div**

Find:

```html
  <div class="hero-image">
    <img
      src="Blockvermessung.jpg"
      alt="Granitblock bei der Vermessung im Steinbruch"
      width="720"
      height="450"
      loading="eager"
      fetchpriority="high">
  </div>
```

Replace with:

```html
  <div class="hero-slider" aria-label="Bildergalerie">
    <div class="hero-slider__track">
      <img
        src="Blockvermessung.jpg"
        alt="Granitblock bei der Vermessung im Steinbruch"
        class="hero-slider__slide hero-slider__slide--active"
        width="720" height="450"
        loading="eager" fetchpriority="high">
      <img
        src="kornberg_hell.jpg"
        alt="Kornberg Granit – helle Steinoberfläche"
        class="hero-slider__slide"
        width="720" height="450"
        loading="lazy">
      <img
        src="kornberg_gelb.jpg"
        alt="Kornberg Granit – gelbliche Steinoberfläche"
        class="hero-slider__slide"
        width="720" height="450"
        loading="lazy">
    </div>
    <div class="hero-slider__dots" role="tablist" aria-label="Bild wählen">
      <button class="hero-slider__dot hero-slider__dot--active"
              role="tab" aria-selected="true" aria-label="Bild 1" data-index="0"></button>
      <button class="hero-slider__dot"
              role="tab" aria-selected="false" aria-label="Bild 2" data-index="1"></button>
      <button class="hero-slider__dot"
              role="tab" aria-selected="false" aria-label="Bild 3" data-index="2"></button>
    </div>
  </div>
```

- [ ] **Step 2: Commit HTML structure**

```bash
git add index.html
git commit -m "feat: replace hero image with slider markup"
```

---

### Task 3: Add slider JavaScript

**Files:**
- Modify: `index.html` (the inline `<script>` block — append after the `// ── Dynamic copyright year` block, before the `// ── Particle animation` block)

- [ ] **Step 1: Add the slider JS**

Inside the existing `(function () { … }())` IIFE, after the year logic (around line 82), insert:

```js
  // ── Image slider ──────────────────────────────────────────────────────
  var slides = document.querySelectorAll('.hero-slider__slide');
  var dots   = document.querySelectorAll('.hero-slider__dot');
  if (slides.length) {
    function goTo(index) {
      slides.forEach(function (s, i) {
        s.classList.toggle('hero-slider__slide--active', i === index);
      });
      dots.forEach(function (d, i) {
        d.classList.toggle('hero-slider__dot--active', i === index);
        d.setAttribute('aria-selected', i === index ? 'true' : 'false');
      });
    }
    dots.forEach(function (dot, i) {
      dot.addEventListener('click', function () { goTo(i); });
    });
  }
```

- [ ] **Step 2: Open `index.html` in a browser and verify**

- All three line indicators appear below the image
- Clicking indicator 2 fades to `kornberg_hell.jpg`
- Clicking indicator 3 fades to `kornberg_gelb.jpg`
- Clicking indicator 1 returns to `Blockvermessung.jpg`
- Active indicator line is dark (`var(--ink)`), inactive lines are light (`var(--faint)`)
- No console errors

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add slider JS — fade between hero images via line indicators"
```
