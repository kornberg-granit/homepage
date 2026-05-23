# Design: Kornberg Granit Landing Page

**Datum:** 2026-05-23  
**Typ:** Statische Landing Page / Digitale Visitenkarte  
**Hosting:** GitHub Pages

---

## Rationale

Kornberg Granit benötigt eine minimalistische digitale Visitenkarte ohne Backend,
Formulare oder Tracking. Die Seite soll den Charakter des Unternehmens (Naturstein,
Handwerk, Qualität) visuell transportieren und lediglich die Kontaktaufnahme per
E-Mail ermöglichen — geschützt vor Spam-Bots.

---

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Startseite: Hero, Bildgalerie, E-Mail-CTA, Footer |
| `impressum.html` | Pflichtangaben nach § 5 DDG mit Platzhaltern |
| `datenschutz.html` | DSGVO-Erklärung für statische Seite ohne Cookies |
| `style.css` | Gemeinsame Styles für alle drei Seiten |

Bestehende Bilder im Repo werden verwendet (Reihenfolge im Grid):
1. `kornberg_hell.jpg` (heller Granit)
2. `kornberg_gelb.jpg` (gelblicher Granit)
3. `Blockvermessung.jpg` (Rohblock / Handwerk)

---

## Seitenstruktur (index.html)

Footer-Jahr: dynamisch via `new Date().getFullYear()` (JS), damit die Seite nicht jährlich manuell aktualisiert werden muss.

```
<header>  Kornberg Granit  (kein Navigationsmenü)
<main>
  <section class="hero">
    <h1>Kornberg Granit</h1>
    <p>Kontaktanfragen bitte per E-Mail.</p>
  </section>
  <section class="gallery">
    3-spaltiges CSS-Grid (4:3, object-fit: cover)
    Mobile: 1 Spalte
  </section>
  <section class="contact">
    E-Mail-Link (via JS eingefügt)
    <noscript>-Fallback mit [at]/[punkt]
  </section>
</main>
<footer>  © Kornberg Granit · Impressum · Datenschutz
```

---

## Visuelles Design

**Farbpalette:**
- Hintergrund: `#f5f3f0` (warmes Steinweiß)
- Primärtext: `#2a2a2a` (Anthrazit)
- Sekundärtext / Akzente: `#7a7a7a` (Mittelgrau)
- Trennlinien / Borders: `#d8d4cf` (Steingrau)
- E-Mail-Link Hover: `#2a2a2a` mit Unterstreichung

**Typografie:** Google Fonts „Inter" (weights 300, 400, 600)

**Abstände:** Großzügiger Weißraum (Mobile First), max-width ~900 px zentriert.

**Galerie:** CSS Grid, 3 Spalten `minmax(0, 1fr)`, `aspect-ratio: 4/3`,
`object-fit: cover`. Breakpoint < 600 px → 1 Spalte.

---

## E-Mail Spam-Schutz (mehrschichtig)

1. **Base64-Kodierung im JS:** E-Mail als `atob("aW5mb0Brb3JuYmVyZy1ncmFuaXQuZGU=")` — kein Klartext im HTML-Quellcode
2. **mailto-Link via JS:** `<a>`-Element wird per `document.createElement` erzeugt und erst nach DOMContentLoaded in den DOM eingefügt
3. **noscript-Fallback:** Sichtbarer Hinweistext `info [at] kornberg-granit [punkt] de`
4. **Honeypot:** Unsichtbares `<span aria-hidden="true" style="display:none">` mit gefälschter Adresse für Crawler

---

## Impressum (impressum.html)

Pflichtangaben nach § 5 DDG (ehemals TMG):
- Name / Firma `[HIER EINTRAGEN]`
- Anschrift `[HIER EINTRAGEN]`
- Vertretungsberechtigte Person `[HIER EINTRAGEN]`
- Kontakt (E-Mail spam-geschützt wie oben)
- Umsatzsteuer-ID `[HIER EINTRAGEN — falls vorhanden]`
- Aufsichtsbehörde `[HIER EINTRAGEN — falls zutreffend]`
- Verantwortlich nach § 18 Abs. 2 MStV `[HIER EINTRAGEN]`
- EU-Streitschlichtungshinweis (Link zu ec.europa.eu/consumers/odr) + Erklärung zur Nicht-Teilnahme

HTML-Kommentar oben: *„Vorlage — vor Veröffentlichung rechtlich prüfen lassen."*

---

## Datenschutz (datenschutz.html)

Abschnitte:
1. Verantwortlicher `[HIER EINTRAGEN]`
2. Hosting (GitHub Pages / GitHub Inc., Server-Logfiles)
3. Kontaktaufnahme per E-Mail (keine Weitergabe an Dritte)
4. Betroffenenrechte (Auskunft, Berichtigung, Löschung, Beschwerde bei Aufsichtsbehörde)
5. Keine automatisierte Entscheidungsfindung
6. Hinweis: kein Cookie-Banner nötig (keine Cookies / kein Tracking)

HTML-Kommentar oben: *„Vorlage — vor Veröffentlichung rechtlich prüfen lassen."*

---

## Technische Anforderungen

- Kein Build-Schritt, keine Abhängigkeiten außer optional Google Fonts
- Alle Pfade relativ (GitHub-Pages-kompatibel)
- `lang="de"` auf allen Seiten
- Semantisches HTML5 (`<header>`, `<main>`, `<footer>`, `<nav>`, `<section>`)
- Accessibility: `alt`-Attribute auf allen Bildern, ausreichende Kontraste (WCAG AA), ARIA wo sinnvoll
- Responsive / Mobile First
