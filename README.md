# THM Hörsaalkapazitäten

Verwaltung der Hörsaal- und Raumkapazitäten der Technischen Hochschule Mittelhessen. Dieses Repository enthält strukturierte Daten zu Sitzplatzkapazitäten für Vorlesungen und Klausuren.

## Übersicht

Die Daten umfassen 41 Hörsäle und Seminarräume mit detaillierten Informationen zu:
- Sitzplatzkapazitäten für reguläre Vorlesungen
- Sitzplatzkapazitäten für Klausuren (mit Abstandsregelungen)
- Besondere Ausstattungsmerkmale (PC-Arbeitsplätze, Medienwände, etc.)
- Raumspezifische Hinweise und Einschränkungen

## Verfügbare Formate

- **JSON**: `hoersaal_kapazitaeten.json` - Für programmatische Verarbeitung
- **YAML**: `hoersaal_kapazitaeten.yaml` - Für bessere Lesbarkeit und manuelle Bearbeitung

## Datenstruktur

### Hörsaal-Objekt
```yaml
raumnummer: A1.0.01
sitzplaetze_vorlesung: 139
bemerkung_vorlesung: "+ 15 Klappstühle ohne Tische"
sitzplaetze_klausur: 37
bemerkung_klausur: "Klapptische, Stufen. Aufsteigendes Gestühl..."
```

### Felder

| Feld | Typ | Beschreibung |
|------|-----|--------------|
| `raumnummer` | String | Eindeutige Raumbezeichnung (z.B. A1.0.01, B2.U1.02) |
| `sitzplaetze_vorlesung` | Integer | Anzahl verfügbarer Plätze für Vorlesungen |
| `bemerkung_vorlesung` | String | Besonderheiten zur Ausstattung (PC-Plätze, kombinierbare Räume, etc.) |
| `sitzplaetze_klausur` | Integer/null | Anzahl verfügbarer Plätze für Klausuren (mit Abstandsregelung) |
| `bemerkung_klausur` | String | Hinweise zu Klausur-Bestuhlung und Einschränkungen |


## Verwendungsbeispiele

### Python
```python
import json

with open('hoersaal_kapazitaeten.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

# Alle Hörsäle mit mehr als 100 Plätzen finden
grosse_hoersaele = [
    raum for raum in data['hoersaele'] 
    if raum['sitzplaetze_vorlesung'] > 100
]

# Gesamtkapazität berechnen
total_plaetze = sum(raum['sitzplaetze_vorlesung'] for raum in data['hoersaele'])
```

### JavaScript/Node.js
```javascript
const fs = require('fs');

const data = JSON.parse(fs.readFileSync('hoersaal_kapazitaeten.json', 'utf8'));

// Räume mit PC-Arbeitsplätzen filtern
const pcRaeume = data.hoersaele.filter(raum => 
  raum.bemerkung_vorlesung.includes('PC-Arbeitsplätze')
);
```

### Bash/jq
```bash
# Alle Raumnummern ausgeben
jq -r '.hoersaele[].raumnummer' hoersaal_kapazitaeten.json

# Räume sortiert nach Klausurkapazität
jq '.hoersaele | sort_by(.sitzplaetze_klausur) | reverse' hoersaal_kapazitaeten.json
```

## Besonderheiten

### Kombinierbare Räume

Einige Räume können zu größeren Einheiten verbunden werden:
- **A1.1.01 + A1.1.02**: 50 + 50 = 100 Plätze
- **A8.0.01A + A8.0.01B**: 36 + 36 = 72 Plätze
- **A8.0.03A + A8.0.03B**: 36 + 36 = 72 Plätze
- **A8.1.06A + A8.1.06B**: 36 + 36 = 72 Plätze

### Klausurkapazitäten

Bei Hörsälen mit aufsteigendem Gestühl gelten besondere Regelungen:
> Für Klausuren kann nur jede 2. Reihe und darin höchstens jeder 2. Platz besetzt werden.

Dies führt zu deutlich reduzierten Klausurkapazitäten gegenüber der Vorlesungsbestuhlung.

### PC-Räume

Folgende Räume verfügen über PC-Arbeitsplätze und sind primär für IT-Veranstaltungen vorgesehen:
- A2.0.07 (18 Plätze)
- A4.1.14 (51 Plätze, versenkte PCs)
- A4.1.17 (16 PC-Arbeitsplätze)
- A4.1.18 (24 PC-Arbeitsplätze, geplant: 48 nach Umbau)
- B1.2.06 (20 PC-Arbeitsplätze)
- B1.2.07 (27 PC-Arbeitsplätze)

## Aktualisierung

Stand der Daten: **12.03.2019** (Neuauszählung D. Baums)

Bei Änderungen an Raumkapazitäten oder -ausstattung:
1. YAML-Datei aktualisieren (besser lesbar für manuelle Änderungen)
2. JSON-Datei entsprechend anpassen oder neu generieren
3. Stand-Datum in den Metadaten aktualisieren

## Nutzung

Diese Daten können verwendet werden für:
- Automatisierte Raumbuchungssysteme
- Vorlesungsplanung und Stundenplanerstellung
- Klausurplanung mit Kapazitätsberechnung
- Raumauslastungsanalysen
- Integration in Campus-Management-Systeme

## Lizenz

Die Daten werden von der Technischen Hochschule Mittelhessen zur Verfügung gestellt. 
