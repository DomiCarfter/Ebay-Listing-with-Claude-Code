# eBay Listing Automation mit Playwright & Claude

Automatisiertes Erstellen von eBay-Listings per Browser-Automation (Playwright CLI) und KI-gestützter Beschreibungsgenerierung (Claude).

## Was macht dieses Projekt?

- **Konkurrenzanalyse**: Günstigstes Angebot auf eBay finden (Neu, Deutschland, günstigste zuerst)
- **Strategische Preisgestaltung**: Gesamtpreis inkl. Versand kalkulieren, Mengenrabatt-Staffelung
- **Listing-Erstellung**: Formular automatisiert ausfüllen (Titel, HTML-Beschreibung, Preis, Versand)
- **Foto-Upload**: Produktfotos hochladen und archivieren
- **Mehrfach-Listings**: Sets mit Mengenrabatt (2x, 5x, 25x, 50x) über "Ähnlichen Artikel verkaufen"

## Voraussetzungen

- [Node.js](https://nodejs.org/)
- [Playwright CLI](https://playwright.dev/) (`npx playwright install`)
- Claude Code CLI

## Setup

```bash
# Playwright Browser installieren
npx playwright install

# eBay-Session starten und Auth speichern
playwright-cli -s=main open https://www.ebay.de --headed
playwright-cli -s=main state-load ebay-auth.json
```

## Projektstruktur

```
├── CLAUDE.md           # Workflow-Anleitung für Claude
├── ebay.txt            # Prompt-Template für Listing-Beschreibungen
├── ebay-auth.json      # eBay Session-Daten (nicht im Repo)
├── ebay fotos/         # Produktfotos (nicht im Repo)
│   └── archiviert/     # Bereits verwendete Fotos
├── .claude/            # Claude Code Einstellungen
└── .playwright-cli/    # Playwright Logs & Screenshots
```

## Workflow

1. **Konkurrenz suchen** – günstigstes Angebot auf eBay finden
2. **Preis kalkulieren** – Versand einrechnen, Staffelpreise berechnen
3. **"Solch einen Artikel verkaufen"** – Formular über Konkurrenzangebot öffnen
4. **Fotos hochladen** – aus `ebay fotos/` in Root kopieren, hochladen, archivieren
5. **Formular ausfüllen** – Titel, HTML-Beschreibung, Preis, Versand (Verkäufer zahlt)
6. **Listing veröffentlichen** – "Artikel kostenlos einstellen"

## Regeln

- Keine Specs annehmen – nur bestätigte Angaben
- Kein Preisvorschlag
- Versand immer "Verkäufer zahlt" (in Preis eingerechnet)
- Beschreibung immer als HTML
- Privatverkauf-Disclaimer am Ende jeder Beschreibung

## Lizenz

Privates Projekt – kein öffentlicher Gebrauch vorgesehen.
