# M1: Sensor Prototype

> ESP32 → Server Kommunikation

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026
**Zeitraum:** KW 1-2 (Januar 2026)

---

## Übersicht

Erster Meilenstein: ESP32 + BNO055 senden stabile Orientierungsdaten per WebSocket an den Server. Grundlage für alle weiteren Meilensteine.

## Erfolgskriterium

> Sensor in Hand bewegen → Werte in Browser-Console sichtbar

## Voraussetzungen

- [ ] Hardware beschafft (ESP32, BNO055, Breadboard, Kabel)
- [ ] Dedizierter Router eingerichtet
- [ ] Node.js Server läuft

## Tasks

- [ ] I2C-Verbindung ESP32 ↔ BNO055 testen
- [ ] WiFi-Konfiguration für dediziertes Netzwerk
- [ ] WebSocket-Client auf ESP32 implementieren
- [ ] JSON-Protokoll gemäß WEBSOCKET_PROTOCOL.md
- [ ] Server empfängt und loggt Telemetrie

## Technische Details

[Wird ergänzt: Code-Snippets, Pinout, Konfiguration]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| BNO055 nicht erkannt | I2C-Adresse 0x28/0x29 prüfen |
| WiFi verbindet nicht | 2.4 GHz Netzwerk testen |
| WebSocket-Fehler | Server-URL und Port prüfen |

## Referenzen

- [ESP32_SETUP.md](../01_hardware/ESP32_SETUP.md)
- [BNO055_SENSOR.md](../01_hardware/BNO055_SENSOR.md)
- [WEBSOCKET_PROTOCOL.md](../03_integration/WEBSOCKET_PROTOCOL.md)

---

*Teil des [Neural Flight](../README.md) Projekts*
