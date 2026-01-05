# ESP32 Arduino Guide

> Embedded Development Guide für die Telemetrie-Unit

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Entwicklungsanleitung für den ESP32-Code, der BNO055-Sensordaten liest und per WebSocket an den Server sendet.

## Voraussetzungen

- [ ] PlatformIO oder Arduino IDE installiert
- [ ] ESP32 DevKit V1
- [ ] BNO055 Sensor angeschlossen

## Inhalt

### PlatformIO Setup

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
lib_deps =
    adafruit/Adafruit BNO055@^1.6.3
    links2004/WebSockets@^2.4.1
    bblanchon/ArduinoJson@^7.0.0
```

### Code-Struktur

[Wird ergänzt: main.cpp Aufbau]

### WiFi-Verbindung

[Wird ergänzt: Reconnection-Logik]

### WebSocket-Client

[Wird ergänzt: JSON-Serialisierung]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Kompilierung fehlgeschlagen | Library-Versionen prüfen |
| I2C-Fehler | SDA/SCL Pins prüfen (21/22) |
| WebSocket-Disconnect | Heartbeat implementieren |

## Referenzen

- [PlatformIO ESP32](https://docs.platformio.org/en/latest/platforms/espressif32.html)
- [ArduinoJson Assistant](https://arduinojson.org/v7/assistant/)

---

*Teil des [Neural Flight](../README.md) Projekts*
