# ESP32 Setup

> Pinout, Konfiguration und Troubleshooting für den ESP32 DevKit V1

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Diese Dokumentation beschreibt die Einrichtung und Konfiguration des ESP32 Microcontrollers für die Telemetrie-Unit der ICAROS WebXR Integration.

## Voraussetzungen

- [ ] ESP32 DevKit V1 (WROOM-32, 38 Pins)
- [ ] USB Micro-USB Kabel
- [ ] Arduino IDE oder PlatformIO installiert

## Inhalt

### Hardware-Spezifikationen

| Eigenschaft | Wert |
|-------------|------|
| Chip | ESP32-WROOM-32 |
| CPU | Dual-Core Xtensa LX6 @ 240 MHz |
| RAM | 520 KB SRAM |
| Flash | 4 MB |
| WiFi | 802.11 b/g/n |
| Bluetooth | v4.2 BR/EDR + BLE |

### Pinout

[Wird ergänzt: Pinout-Diagramm und GPIO-Zuordnung]

### Firmware-Upload

[Wird ergänzt: Schritt-für-Schritt Anleitung]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| ESP32 wird nicht erkannt | CP2102/CH340 Treiber installieren |
| Upload fehlgeschlagen | BOOT-Button während Upload gedrückt halten |
| WiFi verbindet nicht | 2.4 GHz Netzwerk verwenden |

## Referenzen

- [ESP32 Pinout Reference](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
- [ESP32 Arduino Core](https://docs.espressif.com/projects/arduino-esp32)

---

*Teil des [Neural Flight](../README.md) Projekts*
