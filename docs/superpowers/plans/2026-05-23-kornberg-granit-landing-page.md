# Kornberg Granit Landing Page — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a static, GitHub-Pages-compatible 4-file landing page (index, impressum, datenschutz, style.css) serving as a minimal digital business card for Kornberg Granit.

**Architecture:** Pure HTML5/CSS3/Vanilla JS — no build step, no dependencies beyond Google Fonts. All four files share one stylesheet. Email address is never present as cleartext in any HTML file; it is Base64-decoded at runtime by an inline script.

**Tech Stack:** HTML5, CSS3 (custom properties, CSS Grid, flexbox), Vanilla JS (ES5-compatible for broad support), Google Fonts (Inter).

---

## File Map

| Action | File | Responsibility |
|--------|------|----------------|
| Create | `style.css` | All shared styles: reset, variables, typography, layout, components, responsive |
| Create | `index.html` | Hero, image gallery, email CTA (JS-injected), footer |
| Create | `impressum.html` | § 5 DDG legal notice with `[HIER EINTRAGEN]` placeholders |
| Create | `datenschutz.html` | DSGVO privacy policy for static site without cookies |

Images already in repo (used as-is):
- `kornberg_hell.jpg` — gallery slot 1
- `kornberg_gelb.jpg` — gallery slot 2
- `Blockvermessung.jpg` — gallery slot 3

---

## Task 1: style.css — Shared Stylesheet

**Files:**
- Create: `style.css`

- [ ] **Step 1: Create style.css with reset, custom properties, and base typography**

```css
/* style.css */
/* =========================================================
   Kornberg Granit — Shared Stylesheet
   ========================================================= */

/* --- Reset & Box Model --- */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* --- Custom Properties --- */
:root {
  --color-bg:      #f5f3f0;
  --color-text:    #2a2a2a;
  --color-muted:   #7a7a7a;
  --color-border:  #d8d4cf;
  --color-link:    #2a2a2a;
  --font-family:   'Inter', system-ui, -apple-system, sans-serif;
  --max-width:     900px;
  --spacing-xs:    0.5rem;
  --spacing-sm:    1rem;
  --spacing-md:    2rem;
  --spacing-lg:    4rem;
}

/* --- Base --- */
html {
  font-size: 16px;
  scroll-behavior: smooth;
}

body {
  background-color: var(--color-bg);
  color: var(--color-text);
  font-family: var(--font-family);
  font-weight: 400;
  line-height: 1.6;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* --- Typography --- */
h1 {
  font-size: clamp(2rem, 6vw, 3.5rem);
  font-weight: 300;
  letter-spacing: 0.04em;
  line-height: 1.15;
}

h2 {
  font-size: clamp(1.25rem, 3vw, 1.75rem);
  font-weight: 400;
  letter-spacing: 0.02em;
}

p {
  color: var(--color-muted);
  font-size: 1rem;
  font-weight: 300;
}

a {
  color: var(--color-link);
  text-decoration: underline;
  text-underline-offset: 3px;
}

a:hover,
a:focus-visible {
  opacity: 0.7;
  outline: none;
}

a:focus-visible {
  outline: 2px solid var(--color-text);
  outline-offset: 3px;
  border-radius: 2px;
}

/* --- Layout --- */
.container {
  width: 100%;
  max-width: var(--max-width);
  margin-inline: auto;
  padding-inline: var(--spacing-sm);
}

main {
  flex: 1;
}

/* --- Header --- */
.site-header {
  border-bottom: 1px solid var(--color-border);
  padding-block: var(--spacing-sm);
}

.site-header .container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.site-header__name {
  font-size: 0.875rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-decoration: none;
  color: var(--color-text);
}

.site-header__name:hover {
  opacity: 0.7;
}

/* --- Hero Section --- */
.hero {
  padding-block: var(--spacing-lg);
  text-align: center;
}

.hero__tagline {
  margin-top: var(--spacing-sm);
  font-size: 1.125rem;
}

/* --- Gallery Section --- */
.gallery {
  padding-block: var(--spacing-md);
}

.gallery__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--spacing-xs);
}

.gallery__grid img {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  display: block;
}

@media (max-width: 600px) {
  .gallery__grid {
    grid-template-columns: 1fr;
  }
}

/* --- Contact Section --- */
.contact {
  padding-block: var(--spacing-lg);
  text-align: center;
  border-top: 1px solid var(--color-border);
}

.contact__label {
  font-size: 0.8125rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  margin-bottom: var(--spacing-sm);
}

.email-link {
  font-size: clamp(1rem, 2.5vw, 1.375rem);
  font-weight: 400;
  color: var(--color-text);
  text-decoration: none;
  border-bottom: 1px solid var(--color-border);
  padding-bottom: 2px;
  transition: border-color 0.2s ease;
}

.email-link:hover,
.email-link:focus-visible {
  opacity: 1;
  border-color: var(--color-text);
}

.noscript-email {
  font-size: 1rem;
  color: var(--color-muted);
}

/* --- Honeypot (invisible to humans, visible to bots) --- */
.hp-trap {
  display: none !important;
  visibility: hidden;
}

/* --- Footer --- */
.site-footer {
  border-top: 1px solid var(--color-border);
  padding-block: var(--spacing-md);
  margin-top: auto;
}

.site-footer .container {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: var(--spacing-sm);
  font-size: 0.8125rem;
  color: var(--color-muted);
}

.site-footer nav {
  display: flex;
  gap: var(--spacing-md);
}

.site-footer a {
  color: var(--color-muted);
  text-decoration: none;
  font-size: 0.8125rem;
}

.site-footer a:hover {
  color: var(--color-text);
  opacity: 1;
}

/* --- Legal Pages (impressum, datenschutz) --- */
.legal {
  padding-block: var(--spacing-lg);
}

.legal h1 {
  font-size: clamp(1.5rem, 4vw, 2.25rem);
  font-weight: 400;
  margin-bottom: var(--spacing-md);
}

.legal h2 {
  font-size: 1.0625rem;
  font-weight: 600;
  margin-top: var(--spacing-md);
  margin-bottom: var(--spacing-xs);
  letter-spacing: 0.01em;
}

.legal p,
.legal li {
  color: var(--color-text);
  font-size: 0.9375rem;
  font-weight: 400;
  line-height: 1.75;
  max-width: 65ch;
}

.legal ul {
  list-style: disc;
  padding-left: 1.25rem;
}

.legal li + li {
  margin-top: 0.25rem;
}

.legal p + p {
  margin-top: var(--spacing-xs);
}

.legal__placeholder {
  background: #fff3cd;
  border: 1px solid #ffc107;
  border-radius: 3px;
  padding: 0 4px;
  font-style: italic;
}

.legal__notice {
  margin-top: var(--spacing-md);
  padding: var(--spacing-sm);
  border-left: 3px solid var(--color-border);
  font-size: 0.875rem;
  color: var(--color-muted);
}
```

- [ ] **Step 2: Commit**

```bash
git add style.css
git commit -m "feat: add shared stylesheet for Kornberg Granit landing page"
```

---

## Task 2: index.html — Main Landing Page

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html with full page structure**

Note: The `<div id="email-container">` is intentionally empty — the email link is injected by the inline script in Task 3. The `<noscript>` block provides a fallback.

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Kornberg Granit – Kontaktanfragen per E-Mail.">
  <title>Kornberg Granit</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <header class="site-header" role="banner">
    <div class="container">
      <a href="index.html" class="site-header__name" aria-label="Kornberg Granit – Startseite">Kornberg Granit</a>
    </div>
  </header>

  <main id="main-content">

    <!-- Hero -->
    <section class="hero" aria-labelledby="hero-heading">
      <div class="container">
        <h1 id="hero-heading">Kornberg Granit</h1>
        <p class="hero__tagline">Kontaktanfragen bitte per E-Mail.</p>
      </div>
    </section>

    <!-- Image Gallery -->
    <section class="gallery" aria-label="Impressionen">
      <div class="container">
        <div class="gallery__grid">
          <img
            src="kornberg_hell.jpg"
            alt="Heller Kornberg-Granit – gleichmäßige helle Körnung"
            width="600"
            height="450"
            loading="lazy">
          <img
            src="kornberg_gelb.jpg"
            alt="Gelblicher Kornberg-Granit – warmer, goldener Farbton"
            width="600"
            height="450"
            loading="lazy">
          <img
            src="Blockvermessung.jpg"
            alt="Granitblock bei der Vermessung im Steinbruch"
            width="600"
            height="450"
            loading="lazy">
        </div>
      </div>
    </section>

    <!-- Contact -->
    <section class="contact" aria-labelledby="contact-heading">
      <div class="container">
        <p class="contact__label" id="contact-heading">Kontakt</p>

        <!-- Email injected here by JavaScript (see inline script below) -->
        <div id="email-container" aria-live="polite"></div>

        <!-- Fallback for users without JavaScript -->
        <noscript>
          <p class="noscript-email">
            info [at] kornberg-granit [punkt] de
          </p>
        </noscript>

        <!-- Honeypot: only visible to bots, ignored by real users -->
        <span class="hp-trap" aria-hidden="true" tabindex="-1">
          do-not-contact@example.invalid
        </span>

      </div>
    </section>

  </main>

  <footer class="site-footer" role="contentinfo">
    <div class="container">
      <span>&copy; <span id="footer-year"></span> Kornberg Granit</span>
      <nav aria-label="Rechtliches">
        <a href="impressum.html">Impressum</a>
        <a href="datenschutz.html">Datenschutz</a>
      </nav>
    </div>
  </footer>

  <script>
    // Email spam protection:
    // The address is Base64-encoded. It is never present as cleartext in this file.
    // Encoded value: btoa("info@kornberg-granit.de")
    (function () {
      var encoded = 'aW5mb0Brb3JuYmVyZy1ncmFuaXQuZGU=';
      var email = atob(encoded);

      var container = document.getElementById('email-container');
      if (container) {
        var link = document.createElement('a');
        link.href = 'mailto:' + email;
        link.textContent = email;
        link.className = 'email-link';
        link.setAttribute('aria-label', 'E-Mail an Kornberg Granit senden');
        container.appendChild(link);
      }

      // Dynamic copyright year
      var yearEl = document.getElementById('footer-year');
      if (yearEl) {
        yearEl.textContent = new Date().getFullYear();
      }
    }());
  </script>

</body>
</html>
```

- [ ] **Step 2: Open index.html in browser and verify**

Open `index.html` directly in a browser (file:// is fine for this check).

Expected:
- "Kornberg Granit" appears as large heading
- Three images display in a 3-column grid (on desktop)
- Email address `info@kornberg-granit.de` appears as a clickable link in the contact section
- Footer shows current year and links to Impressum / Datenschutz
- Right-click → View Page Source: confirm `info@kornberg-granit.de` does NOT appear anywhere in the HTML source

- [ ] **Step 3: Verify noscript fallback (optional manual check)**

In browser DevTools → Network tab → disable JavaScript, reload.  
Expected: `[at]` / `[punkt]` text appears instead of email link.  
Re-enable JavaScript before continuing.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add index.html with gallery, JS-protected email, and footer"
```

---

## Task 3: impressum.html — Legal Notice

**Files:**
- Create: `impressum.html`

- [ ] **Step 1: Create impressum.html**

```html
<!DOCTYPE html>
<!--
  HINWEIS: Diese Seite ist eine Vorlage.
  Der Inhalt muss vor der Veröffentlichung von einem Rechtsanwalt
  oder einer sachkundigen Person geprüft werden. Der Betreiber
  der Website ist für die Vollständigkeit und Richtigkeit der
  Angaben selbst verantwortlich.
-->
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="robots" content="noindex">
  <title>Impressum – Kornberg Granit</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <header class="site-header" role="banner">
    <div class="container">
      <a href="index.html" class="site-header__name" aria-label="Kornberg Granit – Startseite">Kornberg Granit</a>
    </div>
  </header>

  <main id="main-content">
    <div class="container">
      <article class="legal" aria-labelledby="impressum-heading">

        <h1 id="impressum-heading">Impressum</h1>

        <p>Angaben gemäß § 5 DDG (Digitale-Dienste-Gesetz)</p>

        <h2>Anbieter</h2>
        <p>
          <span class="legal__placeholder">[HIER EINTRAGEN: Name oder Firma]</span><br>
          <span class="legal__placeholder">[HIER EINTRAGEN: Straße und Hausnummer]</span><br>
          <span class="legal__placeholder">[HIER EINTRAGEN: PLZ und Ort]</span><br>
          Deutschland
        </p>

        <h2>Vertretungsberechtigte Person</h2>
        <p>
          <span class="legal__placeholder">[HIER EINTRAGEN: Vor- und Nachname der vertretungsberechtigten Person]</span>
        </p>

        <h2>Kontakt</h2>
        <p>
          E-Mail: <span id="impressum-email-container" aria-live="polite"></span>
          <noscript>info [at] kornberg-granit [punkt] de</noscript>
        </p>

        <h2>Umsatzsteuer-Identifikationsnummer</h2>
        <p>
          <span class="legal__placeholder">[HIER EINTRAGEN: USt-IdNr. gemäß § 27 a UStG – sofern vorhanden, ansonsten diesen Abschnitt entfernen]</span>
        </p>

        <h2>Aufsichtsbehörde</h2>
        <p>
          <span class="legal__placeholder">[HIER EINTRAGEN: zuständige Aufsichtsbehörde – sofern zutreffend, ansonsten diesen Abschnitt entfernen]</span>
        </p>

        <h2>Verantwortlich für den Inhalt nach § 18 Abs. 2 MStV</h2>
        <p>
          <span class="legal__placeholder">[HIER EINTRAGEN: Vor- und Nachname sowie vollständige Anschrift der verantwortlichen Person]</span>
        </p>

        <h2>EU-Streitschlichtung</h2>
        <p>
          Die Europäische Kommission stellt eine Plattform zur Online-Streitbeilegung (OS) bereit:
          <a href="https://ec.europa.eu/consumers/odr" target="_blank" rel="noopener noreferrer">
            https://ec.europa.eu/consumers/odr
          </a>.
        </p>
        <p>
          Unsere E-Mail-Adresse finden Sie oben im Impressum.
        </p>

        <h2>Verbraucherstreitbeilegung</h2>
        <p>
          Wir sind nicht bereit oder verpflichtet, an Streitbeilegungsverfahren
          vor einer Verbraucherschlichtungsstelle teilzunehmen.
        </p>

        <div class="legal__notice" role="note">
          <strong>Hinweis:</strong> Diese Seite ist eine Vorlage und muss vor der
          Veröffentlichung rechtlich geprüft werden.
        </div>

      </article>
    </div>
  </main>

  <footer class="site-footer" role="contentinfo">
    <div class="container">
      <span>&copy; <span id="footer-year"></span> Kornberg Granit</span>
      <nav aria-label="Rechtliches">
        <a href="impressum.html" aria-current="page">Impressum</a>
        <a href="datenschutz.html">Datenschutz</a>
      </nav>
    </div>
  </footer>

  <script>
    (function () {
      var encoded = 'aW5mb0Brb3JuYmVyZy1ncmFuaXQuZGU=';
      var email = atob(encoded);

      var container = document.getElementById('impressum-email-container');
      if (container) {
        var link = document.createElement('a');
        link.href = 'mailto:' + email;
        link.textContent = email;
        link.setAttribute('aria-label', 'E-Mail an Kornberg Granit senden');
        container.appendChild(link);
      }

      var yearEl = document.getElementById('footer-year');
      if (yearEl) {
        yearEl.textContent = new Date().getFullYear();
      }
    }());
  </script>

</body>
</html>
```

- [ ] **Step 2: Open impressum.html in browser and verify**

Expected:
- Page loads with shared header and footer
- All `[HIER EINTRAGEN]` placeholders are highlighted in yellow
- Email address appears as a link (same JS protection as index.html)
- EU-Streitschlichtung link is present and points to `https://ec.europa.eu/consumers/odr`

- [ ] **Step 3: Commit**

```bash
git add impressum.html
git commit -m "feat: add impressum.html with DDG placeholders and spam-protected email"
```

---

## Task 4: datenschutz.html — Privacy Policy

**Files:**
- Create: `datenschutz.html`

- [ ] **Step 1: Create datenschutz.html**

```html
<!DOCTYPE html>
<!--
  HINWEIS: Diese Seite ist eine Vorlage.
  Der Inhalt muss vor der Veröffentlichung von einem Rechtsanwalt
  oder einer sachkundigen Person geprüft werden. Der Betreiber
  der Website ist für die Vollständigkeit und Richtigkeit der
  Angaben selbst verantwortlich.
-->
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="robots" content="noindex">
  <title>Datenschutz – Kornberg Granit</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <header class="site-header" role="banner">
    <div class="container">
      <a href="index.html" class="site-header__name" aria-label="Kornberg Granit – Startseite">Kornberg Granit</a>
    </div>
  </header>

  <main id="main-content">
    <div class="container">
      <article class="legal" aria-labelledby="datenschutz-heading">

        <h1 id="datenschutz-heading">Datenschutzerklärung</h1>

        <p>
          Diese Website setzt keine Cookies und verwendet kein Tracking oder Analytics.
          Es ist daher kein Cookie-Banner erforderlich. Die nachfolgende Erklärung
          informiert Sie über die Verarbeitung personenbezogener Daten im Zusammenhang
          mit dem Betrieb dieser Seite.
        </p>

        <h2>1. Verantwortlicher</h2>
        <p>
          Verantwortlich im Sinne der DSGVO ist:<br><br>
          <span class="legal__placeholder">[HIER EINTRAGEN: Name / Firma]</span><br>
          <span class="legal__placeholder">[HIER EINTRAGEN: Straße, Hausnummer]</span><br>
          <span class="legal__placeholder">[HIER EINTRAGEN: PLZ, Ort]</span><br>
          Deutschland<br><br>
          E-Mail: <span id="datenschutz-email-container" aria-live="polite"></span>
          <noscript>info [at] kornberg-granit [punkt] de</noscript>
        </p>

        <h2>2. Hosting – GitHub Pages</h2>
        <p>
          Diese Website wird über GitHub Pages gehostet, einen Dienst der
          GitHub, Inc., 88 Colin P Kelly Jr St, San Francisco, CA 94107, USA.
          Beim Abruf dieser Website werden durch GitHub automatisch sogenannte
          Server-Logfiles erhoben. Dazu können gehören:
        </p>
        <ul>
          <li>IP-Adresse des anfragenden Rechners (ggf. anonymisiert)</li>
          <li>Datum und Uhrzeit des Zugriffs</li>
          <li>Aufgerufene URL</li>
          <li>Browser-Typ und -Version</li>
          <li>Betriebssystem des anfragenden Rechners</li>
          <li>Referrer-URL (zuvor besuchte Seite)</li>
        </ul>
        <p>
          Diese Daten werden durch GitHub erhoben und verarbeitet. Wir als
          Betreiber haben darauf keinen direkten Einfluss. Rechtsgrundlage
          ist Art. 6 Abs. 1 lit. f DSGVO (berechtigtes Interesse an einem
          sicheren und stabilen Betrieb der Website). Weitere Informationen
          entnehmen Sie der
          <a href="https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement"
             target="_blank" rel="noopener noreferrer">
            Datenschutzerklärung von GitHub
          </a>.
        </p>

        <h2>3. Kontaktaufnahme per E-Mail</h2>
        <p>
          Wenn Sie uns per E-Mail kontaktieren, werden die von Ihnen
          übermittelten Daten (z. B. Name, E-Mail-Adresse, Nachrichteninhalt)
          ausschließlich zur Bearbeitung Ihrer Anfrage gespeichert und
          verwendet. Eine Weitergabe an Dritte erfolgt nicht.
          Rechtsgrundlage ist Art. 6 Abs. 1 lit. b DSGVO (Vertragsanbahnung
          oder -erfüllung) bzw. Art. 6 Abs. 1 lit. f DSGVO (berechtigtes
          Interesse an der Beantwortung von Anfragen). Die Daten werden
          gelöscht, sobald sie für den Zweck ihrer Erhebung nicht mehr
          erforderlich sind.
        </p>

        <h2>4. Keine Cookies, kein Tracking</h2>
        <p>
          Diese Website setzt keine Cookies und verwendet keine
          Tracking-Technologien (z. B. Google Analytics, Matomo o. Ä.).
          Es werden durch uns keine personenbezogenen Daten für
          Werbezwecke verarbeitet. Ein Cookie-Banner ist daher nicht
          erforderlich — und dieser Zustand soll so bleiben.
        </p>

        <h2>5. Betroffenenrechte</h2>
        <p>Sie haben nach der DSGVO folgende Rechte:</p>
        <ul>
          <li><strong>Auskunft</strong> (Art. 15 DSGVO): Sie können Auskunft über die zu Ihrer Person gespeicherten Daten verlangen.</li>
          <li><strong>Berichtigung</strong> (Art. 16 DSGVO): Sie können die Berichtigung unrichtiger Daten verlangen.</li>
          <li><strong>Löschung</strong> (Art. 17 DSGVO): Sie können unter bestimmten Voraussetzungen die Löschung Ihrer Daten verlangen.</li>
          <li><strong>Einschränkung der Verarbeitung</strong> (Art. 18 DSGVO): Sie können die Einschränkung der Verarbeitung Ihrer Daten verlangen.</li>
          <li><strong>Datenübertragbarkeit</strong> (Art. 20 DSGVO): Sie können Ihre Daten in einem gängigen, maschinenlesbaren Format erhalten.</li>
          <li><strong>Widerspruch</strong> (Art. 21 DSGVO): Sie können der Verarbeitung Ihrer Daten auf Basis von Art. 6 Abs. 1 lit. f DSGVO widersprechen.</li>
        </ul>
        <p>
          Zur Ausübung Ihrer Rechte wenden Sie sich bitte per E-Mail an uns
          (Adresse siehe Abschnitt 1).
        </p>

        <h2>6. Beschwerderecht bei der Aufsichtsbehörde</h2>
        <p>
          Sie haben das Recht, sich bei einer Datenschutz-Aufsichtsbehörde
          über die Verarbeitung Ihrer personenbezogenen Daten zu beschweren.
          Die zuständige Aufsichtsbehörde richtet sich nach Ihrem Wohnort
          oder dem Sitz des Verantwortlichen.
        </p>
        <p>
          <span class="legal__placeholder">[HIER EINTRAGEN: ggf. die zuständige Landesbehörde nennen, z. B. Bayerisches Landesamt für Datenschutzaufsicht (BayLDA), www.lda.bayern.de]</span>
        </p>

        <h2>7. Keine automatisierte Entscheidungsfindung</h2>
        <p>
          Es findet keine automatisierte Entscheidungsfindung einschließlich
          Profiling im Sinne von Art. 22 DSGVO statt.
        </p>

        <div class="legal__notice" role="note">
          <strong>Hinweis:</strong> Diese Seite ist eine Vorlage und muss vor der
          Veröffentlichung rechtlich geprüft werden.
        </div>

      </article>
    </div>
  </main>

  <footer class="site-footer" role="contentinfo">
    <div class="container">
      <span>&copy; <span id="footer-year"></span> Kornberg Granit</span>
      <nav aria-label="Rechtliches">
        <a href="impressum.html">Impressum</a>
        <a href="datenschutz.html" aria-current="page">Datenschutz</a>
      </nav>
    </div>
  </footer>

  <script>
    (function () {
      var encoded = 'aW5mb0Brb3JuYmVyZy1ncmFuaXQuZGU=';
      var email = atob(encoded);

      var container = document.getElementById('datenschutz-email-container');
      if (container) {
        var link = document.createElement('a');
        link.href = 'mailto:' + email;
        link.textContent = email;
        link.setAttribute('aria-label', 'E-Mail an Kornberg Granit senden');
        container.appendChild(link);
      }

      var yearEl = document.getElementById('footer-year');
      if (yearEl) {
        yearEl.textContent = new Date().getFullYear();
      }
    }());
  </script>

</body>
</html>
```

- [ ] **Step 2: Open datenschutz.html in browser and verify**

Expected:
- Alle 7 Abschnitte sind lesbar
- `[HIER EINTRAGEN]`-Felder werden gelb hervorgehoben
- E-Mail-Link erscheint korrekt in Abschnitt 1
- GitHub-Datenschutz-Link öffnet sich in neuem Tab

- [ ] **Step 3: Commit**

```bash
git add datenschutz.html
git commit -m "feat: add datenschutz.html with DSGVO privacy policy template"
```

---

## Task 5: Final Verification

**Files:** no new files

- [ ] **Step 1: Cross-page navigation check**

Open `index.html` and click through every link:
- Impressum link in footer → `impressum.html` ✓
- Datenschutz link in footer → `datenschutz.html` ✓
- "Kornberg Granit" in header of impressum/datenschutz → `index.html` ✓

- [ ] **Step 2: Mobile responsive check**

In browser DevTools → Toggle device toolbar → set to 375px width (iPhone SE).

Expected on index.html:
- Gallery shows 1 column (images stacked)
- Text is readable without horizontal scroll
- Email link is tappable (minimum 44px touch target)

- [ ] **Step 3: Source code inspection for email leakage**

In browser: Right-click → View Page Source on all three HTML files.  
Search for `info@kornberg-granit` or `@kornberg`.  
Expected: NOT found in any file. Only the Base64 string `aW5mb0Brb3JuYmVyZy1ncmFuaXQuZGU=` should be present.

- [ ] **Step 4: Final commit**

```bash
git add .
git status
git commit -m "chore: final verification pass – static landing page complete"
```
