# Design: Kornberg Granit Redesign

**Datum:** 2026-05-24
**Typ:** Visuelles Redesign — index.html + style.css + Anpassung der Legal-Seiten

---

## Rationale

Die ursprüngliche Seite war funktional aber zu neutral. Das Redesign verleiht der Seite mehr Persönlichkeit: ein animierter Partikel-Hintergrund über der gesamten Seite schafft Bewegung und Lebendigkeit, eine stärkere Typografie-Hierarchie (drei Schriften) gibt dem Markenauftritt Eigencharakter.

---

## Entscheidungen aus dem Brainstorming

| Aspekt | Entscheidung |
|---|---|
| Header/Navbar | Entfernt aus `index.html` |
| Hintergrund | Canvas-Partikel-Animation (volle Seite, fixed) |
| Bild | Nur `Blockvermessung.jpg`, mittig, 16:10, Shadow |
| Galerie | Entfällt — kein Dreier-Raster mehr |
| Hero-Eyebrow | „Naturstein aus dem Fichtelgebirge" |
| Hero-H1 | „Kornberg Granit" — DM Serif Display, sehr groß |
| Kontakt-Heading | „Sprechen Sie uns an." — Cormorant Garamond Italic |
| Kontakt-Email | Inter 300, getracked, muted, darunter |
| Tagline | Entfernt |
| Legal-Seiten | Behalten minimale Navigation (Firmenname als Link zurück) |

---

## Typografie

| Element | Schrift | Gewicht | Besonderheit |
|---|---|---|---|
| H1 „Kornberg Granit" | DM Serif Display | 400 | `clamp(5rem, 13vw, 9rem)`, `letter-spacing: -0.02em` |
| H2 „Sprechen Sie uns an." | Cormorant Garamond | 300 italic | `clamp(2.5rem, 6vw, 4.5rem)` |
| Eyebrow | Inter | 300 | `0.6875rem`, `letter-spacing: 0.3em`, uppercase |
| E-Mail-Link | Inter | 300 | `clamp(0.875rem, 2vw, 1.125rem)`, `letter-spacing: 0.1em` |
| Footer | Inter | 300 | `0.625rem`, `letter-spacing: 0.1em` |

Google Fonts Import: `DM+Serif+Display:ital@0;1` + `Cormorant+Garamond:ital,wght@0,300;0,500;1,300;1,500` + `Inter:wght@200;300;400;600`

---

## Farbpalette

| Variable | Wert | Verwendung |
|---|---|---|
| `--white` | `#ffffff` | Hintergrund |
| `--ink` | `#0f0f0f` | Primärtext, Überschriften |
| `--mid` | `#888888` | Eyebrow, E-Mail normal |
| `--faint` | `#c8c8c8` | Footer, Trennlinie, E-Mail hover-border |
| `--border` | `rgba(0,0,0,0.06)` | Footer-Linie |

Keine Akzentfarbe.

---

## Partikel-Animation

- **Canvas** `#particle-canvas`: `position: fixed`, `inset: 0`, `z-index: 0`, `pointer-events: none`
- **Partikel**: 65 Stück, Geschwindigkeit 0.35, Radius 0.6–1.8px
- **Linien**: zwischen Partikeln < 155px Abstand, `rgba(180,172,160, α)` mit α = `(1 - dist/MAX_DIST) * 0.28`
- **Punkte**: `rgba(170,162,150, 0.5)`
- **Canvas-Größe**: `window.innerWidth` × `max(window.innerHeight, document.body.scrollHeight)` — deckt auch gescrollte Bereiche ab
- **Inline JS** am Ende von `index.html` (kein separates File nötig)

---

## Seitenstruktur index.html

```
<canvas id="particle-canvas"> (fixed, hinter allem)

<main>
  <section.hero>
    <p.hero__eyebrow>  Naturstein aus dem Fichtelgebirge
    <h1.hero__h1>      Kornberg / Granit

  <div.hero-image>
    <img src="Blockvermessung.jpg"  16:10  max 720px  shadow>

  <section.contact>
    <h2.contact__heading>  Sprechen Sie uns an.
    <div.contact__divider> (32px Linie)
    <a.email-link>         info@kornberg-granit.de  (JS-injiziert)

<footer.site-footer>
  © Jahr  |  Impressum · Datenschutz
```

Kein `<header>` auf `index.html`.

---

## Legal-Seiten (impressum.html, datenschutz.html)

- Behalten ihren `<header>` als minimale Navigation zurück zur Startseite
- Kein Partikel-Hintergrund (würde Lesbarkeit der langen Texte beeinträchtigen)
- Typografie bleibt wie bisher (Inter, `.legal`-Styles)
- Farbpalette wird auf die neuen CSS-Variablen umgestellt

---

## Geänderte Dateien

| Datei | Art der Änderung |
|---|---|
| `style.css` | Neue Variablen, neue Komponenten (hero, hero-image, contact, particle-canvas), Galerie-Styles entfernen |
| `index.html` | Header entfernen, neue Sektionsstruktur, Partikel-JS inline |
| `impressum.html` | CSS-Variablen aktualisieren (nur Farbwerte) |
| `datenschutz.html` | CSS-Variablen aktualisieren (nur Farbwerte) |
