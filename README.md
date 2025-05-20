# Node-RED Mini-Display Projekt

## Projektbeschreibung
Dieses Node-RED Projekt steuert ein 2x16 Zeichen LCD-Display mit zwei Tasten, um zwischen verschiedenen Mini-Anwendungen umzuschalten und zu interagieren. Das System bietet mehrere nützliche Funktionen, die über die beiden Tasten bedient werden können.

## Funktionen
Das Projekt umfasst folgende Module:

1. **Projekt-Switcher**
   - Mit den 2 Schalter können Sie zwischen den verschiedenen Projekten wechseln
  
2. **Stockmarket-Anzeige**
   - Zeigt simulierte Aktienkurse an
   - Buttons zum Umschalten zwischen verschiedenen Aktien

3. **Mini-Quiz**
   - Einfaches Ja/Nein-Quiz mit verschiedenen Fragen
   - Button 1: Auswahl Ja
   - Button 2: Auswahl Nein
   - Automatisches weiter wenn Ja oder Nein gemacht

4. **Tageszeit-Anzeige**
   - Zeigt aktuelle Uhrzeit und Datum an
   - Button 1: Wechsel zwischen verschiedenen Zeitregionen
   - Button 2: Zusatzinformationen anzeigen (Sekunden, Wochentag)

5. **Wetter-Simulation**
   - Zeigt Wetterdaten an
   - Buttons zum Umschalten zwischen verschiedenen Wetterwerten

## Hardware-Anforderungen
- Node-RED Server/Raspberry Pi
- 2x16 LCD-Display (kompatibel mit HD44780 oder I2C)
- 2 Taster/Buttons
- 2 Schalter/Lever
- Jumper-Kabel und Breadboard
- Stromversorgung

## Installation
1. Installieren Sie Node-RED falls noch nicht geschehen
2. Importieren Sie den Flow aus der beigefügten JSON-Datei
3. Installieren Sie die folgenden Node-RED-Pakete:
   - node-red-
   - node-red-node-pi-gpio (für Tasteneingang auf Raspberry Pi)
4. Verbinden Sie das LCD-Display und die Taster entsprechend des Schaltplans
5. Starten Sie den Flow


- **Taster:**
  - Button 1: GPIO  (und GND)
  - Button 2: GPIO  (und GND)

- **Schalter**
  - Schalter 1: GPIO  (und GND)
  - Schalter 2: GPIO  (und GND)

## Verwendung
1. Nach dem Start befindet sich das System im Projekt-Switcher
2. Wenn Schalter 1 an 
3. Wenn Schalter 2 an
4. Wenn Schalter 1 & 2 an
5. Wenn kein Schalter  an
6. Innerhalb jedes Projekts haben die Buttons spezifische Funktionen

