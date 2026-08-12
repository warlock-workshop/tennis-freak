# 🎾 Tennis Freak – ATP & WTA in einer App

Eine kleine Web-App für das iPhone, die die Live-Spielstände der **ATP-** und
**WTA-Tour** in einer Ansicht zusammenführt. Kein Backend, kein Build-Schritt:
eine einzige HTML-Datei plus App-Icon. Oberfläche auf **britischem Englisch**.

## Navigation

- Oben: **ATP / WTA** als Segment-Schalter, darunter **Singles / Doubles**
  als dezente Text-Tabs (Mixed zählt zu Doubles)
- Unten: **Live / Finished / Upcoming / Rankings** in einer schwebenden
  Leiste; der Live-Tab zeigt die Anzahl laufender Matches der aktuellen
  Auswahl
- **Rankings**: Weltrangliste (Top 150) der gewählten Tour mit Punkten und
  Trend-Pfeilen; Singles/Doubles blendet sich dort aus, da der Feed nur
  Einzel-Ranglisten liefert

Dazwischen die Matches: In Finished/Upcoming zuerst **nach Tagen** (Today,
Yesterday/Tomorrow, …), darin **nach Turnier** und innerhalb des Turniers
**nach Runden**. Die zuletzt gewählte Ansicht wird gemerkt.

## Detailansicht

**Ein Tap auf ein Match** öffnet ein Blatt mit:

- **Head-to-head** – alle früheren Duelle der beiden mit Jahr, Turnier,
  Runde, Sieger und Ergebnis
- **Player cards** – Weltranglistenplatz mit Punkten, Einzelbilanz der
  Saison, Titel, Preisgeld, Alter, Schlaghand, Größe, Geburtsort, Profi seit
- Laufende Matches aktualisieren sich weiter, solange das Blatt offen ist

**Ein Tap auf eine Ranglistenzeile** öffnet dasselbe Blatt für einen Spieler,
mit seinen letzten acht Matches: Gegner, `def.`/`lost to` und Ergebnis –
Letzteres bei Niederlagen aus seiner Sicht gedreht, weil ESPN es immer aus
Sicht des Siegers schreibt.

Beim Doppel entfällt die Duell-Bilanz: Vier Spieler ergeben keine saubere
Paarbilanz. Kopf, Ergebnis und die Profile aller vier bleiben.

## Funktionen

- **Court** jedes Matches in der Infozeile (sofern der Feed ihn liefert)
- **Unterbrochene Matches** (Regen/Dunkelheit) bleiben unter **Live** stehen
  und sind orange als „Suspended" markiert – inkl. Fortsetzungszeit, sobald
  die Neuansetzung im Feed steht. (ESPN sortiert sie sonst fälschlich unter
  Finished bzw. Upcoming ein.)
- **Setzlisten-Nummer** in Klammern neben dem Namen
- Laufender Satz im Live-Match farblich hervorgehoben
- Satzergebnisse mit Tiebreak-Punkten, Siegerinnen/Sieger fett
- Automatische Aktualisierung jede Minute (nur solange die App sichtbar ist),
  zusätzlich manueller Aktualisieren-Knopf
- Alle Zeiten in deutscher Zeit (Europe/Berlin)
- **Versionshinweis** einmal je Veröffentlichung; die laufende Version steht
  in der Statuszeile hinter dem Zeitstempel

## Datenquelle

Zwei ESPN-Schnittstellen, beide öffentlich, ohne Schlüssel und mit offenen
CORS-Headern – deshalb kommt die App ohne Server aus:

| Zweck | Adresse |
|---|---|
| Spielstände | `site.api.espn.com/…/tennis/atp\|wta/scoreboard` |
| Weltrangliste | `site.api.espn.com/…/tennis/atp\|wta/rankings` |
| Spielerprofile, Saisonbilanz, Matchlisten | `sports.core.api.espn.com/v2/sports/tennis/…` |

**Head-to-head gibt es bei ESPN nicht als fertige Abfrage.** Zwei Spieler
haben genau dann gegeneinander gespielt, wenn dieselbe Match-Kennung in
beiden Saison-Matchlisten steht; die App bildet die Schnittmenge über fünf
Saisons. Von diesen Listen werden nur die Kennungen behalten (rund 1 KB je
Spieler und Saison statt 40 KB) und in `localStorage` zwischengespeichert –
abgeschlossene Saisons dauerhaft, die laufende einen Tag.

**Was ESPN für Tennis nicht liefert** (geprüft am 12.08.2026 über alle vier
Zugänge, inklusive der Seite espn.com selbst):

- keinen **Live-Punktestand** (0/15/30/40) und keinen Aufschlag-Indikator
- keine **Match-Statistik** (Asse, Doppelfehler, Breakbälle) – das Feld
  `competitor.statistics` ist immer leer
- keinen Punkt-für-Punkt-Verlauf

Die Quell-Flags `gameSource`, `linescoreSource` und `statsSource` stehen bei
jedem Turnier auf `none`, auch bei den Grand Slams. Diese Daten sind ein
exklusiv vermarktetes Wettdaten-Produkt und aus keiner freien, browser-seitig
abrufbaren Quelle zu bekommen. Die Detailansicht zeigt deshalb Spielerdaten,
keine Matchdaten.

**Hinweis:** Die Schnittstellen sind inoffiziell – sollte ESPN sie ändern,
zeigt die App eine sichtbare Fehlermeldung statt veralteter Stände.

## Aufs iPhone bringen

1. **Hosten** – `index.html` und `icon.png` auf einen statischen Host legen.
   Hier läuft es über GitHub Pages direkt aus `main`.
2. Die URL auf dem iPhone in **Safari** öffnen.
3. **Teilen-Symbol → „Zum Home-Bildschirm"** – die App startet im Vollbild
   wie eine native App.

### Nach einem Update

Die abgelegte App hält ihr HTML fest, und da die Seite selbst nicht scrollt,
gibt es kein Ziehen-zum-Aktualisieren. Der Aktualisieren-Knopf holt nur die
Spielstände, nicht die Seite.

Verlässlich neu laden: **Symbol vom Home-Bildschirm löschen, Adresse in
Safari öffnen, neu ablegen.** Ob es geklappt hat, sagt die Versionskennung in
der Statuszeile.

Wird `apple-mobile-web-app-status-bar-style` geändert, ist das Neuablegen
zwingend – iOS liest diese Einstellung nur beim Ablegen.

### Schneller Test im Heimnetz

```bash
cd tennis-freak
python3 -m http.server 8123
```

Dann auf dem iPhone `http://<IP-des-Macs>:8123` öffnen (gleiches WLAN
vorausgesetzt).

## Aufbau

Die Seite ist ein fester Rahmen in Bildschirmhöhe: Die Kopfzeile steht,
`main` ist der einzige scrollende Bereich. Dadurch federt die Seite oben
nicht nach, die Bildlaufleiste beginnt unter der Kopfzeile, und die
schwebende Leiste unten hängt am Rahmen statt am Anzeigebereich.

Die Statusleiste ist auf `black` gestellt, nicht auf `black-translucent`:
Letzteres zeichnet ab der oberen Bildschirmkante, während iOS weiterhin einen
um die Statusleistenhöhe verkürzten Anzeigebereich meldet – die App endete
dadurch 59 Punkte über der Unterkante.

**Bewegung** ist knapp gehalten: Das Blatt fährt in 260 ms von unten herein,
nachgeladene Blöcke docken fertig an statt sich in der Höhe zu füllen, die
Liste setzt sich beim Ansichtswechsel. Nicht bei der Minuten-Aktualisierung.
Alles davon entfällt bei aktiviertem „Bewegung reduzieren".

Alle Farben hängen an acht Werten im `:root`-Block. Ein Thema ist damit ein
Austausch dieser acht Werte, ohne Eingriff ins Layout.

**Die Versionskennung `APP_VERSION` wird von Hand hochgezählt**, zusammen mit
dem Text im `#update`-Block. Ohne Build-Schritt geht das nicht automatisch,
und ein Hinweis, der Falsches meldet, wäre schlechter als keiner.

## Dateien

| Datei        | Zweck                                      |
|--------------|--------------------------------------------|
| `index.html` | Die komplette App (HTML, CSS, JavaScript)  |
| `icon.png`   | Home-Bildschirm-Icon (180 × 180 px)        |
