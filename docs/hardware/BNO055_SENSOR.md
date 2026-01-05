# BNO055 Sensor

> IMU-Sensor Details, Kalibrierung und Integration mit ESP32

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Der Adafruit BNO055 ist ein 9-DOF IMU-Sensor mit integrierter Sensor Fusion. Er liefert Euler-Winkel und Quaternions direkt, ohne zusätzliche Berechnungen auf dem ESP32.

## Voraussetzungen

- [ ] Adafruit BNO055 Breakout Board
- [ ] ESP32 mit I2C-Verbindung
- [ ] Adafruit BNO055 Arduino Library

## Inhalt

### Technische Spezifikationen

| Eigenschaft | Wert |
|-------------|------|
| Accelerometer | ±2g/±4g/±8g/±16g |
| Gyroscope | ±125/±250/±500/±1000/±2000 °/s |
| Magnetometer | ±1300 µT (x,y) / ±2500 µT (z) |
| Update Rate | 100 Hz |
| Interface | I2C (400 kHz) |
| I2C Adresse | 0x28 (default) oder 0x29 |

### Sensor Fusion

[Wird ergänzt: Beschreibung der internen Fusion]

### Kalibrierung

[Wird ergänzt: Kalibrierungsverfahren]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Sensor nicht erkannt | I2C-Adresse prüfen (0x28/0x29) |
| Kalibrierung verloren | Kalibrierungsdaten im EEPROM speichern |
| Drift nach Zeit | Magnetometer neu kalibrieren |

## Referenzen

- [Adafruit BNO055 Guide](https://learn.adafruit.com/adafruit-bno055-absolute-orientation-sensor)
- [BNO055 Datasheet](https://www.bosch-sensortec.com/products/smart-sensors/bno055/)

---

*Teil des [Neural Flight](../README.md) Projekts*
