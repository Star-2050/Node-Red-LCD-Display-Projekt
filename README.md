# Node-RED LCD Display System

Ein vielseitiges LCD-Display-System basierend auf Node-RED für Raspberry Pi, das verschiedene Informationen anzeigt und durch Schalter und Buttons gesteuert wird.

## 🚀 Features

Das System bietet vier verschiedene Anzeigemodi:

### 1. **Uhrzeit-Modus** ⏰
- Zeigt aktuelles Datum und Uhrzeit an
- Automatische Sekunden-Updates
- Deutsche Datumsformatierung
- Format: "Datum : DD.MM.YYYY" und "HH:MM:SS"

### 2. **Wetter-Modus** 🌤️ 
- Live-Wetterdaten für Luxembourg über OpenWeatherMap API
- Anzeige von:
  - Temperatur (°C)
  - Luftfeuchtigkeit (%)
  - Windgeschwindigkeit (km/h)
  - Wetterbeschreibung
- Automatische Updates alle 5 Minuten
- Manuelle Umschaltung zwischen Wetter-Anzeigen mit Button 2
- Fehlerbehandlung mit Recovery-System (stündliche Überprüfung)

### 3. **Aktien-Modus** 📈
- Live-Aktienkurse über Finnhub API
- Unterstützte Aktien: AAPL, MSFT, GOOGL, AMZN, TSLA
- Anzeige von:
  - Aktueller Kurs in USD
  - Kursänderung mit Pfeil-Indikator (^ für steigend, v für fallend)
  - Absolute und prozentuale Änderung
- Automatische Rotation durch verschiedene Aktien alle 10 Minuten
- Manuelle Umschaltung mit Button 2
- Drei Display-Modi: Preis, Änderung, Vollansicht

### 4. **Kryptowährungs-Modus** 💰
- Live-Kryptowährungskurse über CoinGecko API
- Unterstützte Kryptowährungen: Bitcoin (BTC), Ethereum (ETH), Cardano (ADA), Polkadot (DOT), Chainlink (LINK)
- Anzeige von:
  - Aktueller Kurs in USD
  - 24h-Kursänderung mit Pfeil-Indikator
  - Prozentuale 24h-Änderung
- Automatische Rotation durch verschiedene Kryptowährungen alle 5 Minuten
- Manuelle Umschaltung mit Button 2
- Drei Display-Modi: Preis, Änderung, Vollansicht

## 🕹️ Bedienung

### Hardware-Komponenten
- **2 Schalter** (GPIO 20 & 21): Modusauswahl
- **2 Buttons** (GPIO 16 & 19): Interaktion
- **LCD-Display**: Anzeige der Informationen

### Schalter-Kombinationen

| Schalter 1 (GPIO 21) | Schalter 2 (GPIO 20) | Modus |
|------------|------------|-------|
| AUS | AUS | **Uhrzeit** ⏰ |
| EIN | AUS | **Wetter** 🌤️ |
| AUS | EIN | **Aktien** 📈 |
| EIN | EIN | **Kryptowährungen** 💰 |

### Button-Funktionen

#### Button 2 (GPIO 16)
- **Im Wetter-Modus**: Zyklisches Wechseln zwischen Wetter-Anzeigen
  - Temperatur → Luftfeuchtigkeit → Windgeschwindigkeit → Wetterbeschreibung → (Wiederholung)
- **Im Aktien-Modus**: Manueller Wechsel zum nächsten Aktiensymbol
- **Im Kryptowährungs-Modus**: Manueller Wechsel zur nächsten Kryptowährung

#### Button 1 (GPIO 19)
- Aktuell nicht implementiert (für zukünftige Erweiterungen vorgesehen)

## ⚙️ Technische Details

### Automatische Updates
- **Wetter**: Alle 5 Minuten (300 Sekunden)
- **Aktien**: Alle 10 Minuten (600 Sekunden)
- **Kryptowährungen**: Alle 5 Minuten (300 Sekunden)
- **Uhrzeit**: Jede Sekunde (nur im Uhrzeit-Modus aktiv)
- **Fehlerüberwachung**: Stündliche Überprüfung der letzten Updates

### APIs verwendet
- **OpenWeatherMap**: Wetterdaten für Luxembourg
- **Finnhub**: Aktienkurse und Marktdaten
- **CoinGecko**: Kryptowährungskurse und 24h-Änderungen

### GPIO-Belegung
```
GPIO 16 (Pin 36): Button 2 (Pull-Down, Reaktion bei HIGH)
GPIO 19 (Pin 35): Button 1 (Pull-Down, Reaktion bei HIGH)
GPIO 20 (Pin 38): Schalter 2 (Pull-Up, invertiert)
GPIO 21 (Pin 40): Schalter 1 (Pull-Up, invertiert)
```

### LCD-Steuerung
Das System verwendet Python-Skripte für die LCD-Kommunikation:
- `init.py`: Initialisierung und Clearing
- `write.py`: Schreiben von Text in spezifische Zeilen (--line 1/2 --message "Text")

## 🔧 Installation & Setup

### Voraussetzungen
- Raspberry Pi mit Node-RED installiert
- LCD-Display (16x2 oder ähnlich)
- Python-Umgebung für LCD-Skripte
- GPIO-Zugriff aktiviert

### Node-RED Packages
Erforderliche Node-RED-Knoten:
- `node-red-node-pi-gpio`: GPIO-Steuerung
- `node-red-contrib-http-request`: API-Aufrufe (falls nicht bereits vorhanden)
- Standard Node-RED-Knoten (function, inject, debug, exec, etc.)

### Konfiguration

1. **API-Schlüssel einrichten**:
   - OpenWeatherMap API-Key
   - Finnhub API-Token
   - CoinGecko: Keine API-Key erforderlich (kostenlose API)

2. **Pfade anpassen** (falls abweichend):
   ```
   /home/star2050/digilab/lcd/init.py
   /home/star2050/digilab/lcd/write.py
   ```

3. **GPIO-Pins konfigurieren** (falls abweichend von Standard)

## Flow-Struktur

### Hauptkomponenten

#### 1. Wetter-Datenbereich
- Automatischer 5-Minuten-Timer (`2bc2d46292aae71f`)
- HTTP-Request zu OpenWeatherMap (`b3cfd559401f9bfa`)
- Datenextraktion und -formatierung (`733d598a4f7b149d`)
- Fehlerbehandlung mit stündlicher Recovery (`33bac39479946e4c`)

#### 2. LCD-Display-Bereich
- Zentraler Display-Handler (`1996b9100a3bb7f4`)
- Modus-basierte Anzeige-Logik
- Text-Bereinigung für LCD-Kompatibilität
- Zeilen-spezifische Ausgabe mit Delay für synchrone Darstellung

#### 3. Schalter-Eingabe-Bereich
- GPIO-Eingabe-Verarbeitung (`789c0aec48a6fb3d`, `3c5df00822c9a005`)
- Pull-Up-Resistor-Logik (invertiert) (`b63af0586ec5ebf7`)
- Modus-Bestimmung basierend auf Schalter-Kombinationen
- Debouncing (50ms) integriert

#### 4. Aktien-Datenbereich
- Automatischer 10-Minuten-Timer (`0f60d230ee7f1d8d`)
- Dynamische Symbol-Rotation (`f15478bfc1c3599a`)
- Finnhub API-Integration (`a269df6103c55b01`)
- Formatierung für LCD-Anzeige (`ae68e326a10a4a39`)

#### 5. Kryptowährungs-Datenbereich
- Automatischer 5-Minuten-Timer (`crypto_timer_inject`)
- CoinGecko API-Integration (`crypto_http_request`)
- Automatische Rotation durch 5 Kryptowährungen (`crypto_process_data`)
- Manuelle Umschaltung per Button (`crypto_manual_button`)

#### 6. Uhrzeit-Bereich
- Sekündlicher Update-Timer (`fast_clock_update`)
- Bedingte Aktualisierung nur im Uhrzeitmodus (`clock_mode_check`)

## 🐛 Debugging & Monitoring

### Debug-Ausgaben
Das System enthält umfangreiche Debug-Knoten für:
- API-Antworten (Wetter, Aktien & Kryptowährungen)
- Button-Drücke und Modus-Wechsel
- LCD-Kommandos und Skript-Ausführung
- Fehlerbehandlung und Recovery-Mechanismen

### Statusüberwachung
- Automatische Überwachung der letzten Updates (20-Minuten-Timeout)
- Recovery-Mechanismen bei API-Fehlern
- Fallback-Anzeigen bei fehlenden Daten
- Dummy-Werte für Testzwecke

## Fehlerbehebung

### Häufige Probleme

**LCD zeigt nichts an**:
- Überprüfen Sie die Python-Skript-Pfade (`/home/star2050/digilab/lcd/`)
- Testen Sie die GPIO-Verbindungen
- Prüfen Sie die LCD-Stromversorgung
- Überprüfen Sie die Skript-Berechtigungen

**Wetter-/Aktien-/Kryptodaten nicht verfügbar**:
- API-Schlüssel überprüfen (bereits im Code hinterlegt)
- Internetverbindung testen
- Debug-Ausgaben in Node-RED kontrollieren
- Recovery-Timer abwarten (stündlich)

**Buttons reagieren nicht**:
- GPIO-Pin-Belegung überprüfen (GPIO 16 & 19)
- Pull-Down-Konfiguration prüfen
- Debounce-Zeiten anpassen (aktuell 100ms)
- Flow-Kontext-Variablen in Debug-Panel überprüfen

**Schalter-Modi funktionieren nicht**:
- Überprüfen Sie die Schalter-Verdrahtung (GPIO 20 & 21)
- Pull-Up-Resistor-Logik beachten (invertiert: 0=EIN, 1=AUS)
- Flow-Kontext-Variablen in Debug-Panel überprüfen
- Debounce-Zeit anpassen (aktuell 50ms)

### Text-Darstellungsprobleme
- Das System bereinigt automatisch HTML-Entities und Sonderzeichen
- Maximale Zeilenlänge: 16 Zeichen
- Unterstützte Sonderzeichen: °, %, $, ^, v

## Erweiterungsmöglichkeiten

### Einfache Erweiterungen
- Weitere Aktien-Symbole zur Rotation hinzufügen
- Zusätzliche Kryptowährungen integrieren
- Neue Wetter-Parameter einbinden
- Weitere Display-Modi entwickeln

### Erweiterte Features
- Button 1 (GPIO 19) implementieren
- Smartphone-App für Remote-Steuerung
- Datenlogging und Historien-Anzeige
- Custom-Animationen für das LCD
- Alarm-Funktionen bei bestimmten Kursschwellen
- Integration weiterer APIs (z.B. News, Sport)

### Technische Verbesserungen
- WebSocket-Integration für Echtzeit-Updates
- OLED-Display-Unterstützung
- Touch-Screen-Integration
- WiFi-Konfiguration über Web-Interface

## Flow-Statistiken

- **Gesamt-Knoten**: ~50 Knoten
- **Timer-Knoten**: 5 (Wetter, Aktien, Krypto, Uhrzeit, Recovery)
- **GPIO-Knoten**: 4 (2 Schalter, 2 Buttons)
- **HTTP-Request-Knoten**: 3 (OpenWeatherMap, Finnhub, CoinGecko)
- **Function-Knoten**: ~15 (Datenverarbeitung und Logik)
- **Debug-Knoten**: ~10 (Monitoring und Fehlerbehebung)

## API-Informationen

### OpenWeatherMap
- **URL**: `https://api.openweathermap.org/data/2.5/weather`
- **Parameter**: `q=Luxembourg&units=metric`
- **Update-Intervall**: 5 Minuten

### Finnhub
- **URL**: `https://finnhub.io/api/v1/quote`
- **Parameter**: `symbol={SYMBOL}`
- **Update-Intervall**: 10 Minuten

### CoinGecko
- **URL**: `https://api.coingecko.com/api/v3/simple/price`
- **Parameter**: `ids=bitcoin,ethereum,cardano,polkadot,chainlink&vs_currencies=usd&include_24hr_change=true`
- **Update-Intervall**: 5 Minuten

## Lizenz

Dieses Projekt steht unter der MIT-Lizenz zur freien Verfügung.

---

**Entwickelt für Node-RED auf Raspberry Pi** 🍓

*Letzte Aktualisierung: Juni 2025*