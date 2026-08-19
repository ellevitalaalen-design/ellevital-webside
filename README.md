# ellevital – Website

Statische Website für **ellevital – Woman in Balance**, Aalen.
Deployment über **IONOS Deploy Now**.

## Seiten

| Datei | Seite |
| --- | --- |
| `index.html` | Startseite |
| `training.html` | Training |
| `kurse.html` | Kurse |
| `medical-wellness.html` | Medical Wellness |
| `ueber-uns.html` | Über uns |
| `shop.html` | Shop |
| `impressum.html` | Impressum |
| `datenschutz.html` | Datenschutzerklärung |
| `404.html` | Fehlerseite |

Dazu: `assets/` (Bilder, Logo, QR-Code), `support.js` (Runtime), `robots.txt`, `sitemap.xml`.

## Deployment (IONOS Deploy Now)

- **Build-Befehl:** keiner (rein statisch)
- **Publish-Verzeichnis:** `/` (Repository-Wurzel)
- **Branch:** `main`

Jeder Push auf `main` löst automatisch ein neues Deployment aus.

## Bearbeiten

Alle Seiten sind einzelne HTML-Dateien und können direkt bearbeitet werden.
Bilder liegen in `assets/`.

**Vor Änderungen `CLAUDE.md` lesen** — dort stehen Aufbau, Farben, Schriften,
wo welcher Inhalt liegt und was vor dem Push zu prüfen ist.

Lokal ansehen:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

Ein Server ist nötig, weil `support.js` über `file://` nicht geladen wird.

## Externe Links

- Beratungstermin: https://calendly.com/ellevital-info/beratungsgesprach
- Selbsttests: https://ellevital.gesundfit.app
