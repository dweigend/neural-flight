# Hardware-Einkaufsliste

> Alle Komponenten für den Sensor-Prototyp und die Entwicklungsumgebung

**Status:** 🟢 Complete  
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Diese Liste enthält alle Hardware-Komponenten für:
1. **Prototyp-Phase:** Breadboard-Aufbau zum Testen
2. **Production-Phase:** Robuste Installation am ICAROS

---

## 1. Kern-Komponenten (Prototyp)

### Microcontroller

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 1 | **ESP32 DevKit V1** | NodeMCU, WROOM-32, 38 Pins, WiFi+BT | AZ-Delivery | [Link](https://www.az-delivery.de/products/esp32-developmentboard) | ~10€ |

**Warum ESP32?**
- Integrierter WiFi + Bluetooth Stack
- Dual-Core 240 MHz (genug für WebSocket-Encryption)
- 512KB SRAM, 4MB Flash
- Arduino-kompatibel
- Günstig und gut dokumentiert

**Alternative:** ESP32-S3 (mehr GPIOs, USB-C) – für dieses Projekt Overkill.

---

### IMU-Sensor

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 2 | **Adafruit BNO055** | 9-DOF, I2C, Sensor Fusion, Cortex-M0 | BerryBase | [Link](https://www.berrybase.de/adafruit-9-dof-absolute-orientation-imu-fusion-breakout-bno055) | ~35€ |
| | *Alternative* | STEMMA QT Version (kein Löten) | Eckstein | [Link](https://eckstein-shop.de/Adafruit-9-DOF-Absolute-Orientation-IMU-Fusion-Breakout-BNO055-EN_1) | ~40€ |

**Warum BNO055 statt MPU6050?**

| Eigenschaft | BNO055 | MPU6050 |
|-------------|--------|---------|
| **Preis** | ~35€ | ~5€ |
| **Sensor Fusion** | ✅ Onboard (ARM Cortex-M0) | ❌ Software nötig |
| **Output** | Euler-Winkel, Quaternions | Raw Accel/Gyro |
| **Drift-Korrektur** | ✅ Automatisch | ❌ Manuell |
| **Kalibrierung** | ✅ Auto-Kalibrierung | ❌ Manuell |
| **Update Rate** | 100 Hz | 1000 Hz (aber raw) |
| **VR-Eignung** | ⭐⭐⭐⭐⭐ | ⭐⭐ |

Für VR ist die integrierte Sensor Fusion essentiell, um Motion Sickness zu vermeiden.

---

### Prototyping-Zubehör

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 3 | **Breadboard** | 830 Kontakte, Full-Size | AZ-Delivery | [Link](https://www.az-delivery.de/products/breadboard-830) | ~5€ |
| 4 | **Jumper-Kabel Set** | M-M, M-F, F-F, 40-pin je | BerryBase | [Link](https://www.berrybase.de/40-pin-jumper-kabel-set-m-m-m-w-w-w) | ~5€ |
| 5 | **USB-A auf Micro-USB** | ESP32 Programmierung, 1m | Amazon | Beliebig | ~3€ |

---

### Stromversorgung

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 6 | **USB-Powerbank** | 5V, 2A+, kompakt, <200g | Amazon | Beliebig | ~15€ |

**Anforderungen:**
- Mindestens 2A Output (ESP32 WiFi braucht Strom)
- Kompakt genug für Montage am ICAROS
- Keine Auto-Abschaltung bei geringem Verbrauch

---

## 2. Netzwerk-Infrastruktur

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 7 | **WiFi 6 Router** | 5GHz, AX1800+, dediziert | Amazon | [TP-Link Archer AX23](https://www.amazon.de/dp/B09CDHNB4Q) | ~50€ |
| 8 | **Ethernet-Kabel** | Cat6, 2m | Amazon | Beliebig | ~5€ |

**Warum dedizierter Router?**

| Problem | Lösung |
|---------|--------|
| Ausstellungs-WLAN überlastet | Eigenes Netzwerk |
| Latenz >50ms | Garantiert <20ms |
| Interferenz mit Besuchern | Isoliertes 5GHz Band |
| Debugging schwierig | Volle Kontrolle |

**Router-Konfiguration:**
- SSID: `ICAROS-Lab` (versteckt optional)
- Channel: Fester 5GHz Kanal (nicht Auto)
- Kein Internet nötig
- DHCP: 192.168.10.x

---

## 3. Quest 3 Development

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 9 | **USB-C Kabel** | Quest 3 ADB, 3m, Daten+Laden | Amazon | AmazonBasics | ~10€ |
| 10 | **USB-C Hub** | Für Laptop ohne USB-A | Amazon | Beliebig | ~15€ |

**Quest 3 ist vorhanden im Futurium Lab.**

---

## 4. Production-Komponenten (später)

Für die finale Installation am ICAROS:

| # | Komponente | Spezifikation | Händler | Link | Preis |
|---|------------|---------------|---------|------|-------|
| 11 | **Raspberry Pi 5** | 8GB RAM, als Server | BerryBase | [Link](https://www.berrybase.de/raspberry-pi-5-8gb-ram) | ~80€ |
| 12 | **Pi 5 Netzteil** | 27W USB-C | BerryBase | [Link](https://www.berrybase.de/offizielles-raspberry-pi-27w-usb-c-netzteil) | ~15€ |
| 13 | **MicroSD** | 64GB, A2 | Amazon | SanDisk Extreme | ~15€ |
| 14 | **Gehäuse ESP32** | 3D-Druck oder Hammond | - | Eigenfertigung | ~10€ |
| 15 | **Kabelverschraubung** | M12, für Sensor-Kabel | Conrad | Beliebig | ~5€ |
| 16 | **Klett-Klebeband** | Montage am ICAROS | Amazon | Dual-Lock | ~10€ |

---

## 5. Werkzeug (falls nicht vorhanden)

| # | Werkzeug | Zweck | Preis |
|---|----------|-------|-------|
| 17 | Lötkolben + Lötzinn | Header an Boards | ~30€ |
| 18 | Multimeter | Debugging | ~20€ |
| 19 | Schraubendreher-Set | Montage | ~15€ |

---

## Kostenübersicht

### Prototyp-Phase

| Kategorie | Komponenten | Summe |
|-----------|-------------|-------|
| Kern | ESP32, BNO055 | ~45€ |
| Prototyping | Breadboard, Kabel | ~13€ |
| Strom | Powerbank | ~15€ |
| Netzwerk | Router, Kabel | ~55€ |
| Quest Dev | USB-C Kabel | ~10€ |
| **Prototyp Gesamt** | | **~138€** |

### Production-Phase (zusätzlich)

| Kategorie | Komponenten | Summe |
|-----------|-------------|-------|
| Server | Pi 5, Netzteil, SD | ~110€ |
| Gehäuse | 3D-Druck, Verschraubung | ~15€ |
| Montage | Klett, etc. | ~10€ |
| **Production Zusatz** | | **~135€** |

### Gesamt

| Phase | Kosten |
|-------|--------|
| Prototyp | ~140€ |
| + Production | +135€ |
| **Total** | **~275€** |

---

## Bezugsquellen Deutschland

| Händler | Spezialisierung | Lieferzeit | Website |
|---------|-----------------|------------|---------|
| **AZ-Delivery** | ESP32, Arduino, Sensoren | 1-3 Tage | [az-delivery.de](https://www.az-delivery.de) |
| **BerryBase** | Raspberry Pi, Adafruit | 1-3 Tage | [berrybase.de](https://www.berrybase.de) |
| **Eckstein-Shop** | Adafruit offiziell | 2-4 Tage | [eckstein-shop.de](https://eckstein-shop.de) |
| **Reichelt** | Elektronik allgemein | 1-2 Tage | [reichelt.de](https://www.reichelt.de) |
| **Conrad** | Profi-Elektronik | 1-2 Tage | [conrad.de](https://www.conrad.de) |
| **Amazon** | Alles, schnell | 1-2 Tage | [amazon.de](https://www.amazon.de) |

---

## Pinout-Referenz

### ESP32 ↔ BNO055 Verbindung

```
ESP32 DevKit V1          BNO055 Breakout
┌──────────────┐         ┌──────────────┐
│              │         │              │
│    3V3  ─────┼─────────┼─── VIN      │
│    GND  ─────┼─────────┼─── GND      │
│    GPIO21 ───┼─────────┼─── SDA      │
│    GPIO22 ───┼─────────┼─── SCL      │
│              │         │              │
└──────────────┘         └──────────────┘
```

**Wichtig:**
- 3.3V verwenden (nicht 5V!)
- I2C Adresse: 0x28 (default) oder 0x29
- Keine externen Pull-ups nötig (auf Breakout vorhanden)

---

## Checkliste Bestellung

- [ ] ESP32 DevKit V1 (38 Pin)
- [ ] Adafruit BNO055 Breakout
- [ ] Breadboard 830
- [ ] Jumper-Kabel Set
- [ ] USB Micro-USB Kabel
- [ ] USB Powerbank
- [ ] WiFi 6 Router
- [ ] Ethernet-Kabel
- [ ] USB-C Kabel 3m

---

## Referenzen

- [Adafruit BNO055 Guide](https://learn.adafruit.com/adafruit-bno055-absolute-orientation-sensor)
- [ESP32 Pinout Reference](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
- [ICAROS Health Specs](https://www.icaros.com/de/produkte/icaros-health)

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
