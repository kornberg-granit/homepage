# Image Slider Design

**Date:** 2026-05-24  
**Status:** Approved

## Summary

Replace the single static hero image on `index.html` with a minimal fade-based image slider showing three photographs in sequence. No external libraries. No auto-advance.

## Images & Order

1. `Blockvermessung.jpg` — existing hero image
2. `kornberg_hell.jpg` — new
3. `kornberg_gelb.jpg` — new

## Chosen Approach: Ultra-Minimal (Option C)

- Fade crossfade between slides (CSS `opacity` transition, 0.6s ease)
- Horizontal line indicators below the image (not dots — thin 24px × 1px bars)
- Clicking a line indicator switches to that slide
- No auto-advance
- No arrow buttons
- Matches the zero-dependency, editorial aesthetic of the site

## HTML Changes (`index.html`)

Replace `.hero-image` div with:

```html
<div class="hero-slider" aria-label="Bildergalerie">
  <div class="hero-slider__track">
    <img src="Blockvermessung.jpg" alt="Granitblock bei der Vermessung im Steinbruch"
         class="hero-slider__slide hero-slider__slide--active"
         width="720" height="450" loading="eager" fetchpriority="high">
    <img src="kornberg_hell.jpg" alt="Kornberg Granit — helle Variante"
         class="hero-slider__slide"
         width="720" height="450" loading="lazy">
    <img src="kornberg_gelb.jpg" alt="Kornberg Granit — gelbe Variante"
         class="hero-slider__slide"
         width="720" height="450" loading="lazy">
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

## CSS Changes (`style.css`)

Replace `.hero-image` / `.hero-image img` rules with:

```css
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
  height: 1px;
  padding: 5px 0;
  background: transparent;
  border: none;
  cursor: pointer;
  position: relative;
}

.hero-slider__dot::after {
  content: '';
  position: absolute;
  left: 0; right: 0;
  top: 50%; transform: translateY(-50%);
  height: 1px;
  background: var(--faint);
  transition: background 0.3s;
}

.hero-slider__dot--active::after,
.hero-slider__dot:hover::after {
  background: var(--ink);
}
```

## JS Changes (`index.html` inline script)

Add after the email/year section:

```js
// ── Image slider ──────────────────────────────────────────────────────────
(function () {
  var slides = document.querySelectorAll('.hero-slider__slide');
  var dots   = document.querySelectorAll('.hero-slider__dot');
  if (!slides.length) return;

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
}());
```

## Accessibility

- `role="tablist"` / `role="tab"` + `aria-selected` on indicators
- `aria-label` on each slide image
- `aria-label="Bildergalerie"` on the wrapper
- First image keeps `fetchpriority="high"`, others use `loading="lazy"`
