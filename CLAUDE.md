# eBay Listing Workflow – Claude Anleitung

## Setup

- eBay Session laden: `playwright-cli -s=main open https://www.ebay.de --headed && playwright-cli -s=main state-load ebay-auth.json`
- Auth-Datei: `ebay-auth.json` im Projektordner
- Fotos liegen in: `ebay fotos/` → vor Upload in Root kopieren (Leerzeichen im Pfad blockiert Upload)

---

## Workflow pro Artikel

### 1. Günstigstes Konkurrenzangebot suchen
- Suche: `https://www.ebay.de/sch/i.html?_nkw=SUCHBEGRIFF&LH_ItemCondition=1000&_sop=15&_fsrp=1&LH_PrefLoc=1`
- Filter: Neu, günstigste zuerst, Standort Deutschland
- Günstigstes Angebot anklicken → **Gesamtpreis** (inkl. Versand) notieren

### 2. Strategische Preisgestaltung
- Preis immer als **Gesamtpreis** (Artikelpreis + Versandkosten = Verkaufspreis)
- Versandkosten intern einrechnen, Käufer sieht "Kostenloser Versand"
- Versandart nach Größe:
  - Kleine Artikel (z.B. BIOS Speaker, Kabel) → **Brief groß** (~1,80€)
  - Mittlere Artikel → **Versandtasche / Paket klein** (~4,99€)
  - Artikel ab 50€ → **Paket** (~4,99€)

### 3. Listing erstellen via "Solch einen Artikel verkaufen"
Auf dem günstigsten Konkurrenzangebot auf "Solch einen Artikel verkaufen" klicken → neues Tab öffnet Formular.

---

## Formular ausfüllen

### Fotos
- Fotos liegen in `ebay fotos/` (nicht benutzte) oder `ebay fotos/archiviert/` (bereits verwendet)
- Vor dem Upload in den Root-Ordner kopieren (Leerzeichen im Pfad blockiert Upload):
  ```bash
  cp "ebay fotos/FOTO.png" "./foto_tmp.png"
  ```
- **Fotos immer zuerst anschauen** (Read-Tool) um Artikel korrekt zu identifizieren — nicht aus dem Dateinamen ableiten!
- Upload: Button "Vom Computer hochladen" → Datei aus Root wählen
- Weitere Fotos: "Hinzufügen" → Menuitem "Vom Computer hochladen" → `upload`
- Nach erfolgreichem Listing: Fotos in `ebay fotos/archiviert/` verschieben und temporäre Kopien im Root löschen:
  ```bash
  mv "ebay fotos/FOTO.png" "ebay fotos/archiviert/"
  rm foto_tmp.png
  ```

### Titel (max. 80 Zeichen)
- Maximale Länge: 80 Zeichen (eBay-Limit!) → Zeichenanzahl immer angeben
- Wichtigste Keywords zuerst: Marke, Modell, Hauptmerkmal
- Zustand einbauen (Neu/Wie Neu/Sehr gut/Gut/Akzeptabel)
- 1-2 Alleinstellungsmerkmale
- Keine Sonderzeichen oder Emojis
- Keine Füllwörter ("tolles", "super", "schönes")
- Zahlen statt Wörter (z.B. "128GB" statt "hundertachtundzwanzig")
- Beispiel: `2x R30 PC Interner Lautsprecher Speaker Mainboard BIOS Buzzer 6cm mini Neu` (71 Zeichen)

### Zustand
- Button "Neu" oder "Gebraucht" je nach Artikel anklicken
- Bei "Solch einen Artikel verkaufen": Zustand-Dropdown öffnen und passende Option wählen
- Bei "Ähnliches Angebot erstellen": Zustand wird vom vorherigen Listing übernommen — prüfen ob korrekt

### Beschreibung (HTML)
1. Checkbox "HTML-Code anzeigen" aktivieren
2. HTML direkt in das Textfeld einfügen

**Allgemeine Regeln:**
- Zeilenumbrüche immer mit `<br>` (kein Plaintext-Zeilenumbruch)
- Fettschrift mit `<b>ÜBERSCHRIFT:</b>`
- Aufzählungen mit ✓ + `<br>` je Punkt
- Trennlinien mit `<hr>`
- Nur ✓ als Symbol, keine anderen Emojis
- Keine Zahlungsmöglichkeiten (eBay regelt das)
- SEO-optimiert: Wichtige Suchbegriffe natürlich einbauen

**Einleitung:**
- Aufmerksamkeitsstarker erster Satz
- 2-3 wichtigste Verkaufsargumente
- Zustand klar und ehrlich nennen
- Ton: Professionell, vertrauenswürdig, sachlich-freundlich

**Beschreibung ist kategorieabhängig – passende Vorlage verwenden:**

#### Vorlage: Technische Produkte (Elektronik, Werkzeug, PC-Komponenten)
```html
<b>PRODUKTBEZEICHNUNG – Neu & unbenutzt</b><br><br>
Einleitungssatz mit 2-3 Verkaufsargumenten.<br>
<hr>
<b>PRODUKTDATEN:</b><br>
✓ Marke & Modell: ...<br>
✓ Artikelnummer: ... (falls sichtbar/bekannt)<br>
✓ Farbe/Ausführung: ...<br>
✓ Maße/Gewicht: ... (falls relevant)<br>
<br>
<b>TECHNISCHE DETAILS:</b><br>
✓ Alle relevanten, BESTÄTIGTEN Specs<br>
✓ Keine Annahmen – lieber weglassen als falsch angeben<br>
<br>
<b>LIEFERUMFANG:</b><br>
✓ Nx Artikel<br>
✓ Mit OVP / Ohne OVP<br>
✓ Fehlende Teile klar benennen (z.B. "Ohne Netzteil")<br>
<br>
<b>ZUSTAND:</b><br>
✓ Neu, unbenutzt<br>
✓ Gebrauchsspuren konkret beschreiben (wo, was)<br>
✓ Funktionsfähigkeit bestätigt<br>
<hr>
Privatverkauf – keine Rücknahme, keine Garantie, kein Umtausch. Tierfreier Nichtraucherhaushalt.
```

#### Vorlage: Medien (Spiele, DVDs, CDs, Bücher)
```html
<b>PRODUKTBEZEICHNUNG – Zustand</b><br><br>
Einleitungssatz.<br>
<hr>
<b>ÜBER DIESES PRODUKT:</b><br>
✓ Titel: ...<br>
✓ Publisher/Autor: ...<br>
✓ Plattform/Format: ...<br>
✓ Region/Sprache: ...<br>
✓ USK/FSK: ... (falls vorhanden)<br>
<br>
<b>LIEFERUMFANG:</b><br>
✓ Disc/Buch vorhanden<br>
✓ Hülle/Einband: ...<br>
✓ Anleitung/Booklet: Ja/Nein<br>
<br>
<b>ZUSTAND:</b><br>
✓ Hülle: ...<br>
✓ Disc/Seiten: ...<br>
<hr>
Privatverkauf – keine Rücknahme, keine Garantie, kein Umtausch. Tierfreier Nichtraucherhaushalt.
```

#### Vorlage: Kleidung & Accessoires
```html
<b>PRODUKTBEZEICHNUNG – Zustand</b><br><br>
Einleitungssatz.<br>
<hr>
<b>PRODUKTDETAILS:</b><br>
✓ Marke: ...<br>
✓ Modell: ...<br>
✓ Größe: ...<br>
✓ Farbe & Muster: ...<br>
✓ Material: ... (nur wenn bekannt/sichtbar)<br>
✓ Passform: ...<br>
<br>
<b>ZUSTAND:</b><br>
✓ Gebrauchsspuren konkret beschreiben<br>
✓ Maße wenn relevant<br>
<hr>
Privatverkauf – keine Rücknahme, keine Garantie, kein Umtausch. Tierfreier Nichtraucherhaushalt.
```

#### Vorlage: Kabel & Adapter
```html
<b>PRODUKTBEZEICHNUNG – Neu & unbenutzt</b><br><br>
Einleitungssatz.<br>
<hr>
<b>PRODUKTDATEN:</b><br>
✓ Typ und Anschlüsse: ... (z.B. "USB-A auf USB-B")<br>
✓ Länge: ... (nur wenn bekannt/gemessen)<br>
✓ Standard: ... (z.B. Cat6, USB 3.0 – nur wenn bestätigt/aufgedruckt)<br>
✓ Marke: ... (nur wenn erkennbar)<br>
<br>
<b>LIEFERUMFANG:</b><br>
✓ Nx Kabel/Adapter<br>
✓ Mit OVP / Ohne OVP<br>
<br>
<b>ZUSTAND:</b><br>
✓ Neu, unbenutzt<br>
✓ Funktionsfähigkeit bestätigt<br>
<hr>
Privatverkauf – keine Rücknahme, keine Garantie, kein Umtausch. Tierfreier Nichtraucherhaushalt.
```

#### Sonderregeln Smartphones
- IMEI niemals in der Beschreibung nennen
- Akkuzustand als Platzhalter lassen wenn nicht geprüft
- Speichervariante, Simlock, Farbe abfragen

### Preisgestaltung
- Format: Sofort-Kaufen
- Artikelpreis: Gesamtpreis eintragen (inkl. Versand)
- **Preis-Eingabe:** `click` auf Feld → `type "175"` → `press ","` → `type "00"` → `press "Tab"` → im Snapshot verifizieren
- **NIEMALS** `fill` für Preisfeld verwenden — Wert wird bei Re-Render gelöscht
- **NIEMALS** Punkt als Dezimaltrenner — `4.99` wird zu `499,00`!
- Stückzahl: Wie viele Sets vorhanden (z.B. 10)
- **Preisvorschläge zulassen: IMMER deaktivieren** (Switch ausschalten)
- Preisvorschläge sind bei JEDER Listing-Erstellung potenziell aktiv — egal ob "Solch einen Artikel verkaufen" ODER "Ähnliches Angebot erstellen"
- **Immer im Snapshot prüfen** ob `[checked]` am Switch steht und ggf. deaktivieren

### Versand
- "Verkäufer zahlt" auswählen
- Versandart passend wählen (Brief Maxi / Paket / DHL)
- **IMMER den BPEC→SP Bug-Fix VOR dem ersten Publish durchführen:**
  1. `document.querySelector('button[value="BPEC"]').click()` (Käufer zahlt)
  2. Kurz warten
  3. `document.querySelector('button[value="SP"]').click()` (Verkäufer zahlt)
  4. **Danach Preisfeld erneut prüfen** — Toggle kann den Preis leeren!
  5. Verifizieren: Snapshot muss `Käufer sehen „Kostenloser Versand"` zeigen

### Standort
- Wird automatisch aus Profil übernommen (Deutschland)

---

## Veröffentlichen

```js
// Einstellen-Button (außerhalb Viewport → per eval klicken):
document.querySelector('[aria-label="Artikel kostenlos einstellen"]').click()
```

**WICHTIG — Vor dem Veröffentlichen immer prüfen:**
1. Versand-Bug-Fix (BPEC→SP) **VOR** dem ersten Klick auf "Einstellen" durchführen — nicht erst nach Fehlermeldung
2. Preis-Feld prüfen — eBay leert das Feld bei Seitenaktualisierungen (z.B. nach Versand-Toggle)
3. Preisvorschläge-Switch prüfen — wird bei "Ähnliches Angebot" immer zurückgesetzt

---

## Mehrfach-Listings (Mengenrabatt-Sets)

Für Sets immer "Ähnlichen Artikel verkaufen" auf dem vorherigen Listing klicken.
**Wichtig:** Bei jedem neuen Listing über "Ähnlichen Artikel verkaufen" wird "Preisvorschläge zulassen" erneut aktiviert — immer manuell deaktivieren!
Menge anpassen in Titel, Beschreibung, Preis und Stückzahl.

Typische Staffelung:
| Set  | Preis/Stk | Rabatt ggü. Einzeln |
|------|-----------|---------------------|
| 2x   | ~94%      | -6%                 |
| 5x   | ~86%      | -14%                |
| 25x  | ~75%      | -25%                |
| 50x  | ~65%      | -35%                |

---

## Rückfragen-Schema

Vor dem Listing bei unklaren Angaben gezielt nachfragen. Nie Specs annehmen – lieber fragen.

**Elektronik/Hardware:**
- Zustand? (Neu/Neuwertig/Sehr gut/Gut/Akzeptabel/Defekt)
- OVP vorhanden?
- Vollständiger Lieferumfang? (was fehlt?)
- Funktionstest durchgeführt?

**Smartphones:**
- Speichervariante?
- Farbe?
- Akkuzustand geprüft?
- Simlock vorhanden?
- Mit/ohne OVP?
- Sichtbare Schäden konkret beschreiben

**Kabel:**
- Länge bekannt/gemessen?
- Standard aufgedruckt? (Cat5e/Cat6, USB-Version etc.)
- Neu oder gebraucht?

**Spiele/Medien:**
- Zustand Hülle und Disc/Seiten separat?
- Anleitung/Booklet dabei?

---

## Wichtige Regeln

- Keine Specs annehmen – nur bestätigte Angaben verwenden
- Bei Unsicherheit: Frage stellen statt raten
- Kein Preisvorschlag
- Versand immer Verkäufer zahlt (in Preis einrechnen)
- Preise immer als Gesamtpreis
- Standort immer Deutschland
- Beschreibung immer als HTML (Checkbox aktivieren!)
- Privatverkauf-Disclaimer immer am Ende
- Keine Zahlungsmöglichkeiten erwähnen (eBay regelt das)
- Nur ✓ als Symbol, keine anderen Emojis
- Rechtschreibung und Grammatik perfekt
- Ehrlich und transparent über Zustand berichten

---

## Bekannte Bugs & Workarounds (playwright-cli)

### Preisfeld wird bei Seitenaktualisierung geleert
- `fill` für das Preisfeld funktioniert oft nicht zuverlässig — Wert wird bei Re-Render gelöscht
- **Lösung:** Preisfeld mit `click` fokussieren → `type "PREIS"` → `press "Tab"` zum Bestätigen
- **WICHTIG:** Dezimaltrenner ist **Komma**, nicht Punkt! `4.99` wird als `499,00` interpretiert
- Preis immer als separaten Schritt eingeben: erst `type "4"`, dann `press ","`, dann `type "99"`
- Nach Tab-Druck immer im Snapshot verifizieren, dass der Preis korrekt steht

### Versand-Bug "Versandservicekosten müssen auf 0 gesetzt werden"
- Tritt fast IMMER auf bei "Solch einen Artikel verkaufen" und "Ähnliches Angebot erstellen"
- **Lösung:** BPEC→SP Toggle **IMMER VOR dem ersten Publish-Versuch** durchführen:
  ```js
  document.querySelector('button[value="BPEC"]').click()  // kurz warten
  document.querySelector('button[value="SP"]').click()
  ```
- Danach Preisfeld erneut prüfen — Toggle kann den Preis leeren!

### HTML-Beschreibung: Ref ändert sich nach Checkbox-Toggle
- Nach dem Aktivieren der HTML-Checkbox ändert sich die Ref des Beschreibungs-Textfelds
- **Lösung:** Nach `check` der HTML-Checkbox immer neuen Snapshot machen und neue Ref für Beschreibung suchen

### Foto-Upload: Mehrere Fotos nacheinander
- `upload` akzeptiert nur eine Datei pro Aufruf
- Für weitere Fotos: "Hinzufügen" Button klicken → öffnet Dropdown-Menü → "Vom Computer hochladen" Menuitem klicken → dann `upload`
- Workflow pro zusätzliches Foto:
  1. `click` auf "Hinzufügen" Button
  2. `click` auf menuitem "Vom Computer hochladen" (NICHT den Button, sondern den Menuitem!)
  3. `upload "datei.jpeg"`

### File-Chooser-Modals können sich stapeln
- Wenn mehrere File-Chooser-Modals offen sind, blockieren sie andere Befehle (`eval`, `press`, etc.)
- **Lösung:** Pro Modal einmal `upload` mit beliebiger Datei ausführen um es zu schließen

### Session-Ablauf
- eBay-Session läuft nach Inaktivität ab → Redirect auf Login-Seite
- **Lösung:** User loggt manuell im Browser ein → `state-save ebay-auth.json`
- Neues Tab über "Solch einen Artikel verkaufen" öffnet sich ggf. auf Login-Seite → nach Login Tab wechseln

### Elemente außerhalb des Viewports
- `click` per Ref funktioniert nicht für Elemente außerhalb des sichtbaren Bereichs
- **Lösung:** `eval "document.querySelector('...').click()"` verwenden
