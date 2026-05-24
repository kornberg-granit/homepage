# Slider Auto-Advance + 8 Images Design

**Date:** 2026-05-24  
**Status:** Approved

## Summary

Extend the existing hero slider to use 8 images (`1.jpg`–`8.jpg`) and auto-advance every 3 seconds. Manual indicator clicks reset the timer.

## Images

`1.jpg`, `2.jpg`, `3.jpg`, `4.jpg`, `5.jpg`, `6.jpg`, `7.jpg`, `8.jpg` — all in the project root.

- `1.jpg`: `loading="eager" fetchpriority="high"`, `aria-hidden="false"`, active on load
- `2.jpg`–`8.jpg`: `loading="lazy"`, `aria-hidden="true"`

## HTML Changes (`index.html`)

Replace the 3 existing `<img>` slides and 3 `<button>` indicators with 8 of each, following the identical pattern:

```html
<div class="hero-slider" aria-label="Bildergalerie">
  <div class="hero-slider__track">
    <img src="1.jpg" alt="Kornberg Granit – Bild 1"
         class="hero-slider__slide hero-slider__slide--active"
         aria-hidden="false" width="720" height="450"
         loading="eager" fetchpriority="high">
    <img src="2.jpg" alt="Kornberg Granit – Bild 2"
         class="hero-slider__slide" aria-hidden="true"
         width="720" height="450" loading="lazy">
    <!-- … repeat for 3.jpg–8.jpg … -->
  </div>
  <div class="hero-slider__dots" role="group" aria-label="Bildnavigation">
    <button class="hero-slider__dot hero-slider__dot--active"
            aria-label="Bild 1" aria-current="true"></button>
    <button class="hero-slider__dot" aria-label="Bild 2" aria-current="false"></button>
    <!-- … repeat for Bild 3–8 … -->
  </div>
</div>
```

## JS Changes (`index.html` inline script)

Replace the current slider block (inside the IIFE) with:

```js
// ── Image slider ──────────────────────────────────────────────────────
var slides  = document.querySelectorAll('.hero-slider__slide');
var dots    = document.querySelectorAll('.hero-slider__dot');
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

Key points:
- `current` tracks the active index so auto-advance always knows where it is
- `startTimer()` calls `clearInterval` before `setInterval` — safe to call multiple times
- Timer restarts on manual click (prevents immediate jump right after click)
- `slides.length` used in modulo so the count is never hardcoded
