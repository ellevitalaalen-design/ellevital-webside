# ellevital – Website

Statische Website für **ellevital GmbH – Woman in Balance**, Aalen.
Hosting: **IONOS Deploy Now**. Jeder Push auf `main` veröffentlicht automatisch.

Diese Datei ist die Orientierung für Claude Code. Bitte vor Änderungen lesen.

---

## 1 · Wie die Seite funktioniert

Kein Build-Schritt, kein npm, kein Framework-Toolchain. Neun HTML-Dateien, ein Ordner Bilder, eine Runtime-Datei. Datei öffnen, ändern, pushen — fertig.

Die Seiten nutzen eine kleine Rendering-Runtime (`support.js`), die im Browser läuft. Aufbau jeder Seite:

```html
<!DOCTYPE html>
<html lang="de"><head>
  <title>…</title>
  <meta name="description" content="…">
  <script src="support.js"></script>
</head>
<body>
<x-dc>
  <helmet>
    <!-- Google-Fonts-Links, @keyframes, body-Reset -->
  </helmet>
  …Markup…
</x-dc>
<script type="text/x-dc" data-dc-script>
class Component extends DCLogic {
  state = { … }
  renderVals() { return { … } }   // liefert die Werte für {{ … }}
}
</script>
</body></html>
```

**Wichtig zu verstehen:**

- `{{ name }}` im Markup ist ein Platzhalter. Er wird aus dem Rückgabewert von `renderVals()` gefüllt. Nur einfache Pfade (`{{ user.name }}`), **keine Ausdrücke** — Logik gehört in `renderVals()`.
- `<sc-for list="{{ items }}" as="item">` wiederholt seinen Inhalt pro Listeneintrag.
- `<sc-if value="{{ flag }}">` zeigt seinen Inhalt nur, wenn `flag` wahr ist.
- **Styling ausschließlich inline** über `style="…"`. Keine CSS-Klassen, kein Stylesheet. Pseudo-Zustände als `style-hover="…"`, `style-active="…"`, `style-focus="…"`.
- `support.js` bitte nicht bearbeiten. Sie ist die Runtime, nicht Projektcode.

Zum Ansehen genügt ein lokaler Server (`python3 -m http.server`), weil `support.js` per `file://` nicht geladen wird.

---

## 2 · Dateien

| Datei | Seite | Logik |
| --- | --- | --- |
| `index.html` | Startseite | `menuOpen` (Mobilmenü), `openTest` (Selbsttest-Akkordeon) |
| `training.html` | Training / milon-Zirkel | Props `showTrialBand`, `showBreadcrumb` |
| `kurse.html` | Kurse | `filter1/2`, `open1/2`; Kursdaten im Array `COURSES` |
| `medical-wellness.html` | Medical Wellness | `soundOn` (Ambient-Video stumm) |
| `ueber-uns.html` | Über uns | keine — reines Markup |
| `shop.html` | Shop | `menuOpen`, `activeCategory`; Produkte im Array in `renderVals()` |
| `impressum.html` | Impressum | keine |
| `datenschutz.html` | Datenschutzerklärung | keine |
| `404.html` | Fehlerseite | keine (reines HTML ohne Runtime) |

Dazu `assets/` (26 Bilder), `support.js`, `robots.txt`, `sitemap.xml`.

**Dateinamen nicht umbenennen** — sie stehen in `sitemap.xml`, in den Canonical-Tags und in der Navigation jeder Seite.

---

## 3 · Wo welcher Inhalt liegt

**Texte** stehen im Klartext im Markup. Gesuchten Satz einfach über alle Dateien suchen.

**Kursliste** (`kurse.html`): im Array `COURSES` in der Logikklasse. Ein Eintrag:

```js
{ id:'bodymind', cat:'Entspannung', name:'Body & Mind',
  tagline:'Körper & innere Balance', img:'',
  desc:'…',
  bullets:['…','…','…'] }
```

`cat` steuert den Filter. `img:''` bedeutet: kein Bild vorhanden, die Kachel nutzt den Text-Fallback. Bild ergänzen = Datei nach `assets/` legen und `img:'assets/dateiname.png'` setzen.

**Shop-Produkte** (`shop.html`): Array `allProducts` in `renderVals()`.

**Navigation** steht in jeder Seite einzeln im `<header>`. Ein neuer Menüpunkt muss in allen acht Seiten ergänzt werden — es gibt kein gemeinsames Layout. Reihenfolge überall: Ihre Gesundheit · Training · Kurse · Medical Wellness · Über uns · Shop · Beratungstermin.

**Footer** ebenso pro Seite. Impressum- und Datenschutz-Link müssen auf **jeder** Seite bleiben — § 5 DDG verlangt ständige Verfügbarkeit.

---

## 4 · Gestaltung

Wenn du etwas Neues anlegst, halte dich an diese Werte.

| Rolle | Wert |
| --- | --- |
| Creme (Hintergrund) | `#F7F1E8` |
| Creme, zweite Stufe | `#EEEADD` |
| Salbeigrün (Marke) | `#5E6B50`, hover `#4C5740` |
| Salbei hell | `#A9B796` / `#B8C2A6` |
| Taupe-Braun | `#6B5A47` |
| Gold | `#C2A06F` |
| Rosé | `#C9A6A0` |
| Text | `#322E2B`, sekundär `#6B635C` |

Schriften: **Cormorant Garamond** für Überschriften und Zitate (500/600, oft kursiv), **Mulish** für Fließtext, **Jost** für Eyebrows und Navigation. Eyebrows sind Großbuchstaben mit `letter-spacing:6px`.

Radien: 6–12 px bei Karten, `999px` bei Buttons. Layout überwiegend `display:flex` / `grid` mit `gap`, Größen fluid über `clamp()`.

**Responsiv ohne Media Queries.** Die Seiten kommen fast ohne Breakpoints aus. Stattdessen:

- Mehrspaltige Grids immer `repeat(auto-fit, minmax(280px, 1fr))` — nie `repeat(3, 1fr)` oder `1fr 1fr`. Nur so bricht die Spalte auf dem Handy um.
- Seitliche Innenabstände fluid: `padding: 48px clamp(20px, 4vw, 64px)` statt `padding: 48px 64px`.
- **Jede** mehrgliedrige Flex-Reihe braucht `flex-wrap: wrap` plus `gap` — nicht nur Header und Button-Reihen, sondern auch Kennzahlen, Icon-Text-Paare, Footer-Spalten und Chip-Listen. Eine `nowrap`-Reihe mit mehr als zwei Kindern ragt auf dem Handy heraus und lässt die ganze Seite seitlich scrollen.
- Feste Pixelbreiten in Grids nur als `minmax(min(440px, 100%), 1fr)`.

Deutsche Komposita wie „Ganzkörpertraining" setzen eine hohe Mindestbreite — deshalb die Mindestwerte nicht unter 240 px drücken. Nach jeder Layout-Änderung bei 390 px Breite prüfen, dass nichts seitlich herausragt.

Die Unterseiten Training, Kurse, Medical Wellness und Über uns kommen aus separaten Entwürfen und haben eigene, leicht abweichende Paletten. Innerhalb einer Seite konsistent bleiben, nicht seitenübergreifend vereinheitlichen — es sei denn, das ist ausdrücklich die Aufgabe.

---

## 5 · Externe Ziele

| Zweck | URL |
| --- | --- |
| Beratungstermin (alle CTAs) | `https://calendly.com/ellevital-info/beratungsgesprach` |
| Selbsttest Bindegewebe | `https://ellevital.gesundfit.app/bindegewebeselbsttest-intro/` |
| Selbsttest Hormonbalance | `https://ellevital.gesundfit.app/hormonbalance-selbsttest-intro/` |
| Selbsttest Gelenke | `https://ellevital.gesundfit.app/gelenkeselbsttest-intro/` |
| Facebook | `https://www.facebook.com/profile.php?id=61562247976360` |
| Instagram | `https://www.instagram.com/ellevital.aa/` |
| LinkedIn | `https://www.linkedin.com/company/ellevital/` |

Externe Links immer mit `target="_blank" rel="noopener"`.

Kontakt: ellevital GmbH, Eduard-Pfeiffer-Str. 13, 73430 Aalen, 07361 62850, info@ellevital.com

---

## 6 · Offene Punkte

- **14 Kursfotos fehlen.** Sie waren in der Quelldatei nie enthalten. Betroffen: Body & Mind, Body Workout, Fascial Stretch, Formen & Straffen, Gesichts-Yoga, Medical Yoga, Mobility, Morning Mix, Pilates, Rücken & Gelenke, Step & Tone, Vitalzirkel, Brain Balance, Cardio Dance. Bis dahin greift der Text-Fallback.
- **Wochenplan-PDF fehlt.** Der Button auf `kurse.html` verweist ersatzweise auf `mailto:info@ellevital.com`. Sobald das PDF in `assets/` liegt, auf die Datei umstellen.
- **Ambient-Video fehlt.** Die beiden `<video>`-Elemente auf `medical-wellness.html` lagen auf der alten WordPress-Installation und hätten bei der Domain-Umstellung ins Leere gezeigt. Sie zeigen jetzt das Poster-Bild `assets/mw-8.jpg`. Sobald die MP4-Datei in `assets/` liegt, `src="assets/…"` an beiden Elementen ergänzen.
- **Foto Dr. Ospina** auf `index.html` lädt noch von `i0.wp.com/ellevital.com/…`, also über das Bild-CDN der alten Seite. Funktioniert derzeit, sollte aber nach `assets/` geholt werden.
- **Serverseitiges Rendering.** Die Seiten bauen sich im Browser auf. Für Besucher unkritisch, aber Suchmaschinen indexieren solche Seiten schlechter. Wenn Sichtbarkeit wichtig wird: gerenderten Zustand ins HTML schreiben.
- **Shop.** `shop.html` ist ein Katalog ohne Bezahlung; die Buttons zeigen „Bald verfügbar". Sobald der Anbieter feststeht, dort verlinken.

---

## 7 · Vor dem Push prüfen

- Öffnet die geänderte Seite ohne Fehler in der Konsole?
- Sind Impressum- und Datenschutz-Link noch im Footer?
- Zeigen alle Beratungstermin-Buttons auf Calendly, keiner auf `#`?
- Kein `href` **oder `src`** auf `ellevital.com` — interne Ziele sind relativ (`index.html`).
- Läuft der Header bei 390 px Breite nicht aus dem Bild?
- Alle `assets/…`-Pfade existieren wirklich?
- Bei neuer Seite: `sitemap.xml` ergänzen, Navigation aller Seiten erweitern.

---

## 8 · Deployment

| | |
| --- | --- |
| Build-Befehl | keiner |
| Publish-Verzeichnis | `/` |
| Branch | `main` |
| Domain | www.ellevital.com |

Push auf `main` → Deploy Now baut und veröffentlicht. Größere Änderungen zuerst über die `.ionos.space`-Vorschau-URL prüfen.
