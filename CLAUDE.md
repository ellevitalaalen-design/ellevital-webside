# ellevital – Website

Statische Website für **ellevital GmbH – Woman in Balance**, Aalen.
Hosting: **IONOS Deploy Now**. Jeder Push auf `main` veröffentlicht automatisch.

Diese Datei ist die Orientierung für Claude Code. Bitte vor Änderungen lesen.

---

## 1 · Wie die Seite funktioniert

Kein Build-Schritt, kein npm, kein Framework, keine Runtime. Neun HTML-Dateien und ein Ordner Bilder. Datei öffnen, ändern, pushen — fertig. Doppelklick genügt zum Ansehen, ein lokaler Server ist nicht nötig.

Jede Seite ist eine vollständige, für sich lesbare HTML-Datei:

```html
<!DOCTYPE html>
<html lang="de"><head>
  <title>…</title>
  <meta name="description" content="…">
  <link rel="canonical" href="https://www.ellevital.com/…">
  <!-- Open-Graph-Angaben -->
  <script type="application/ld+json">…</script>   <!-- strukturierte Daten -->
  <style>…</style>                                <!-- nur a:hover, [hidden], @keyframes -->
  <link href="…fonts.googleapis.com…">
</head>
<body>
  …fertiges Markup, alle Texte im Klartext…
  <script>(function(){ … })();</script>           <!-- Menü, Akkordeon, Filter -->
</body></html>
```

**Grundsätze:**

- **Alle Inhalte stehen fertig im HTML.** Keine Platzhalter, keine Templates, nichts wird im Browser zusammengebaut. Suchmaschinen und Besucher sehen dasselbe.
- **Styling inline** über `style="…"`. Der `<style>`-Block im `<head>` enthält nur, was inline nicht geht: `a:hover`, `[hidden]`, `@keyframes`, die zwei Media Queries für die Navigation.
- **Interaktion über kleines Vanilla-JS** am Seitenende, gesteuert über `data-`-Attribute. Kein jQuery, keine Bibliothek.
- **`hidden` zum Ein- und Ausblenden**, nie `style.display`. Dafür steht in jedem `<style>`-Block `[hidden] { display: none !important; }` — nötig, weil die Elemente ein Inline-`display` tragen, das `hidden` sonst überstimmt.

### Interaktionsmuster

| Muster | Auszeichnung | Vorkommen |
| --- | --- | --- |
| Mobilmenü | `#menubtn` + `#mobilmenu[hidden]`, `[data-close]` | `index.html`, `shop.html` |
| Akkordeon | `[data-acc][aria-expanded][aria-controls]` → Panel per `hidden` | `index.html` (Selbsttests), `kurse.html` (14 Kurse) |
| Kursfilter | `[data-filter="Kategorie"]` schaltet `[data-course][data-cat]` | `kurse.html` |
| Shop-Filter | `[data-cat-btn="key"]` schaltet `[data-product][data-cat]` | `shop.html` |
| Scroll-Einblendung | `[data-reveal]` + IntersectionObserver | `medical-wellness.html` |

Wichtig beim Akkordeon: die aufklappbaren Texte stehen **immer** im HTML und werden nur per `hidden` verborgen. Nicht auf Erzeugung per JS umstellen — die Inhalte sollen für Google sichtbar bleiben.

---

## 2 · Dateien

| Datei | Seite | Interaktion |
| --- | --- | --- |
| `index.html` | Startseite | Mobilmenü, Selbsttest-Akkordeon |
| `training.html` | Training / milon-Zirkel | keine |
| `kurse.html` | Kurse | Kategoriefilter + 14 Akkordeons |
| `rehasport.html` | Rehasport | Mobilmenü |
| `sauna.html` | Sauna | Mobilmenü, FAQ über `<details>` |
| `medical-wellness.html` | Medical Wellness | Scroll-Einblendung |
| `ueber-uns.html` | Über uns | keine |
| `shop.html` | Shop | Mobilmenü, Kategoriefilter |
| `impressum.html` | Impressum | keine |
| `datenschutz.html` | Datenschutzerklärung | keine (`noindex`) |
| `404.html` | Fehlerseite | keine |

Dazu `assets/` (26 Bilder, 2,3 MB), `robots.txt`, `sitemap.xml`.

**Dateinamen nicht umbenennen** — sie stehen in `sitemap.xml`, in den Canonical-Tags und in der Navigation jeder Seite.

---

## 3 · Wo welcher Inhalt liegt

**Texte** stehen im Klartext im Markup. Gesuchten Satz über alle Dateien suchen, ändern, fertig.

**Kurse** (`kurse.html`): 14 `<article data-course data-cat="…">`-Blöcke. Ein neuer Kurs = Block kopieren, Texte ersetzen, `data-cat` auf eine der Kategorien setzen (`Kraft`, `Beweglichkeit`, `Entspannung`, `Mind`) und die `id`/`aria-controls`-Paarung eindeutig halten.

**Shop-Produkte** (`shop.html`): `<article data-product data-cat="Gutscheine|Produkte">`-Blöcke, gleiche Vorgehensweise.

**Navigation** steht in jeder Seite einzeln im `<header>`. Ein neuer Menüpunkt muss in allen acht Seiten ergänzt werden — es gibt kein gemeinsames Layout. Reihenfolge überall: Ihre Gesundheit · Training · Kurse · Rehasport · Sauna · Medical Wellness · Über uns · Shop · Beratungstermin. Impressum und Datenschutz haben bewusst keine Navigation, nur den Rückweg zur Startseite.

**Footer** ebenso pro Seite. Impressum- und Datenschutz-Link müssen auf **jeder** Seite bleiben — § 5 DDG verlangt ständige Verfügbarkeit.

---

## 4 · Suchmaschinen

Jede öffentliche Seite trägt: eigenen `<title>` (unter 60 Zeichen), `description` (unter 160), `canonical`, `robots`, Open-Graph-Angaben und strukturierte Daten als JSON-LD.

Die strukturierten Daten bilden einen Graphen. Der Betrieb ist einmal auf der Startseite vollständig beschrieben (`HealthAndBeautyBusiness` mit `@id: https://www.ellevital.com/#studio` — Adresse, Öffnungszeiten, Leistungen, 13 Orte im Einzugsgebiet). Die Unterseiten verweisen darauf per `{"@id": "…#studio"}` statt es zu wiederholen. Bei Änderungen an Adresse, Telefon oder Öffnungszeiten: **Startseite ist die Quelle**, dort ändern.

Pro Unterseite zusätzlich ein `Service`- bzw. `ItemList`-Objekt und ein `BreadcrumbList`.

Suchbegriffe, auf die die Seiten ausgerichtet sind: Frauengesundheit Aalen, Frauenfitness Aalen, Fitnessstudio für Frauen Aalen, Rehasport Aalen, Wechseljahre Beratung Aalen, Beckenbodentraining, Pilates Aalen, Sauna Aalen, Medical Wellness Aalen, Longevity für Frauen.

`&` in `<title>` und `content="…"` immer als `&amp;` schreiben.

**Bilder:** JPEG, längste Kante höchstens 1400 px, Qualität ~0,82. Jedes `<img>` braucht `width`/`height` (verhindert Layout-Sprünge) und alles unterhalb des ersten Bildschirms `loading="lazy" decoding="async"`.

---

## 5 · Gestaltung

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

**Kontrast prüfen, nicht schätzen.** Kleiner Text (unter 18,7 px) braucht mindestens **4,5:1** gegen seinen Hintergrund — das gilt besonders für die Impressum- und Datenschutz-Links im Fuß, die nach § 5 DDG nicht nur vorhanden, sondern auch auffindbar sein müssen. Gedämpfte Grautöne auf dunklem Grund fallen hier regelmäßig durch: `#8A8077` auf `#322E2B` erreicht nur 3,5:1. Jede Fuß-Palette hat einen helleren Ton, der passt (`#CFC8BF`, `#B5AD9E`, `#B8AE9D`) — den nehmen, statt einen neuen Farbwert zu erfinden.

Radien: 6–12 px bei Karten, `999px` bei Buttons. Layout überwiegend `display:flex` / `grid` mit `gap`, Größen fluid über `clamp()`.

**Responsiv ohne Media Queries.** Die Seiten kommen fast ohne Breakpoints aus (Ausnahme: die zwei Regeln, die Desktop-Navigation und Hamburger bei 880 px umschalten). Stattdessen:

- **Der Kopf braucht `min-height`, nie `height`.** Der innere Container trägt `flex-wrap: wrap` — mit fester Höhe bricht die Navigation in eine zweite Zeile, die außerhalb des Kopfs liegt und ohne Hintergrund über dem Inhalt schwebt. Mit `min-height:76px` wächst der Kopf statt zu überlaufen.
- **Der Nav-Breakpoint muss zur Zahl der Einträge passen.** Neun Einträge brauchen rund 1100 px in einer Zeile (Nav + Logo 142 px + Padding 74 px), deshalb schaltet der Hamburger erst ab 1181 px auf die Desktop-Navigation um (`max-width: 1180px` / `min-width: 1181px`). Kommt ein Menüpunkt dazu, den Bedarf neu messen (`nav.scrollWidth` + Logo + Padding) und den Wert mit Reserve darüber setzen — sonst entsteht ein Band von Fensterbreiten, in dem die Leiste umbricht und der Kopf doppelt so hoch wird. 1024 px ist die kritische Breite: iPad quer und geteilte Fenster auf großen Displays landen genau dort. Bei so langer Navigation ist irgendwann ein Untermenü die bessere Antwort als ein noch höherer Breakpoint.
- Mehrspaltige Grids immer `repeat(auto-fit, minmax(280px, 1fr))` — nie `repeat(3, 1fr)` oder `1fr 1fr`. Nur so bricht die Spalte auf dem Handy um.
- Seitliche Innenabstände fluid: `padding: 48px clamp(20px, 4vw, 64px)` statt `padding: 48px 64px`.
- **Jede** mehrgliedrige Flex-Reihe braucht `flex-wrap: wrap` plus `gap` — nicht nur Header und Button-Reihen, sondern auch Kennzahlen, Icon-Text-Paare, Footer-Spalten und Chip-Listen. Eine `nowrap`-Reihe mit mehr als zwei Kindern ragt auf dem Handy heraus und lässt die ganze Seite seitlich scrollen.
- Feste Pixelbreiten in Grids nur als `minmax(min(440px, 100%), 1fr)`.

Deutsche Komposita wie „Ganzkörpertraining" setzen eine hohe Mindestbreite — deshalb die Mindestwerte nicht unter 240 px drücken. Nach jeder Layout-Änderung bei 390 px Breite prüfen, dass nichts seitlich herausragt.

Die Unterseiten Training, Kurse, Medical Wellness, Über uns und Sauna kommen aus separaten Entwürfen und haben eigene, leicht abweichende Paletten — die Sauna-Seite arbeitet mit Gold `#B7924F` und Oliv `#6E7358` auf `#F7F3EC`. Innerhalb einer Seite konsistent bleiben, nicht seitenübergreifend vereinheitlichen — es sei denn, das ist ausdrücklich die Aufgabe.

---

## 6 · Externe Ziele

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

Öffnungszeiten: Mo/Mi/Fr 8:30–21:30 · Di/Do 8:30–12:00 und 15:00–21:30 · Sa/So 10:00–15:00

---

## 7 · Offene Punkte

- **Rehasport ohne Bildmaterial.** `rehasport.html` ist textgeführt aufgebaut, weil die Bilder der alten Seite reine Canva-Grafiken mit eingebranntem Text waren. Echte Aufnahmen aus den Kursen würden die Seite deutlich stärker machen — dann Hero-Bild und je ein Bild pro Angebot ergänzen.
- **Rehasport-Video fehlt.** Auf der alten Seite lag `VIDEO-rehasport.mp4`. Falls gewünscht: Datei nach `assets/` legen und einen Abschnitt anlegen.
- **14 Kursfotos fehlen.** Sie waren in der Quelldatei nie enthalten; die Kacheln zeigen nur Text. Betroffen: Body & Mind, Body Workout, Fascial Stretch, Formen & Straffen, Gesichts-Yoga, Medical Yoga, Mobility, Morning Mix, Pilates, Rücken & Gelenke, Step & Tone, Vital Zirkel, Brain Balance, Cardio Dance Mix.
- **Wochenplan ist keine Datei.** Der Abschnitt auf `kurse.html` zeigt den Plan als Bild (`assets/kurse-3-opt.jpg`); es gibt bewusst keinen Download und keine PDF-Anfrage, der Button führt zum Beratungsgespräch. Bei neuem Plan einfach das Bild ersetzen.
- **Ambient-Video entfernt.** Es lag auf der alten WordPress-Installation. Soll es zurück: MP4 nach `assets/` legen und den Abschnitt neu anlegen.
- **Shop ohne Bezahlung.** `shop.html` ist ein Katalog, die Buttons zeigen „Bald verfügbar". Sobald der Anbieter feststeht, dort verlinken.
- **Schreibweise der Straße prüfen.** Impressum und die neueren Seiten schreiben „Eduard-Pfeiffer-Straße“ (zwei f), die alte WordPress-Seite schrieb „Eduard Pfeifer Strasse“. Maßgeblich ist die Handelsregister-Schreibweise — einmal verbindlich klären und überall gleich setzen.
- **Google-Unternehmensprofil ist veraltet.** Für die lokale Sichtbarkeit der wirksamste offene Punkt — wirkt stärker als jede weitere Änderung an der Website.

---

## 8 · Vor dem Push prüfen

- Öffnet die geänderte Seite ohne Fehler in der Konsole?
- Sind Impressum- und Datenschutz-Link noch im Footer — und erreichen sie mindestens 4,5:1 Kontrast?
- Zeigen alle Beratungstermin-Buttons auf Calendly, keiner auf `#`?
- Kein `href` **oder `src`** auf `ellevital.com` oder `i0.wp.com` — interne Ziele sind relativ (`index.html`), Bilder liegen in `assets/`.
- Läuft der Header bei 390 px Breite nicht aus dem Bild?
- Alle `assets/…`-Pfade existieren wirklich?
- Enthält die Seite noch `{{`, `<sc-`, `<x-dc>` oder `support.js`? Dann ist Template-Rest übrig geblieben — muss raus.
- Bei neuer Seite: `sitemap.xml` ergänzen, Navigation aller Seiten erweitern, `title`/`description`/`canonical`/JSON-LD setzen.

---

## 9 · Deployment

| | |
| --- | --- |
| Build-Befehl | keiner |
| Publish-Verzeichnis | `/` |
| Branch | `main` |
| Domain | www.ellevital.com |

Push auf `main` → Deploy Now baut und veröffentlicht. Größere Änderungen zuerst über die `.ionos.space`-Vorschau-URL prüfen.
