# Slider Auto-Advance + 8 Images Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update the hero slider to show 8 numbered images (1.jpg–8.jpg) and auto-advance every 3 seconds, resetting the timer on manual indicator clicks.

**Architecture:** Two changes to `index.html` only — (1) replace the 3 slide images and 3 indicator buttons with 8 each, and (2) replace the current static slider JS block with one that adds `setInterval`/`clearInterval` auto-advance and a `current` index variable.

**Tech Stack:** Vanilla HTML/CSS/JS — no build step, no dependencies.

---

## Files

- Modify: `index.html` — update HTML slider markup (Task 1) and inline JS slider block (Task 2)

---

### Task 1: Replace slider HTML — 8 images and 8 indicators

**Files:**
- Modify: `index.html` (the `.hero-slider` div, currently around lines 22–53)

- [ ] **Step 1: Replace the entire `.hero-slider` div**

Find the existing `.hero-slider` block (from `<div class="hero-slider"` to its closing `</div>`):

```html
  <div class="hero-slider" aria-label="Bildergalerie">
    <div class="hero-slider__track">
      <img
        src="Blockvermessung.jpg"
        alt="Granitblock bei der Vermessung im Steinbruch"
        class="hero-slider__slide hero-slider__slide--active"
        aria-hidden="false"
        width="720" height="450"
        loading="eager" fetchpriority="high">
      <img
        src="kornberg_hell.jpg"
        alt="Kornberg Granit – helle Steinoberfläche"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="kornberg_gelb.jpg"
        alt="Kornberg Granit – gelbliche Steinoberfläche"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
    </div>
    <div class="hero-slider__dots" role="group" aria-label="Bildnavigation">
      <button class="hero-slider__dot hero-slider__dot--active"
              aria-label="Bild 1" aria-current="true"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 2" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 3" aria-current="false"></button>
    </div>
  </div>
```

Replace it with:

```html
  <div class="hero-slider" aria-label="Bildergalerie">
    <div class="hero-slider__track">
      <img
        src="1.jpg"
        alt="Kornberg Granit – Bild 1"
        class="hero-slider__slide hero-slider__slide--active"
        aria-hidden="false"
        width="720" height="450"
        loading="eager" fetchpriority="high">
      <img
        src="2.jpg"
        alt="Kornberg Granit – Bild 2"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="3.jpg"
        alt="Kornberg Granit – Bild 3"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="4.jpg"
        alt="Kornberg Granit – Bild 4"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="5.jpg"
        alt="Kornberg Granit – Bild 5"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="6.jpg"
        alt="Kornberg Granit – Bild 6"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="7.jpg"
        alt="Kornberg Granit – Bild 7"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
      <img
        src="8.jpg"
        alt="Kornberg Granit – Bild 8"
        class="hero-slider__slide"
        aria-hidden="true"
        width="720" height="450"
        loading="lazy">
    </div>
    <div class="hero-slider__dots" role="group" aria-label="Bildnavigation">
      <button class="hero-slider__dot hero-slider__dot--active"
              aria-label="Bild 1" aria-current="true"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 2" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 3" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 4" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 5" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 6" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 7" aria-current="false"></button>
      <button class="hero-slider__dot"
              aria-label="Bild 8" aria-current="false"></button>
    </div>
  </div>
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "feat: update slider HTML — 8 numbered images and indicators"
```

---

### Task 2: Replace slider JS — add auto-advance

**Files:**
- Modify: `index.html` (the `// ── Image slider` block inside the inline IIFE)

- [ ] **Step 1: Replace the slider JS block**

Find the entire slider block inside the IIFE (from `// ── Image slider` to the closing `}`):

```js
  // ── Image slider ──────────────────────────────────────────────────────
  var slides = document.querySelectorAll('.hero-slider__slide');
  var dots   = document.querySelectorAll('.hero-slider__dot');
  if (slides.length) {
    var goTo = function (index) {
      slides.forEach(function (s, i) {
        s.classList.toggle('hero-slider__slide--active', i === index);
        s.setAttribute('aria-hidden', i === index ? 'false' : 'true');
      });
      dots.forEach(function (d, i) {
        d.classList.toggle('hero-slider__dot--active', i === index);
        d.setAttribute('aria-current', i === index ? 'true' : 'false');
      });
    };
    dots.forEach(function (dot, i) {
      dot.addEventListener('click', function () { goTo(i); });
    });
    goTo(0);
  }
```

Replace it with:

```js
  // ── Image slider ──────────────────────────────────────────────────────
  var slides = document.querySelectorAll('.hero-slider__slide');
  var dots   = document.querySelectorAll('.hero-slider__dot');
  if (slides.length) {
    var current = 0;
    var timer;

    var goTo = function (index) {
      current = index;
      slides.forEach(function (s, i) {
        s.classList.toggle('hero-slider__slide--active', i === index);
        s.setAttribute('aria-hidden', i === index ? 'false' : 'true');
      });
      dots.forEach(function (d, i) {
        d.classList.toggle('hero-slider__dot--active', i === index);
        d.setAttribute('aria-current', i === index ? 'true' : 'false');
      });
    };

    var startTimer = function () {
      clearInterval(timer);
      timer = setInterval(function () {
        goTo((current + 1) % slides.length);
      }, 3000);
    };

    dots.forEach(function (dot, i) {
      dot.addEventListener('click', function () {
        goTo(i);
        startTimer();
      });
    });

    goTo(0);
    startTimer();
  }
```

- [ ] **Step 2: Verify the placement is correct**

Confirm the slider block is still:
- AFTER the `// ── Dynamic copyright year` section
- BEFORE the `// ── Particle animation` section
- Still inside the outer `(function () { … }())` IIFE

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add auto-advance to slider — 3 s interval, resets on manual click"
```
