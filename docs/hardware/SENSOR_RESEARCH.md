# Sensor-Recherche: Orientierungstracking für Motion Platforms

> Umfassende Recherche zu IMU-Sensoren, Protokollen und DIY-Lösungen

**Status:** 🟢 Complete
**Zuletzt aktualisiert:** 2026-01-05

---

## 1. Problemstellung

### Was wollen wir messen?

Der ICAROS ist eine mechanische Plattform, die sich in **3 Achsen** bewegen kann:
- **Pitch** (Nick): Vorwärts/Rückwärts-Neigung
- **Roll**: Seitliche Neigung (links/rechts)
- **Yaw** (Gier): Rotation um die vertikale Achse (begrenzt)

Wir brauchen diese Bewegungsdaten in Echtzeit, um sie an eine VR-Anwendung zu senden.

### Anforderungen

| Anforderung | Zielwert | Grund |
|-------------|----------|-------|
| **Latenz** | <20ms End-to-End | VR-Komfort, Motion Sickness |
| **Update-Rate** | 50-100 Hz | Flüssige Animation |
| **Genauigkeit** | ±1-2° | Intuitive Steuerung |
| **Drift** | Minimal über 5 min | Session-Dauer |
| **Kosten** | <50€ pro Sensor | Budget-freundlich |

---

## 2. Sensortypen im Überblick

### A. IMU-Sensoren (Inertial Measurement Unit)

IMUs kombinieren mehrere Sensoren:
- **Accelerometer**: Misst Beschleunigung (inkl. Schwerkraft)
- **Gyroscope**: Misst Rotationsgeschwindigkeit
- **Magnetometer**: Misst Erdmagnetfeld (Kompass)

**Vorteile:**
- Kompakt, günstig, selbstständig
- Keine externe Infrastruktur nötig
- Direkt auf der Plattform montierbar

**Nachteile:**
- Drift über Zeit (Gyro-Bias)
- Magnetische Störungen (Metall, Elektronik)
- Nur Orientierung, keine absolute Position

### B. Optisches Tracking

Externe Kameras verfolgen Marker oder erkennen Objekte.

**Beispiele:**
- TrackIR (IR-Kamera + Reflektoren, ~150€)
- Webcam + AI (OpenTrack, kostenlos)
- Vive Tracker (Steam Lighthouse, ~200€)

**Vorteile:**
- Keine Drift
- Absolute Position möglich
- Höhere Präzision

**Nachteile:**
- Sichtlinie erforderlich
- Setup-Aufwand
- Teurer

### Empfehlung für unser Projekt

**IMU-basiert** ist die beste Wahl:
- Einfache Montage direkt am ICAROS
- Keine externe Infrastruktur im Ausstellungsraum
- Kostengünstig für Prototyping
- Bei kurzen Sessions (2-5 min) ist Drift unkritisch

---

## 3. Sensor-Vergleich

### Übersicht

| Sensor | DoF | Fusion | Preis | Drift | Empfehlung |
|--------|-----|--------|-------|-------|------------|
| **BNO085** | 9 | ✅ Onboard | ~25€ | Sehr niedrig | ⭐ Beste Wahl |
| **ICM-20948** | 9 | ✅ DMP | ~8€ | Niedrig | ⭐ Preis-Leistung |
| **BNO055** | 9 | ✅ Onboard | ~35€ | Mittel | Okay, veraltet |
| **LSM6DSO** | 6 | ❌ Extern | ~5€ | Hoch | Nur mit Filter |
| **MPU6050** | 6 | ❌ Extern | ~3€ | Sehr hoch | ❌ Nicht empfohlen |

### Detailvergleich

#### BNO085 (Bosch) – Top-Empfehlung

```
Hersteller: Bosch Sensortec
DoF: 9 (Accel + Gyro + Mag)
Fusion: Integriert (ARM Cortex-M0)
Output: Quaternion, Euler, Linear Accel
I2C-Adresse: 0x4A / 0x4B
Update-Rate: bis 400 Hz (Fusion: ~100 Hz)
Genauigkeit: <1° RMS
```

**Pro:**
- Beste Fusion-Qualität
- Sehr niedriger Drift
- Production-ready

**Contra:**
- Teurer als Alternativen
- Verfügbarkeit manchmal eingeschränkt

#### ICM-20948 (TDK InvenSense) – Preis-Leistungs-Sieger

```
Hersteller: TDK InvenSense
DoF: 9 (Accel + Gyro + Mag)
Fusion: DMP (Digital Motion Processor)
Output: Quaternion, Raw Data
I2C-Adresse: 0x68 / 0x69
Update-Rate: bis 1100 Hz raw, ~100 Hz fused
Genauigkeit: ~2° RMS Heading
```

**Pro:**
- Günstiger als BNO085
- Gute Fusion-Qualität
- Niedriger Stromverbrauch

**Contra:**
- Komplexeres Setup
- Split-Rail Spannung (3.3V/1.8V)

#### BNO055 (Bosch) – Der Klassiker

```
Hersteller: Bosch Sensortec
DoF: 9 (Accel + Gyro + Mag)
Fusion: Integriert (ARM Cortex-M0)
Output: Quaternion, Euler
I2C-Adresse: 0x28 / 0x29
Update-Rate: bis 100 Hz
Gyro Range: ±2000°/s
```

**Pro:**
- Einfachstes Setup (Plug & Play)
- Viele Tutorials verfügbar
- Quaternion-Output direkt nutzbar

**Contra:**
- Veraltet, höherer Drift als Nachfolger
- Magnetometer empfindlich gegen Störungen

#### MPU6050 – Nicht empfohlen

```
DoF: 6 (nur Accel + Gyro, kein Magnetometer)
Fusion: Keine (extern erforderlich)
Preis: ~3€
```

**Warum nicht:**
- Kein Magnetometer → starker Yaw-Drift
- Keine integrierte Fusion
- Hohe Ausfallrate bei billigen Klonen
- Veraltet (2012)

### Bezugsquellen Deutschland

| Händler | Sensoren | Lieferzeit | Link |
|---------|----------|------------|------|
| **Berrybase** | BNO055, ICM-20948 | 1-3 Tage | [berrybase.de](https://www.berrybase.de) |
| **AZ-Delivery** | BNO055, MPU6050 | 1-3 Tage | [az-delivery.de](https://www.az-delivery.de) |
| **Reichelt** | ICM-20948 | 1-2 Tage | [reichelt.de](https://www.reichelt.de) |
| **Eckstein-Shop** | Adafruit Original | 2-4 Tage | [eckstein-shop.de](https://eckstein-shop.de) |
| **Mouser/Digikey** | BNO085 | 3-5 Tage | [mouser.de](https://www.mouser.de) |

---

## 4. Datenformate für 3D-Engines

### Quaternionen vs. Euler-Winkel

| Aspekt | Quaternion | Euler |
|--------|------------|-------|
| **Format** | (w, x, y, z) | (x, y, z) in Grad |
| **Gimbal Lock** | ❌ Kein Problem | ⚠️ Bei ±90° Pitch |
| **Interpolation** | ✅ Smooth (SLERP) | ⚠️ Springt |
| **Verständlichkeit** | Abstrakt | Intuitiv |

**Empfehlung:** Quaternionen für die Übertragung, Euler für Debugging.

### Engine-spezifische Formate

#### Three.js

```javascript
// Quaternion-Reihenfolge: (x, y, z, w)
object.quaternion.set(x, y, z, w);

// Euler-Reihenfolge: 'XYZ' (Standard)
object.rotation.set(x, y, z, 'XYZ');

// Wichtig: Three.js ist Y-up, rechtshändig
```

#### Godot 4

```gdscript
# Quaternion: Quat(x, y, z, w)
transform.basis = Basis(Quaternion(x, y, z, w))

# Euler: Vector3 in Radians
rotation = Vector3(pitch, yaw, roll)

# Godot ist Y-up, rechtshändig
```

#### Unity

```csharp
// Quaternion: new Quaternion(x, y, z, w)
transform.rotation = new Quaternion(x, y, z, w);

// Unity ist Y-up, linkshändig (!)
// Achsen-Konvertierung nötig: z = -z
```

### JSON-Payload Beispiel

```json
{
  "type": "orientation",
  "timestamp": 1704067200000,
  "quaternion": {
    "w": 0.9239,
    "x": 0.0,
    "y": 0.3827,
    "z": 0.0
  },
  "euler": {
    "pitch": 0.0,
    "roll": 0.0,
    "yaw": 45.0
  },
  "calibration": {
    "status": "calibrated",
    "accuracy": 3
  }
}
```

---

## 5. Übertragungsprotokolle

### Vergleich

| Protokoll | Latenz | Use Case | Komplexität |
|-----------|--------|----------|-------------|
| **WebSocket** | 5-15ms | Web, VR | Niedrig |
| **OSC** | <5ms | Audio/VJ | Mittel |
| **MQTT** | 10-30ms | IoT, Multi-Device | Mittel |
| **Serial** | <1ms | Desktop, Wired | Sehr niedrig |
| **BLE** | 10-50ms | Mobile, Wireless | Hoch |

### Empfehlung: WebSocket

**Warum WebSocket?**
- Native Browser-Unterstützung (Three.js, WebXR)
- Bidirektional
- ESP32-Libraries verfügbar
- Gut dokumentiert

**Typische Architektur:**

```
ESP32 + IMU
    │
    │ WiFi (5GHz)
    ▼
┌─────────────────┐
│ WebSocket Server│
│ (Node.js/Python)│
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
Three.js   Godot
(Browser)  (Desktop)
```

### ESP32 WebSocket Code (Beispiel)

```cpp
#include <WebSocketsServer.h>
#include <ArduinoJson.h>

WebSocketsServer webSocket = WebSocketsServer(81);

void sendOrientation() {
  StaticJsonDocument<256> doc;
  doc["type"] = "orientation";
  doc["timestamp"] = millis();

  JsonObject quat = doc.createNestedObject("quaternion");
  quat["w"] = q.w();
  quat["x"] = q.x();
  quat["y"] = q.y();
  quat["z"] = q.z();

  String json;
  serializeJson(doc, json);
  webSocket.broadcastTXT(json);
}
```

### Three.js Client (Beispiel)

```javascript
const ws = new WebSocket('ws://192.168.10.10:81');

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  if (data.type === 'orientation') {
    const { w, x, y, z } = data.quaternion;
    mesh.quaternion.set(x, y, z, w);
  }
};
```

---

## 6. Sensor Fusion

### Was ist Sensor Fusion?

Kombiniert Daten mehrerer Sensoren zu einer stabilen Orientierung:
- **Gyro**: Schnell, aber driftet
- **Accelerometer**: Stabil, aber rauscht bei Bewegung
- **Magnetometer**: Absolute Referenz, aber störanfällig

### Fusion-Algorithmen

| Algorithmus | Komplexität | Qualität | MCU-Last |
|-------------|-------------|----------|----------|
| **Complementary** | Niedrig | Okay | Minimal |
| **Madgwick** | Mittel | Gut | Niedrig |
| **Mahony** | Mittel | Gut | Niedrig |
| **Kalman (EKF)** | Hoch | Sehr gut | Hoch |

**Empfehlung:**
- Mit BNO055/BNO085/ICM-20948: Integrierte Fusion nutzen
- Mit MPU6050/LSM6DSO: Madgwick-Filter implementieren

### Madgwick-Library (Arduino)

```cpp
#include <MadgwickAHRS.h>

Madgwick filter;

void setup() {
  filter.begin(100); // 100 Hz Update-Rate
}

void loop() {
  filter.updateIMU(gx, gy, gz, ax, ay, az);

  float roll = filter.getRoll();
  float pitch = filter.getPitch();
  float yaw = filter.getYaw();
}
```

---

## 7. DIY-Projekte & Communities

### Relevante GitHub-Projekte

| Projekt | Beschreibung | Link |
|---------|--------------|------|
| **ESP32-MPU9250-web-view** | ESP32 + IMU → WebSocket → Three.js | [GitHub](https://github.com/DominikN/ESP32-MPU9250-web-view) |
| **mpu9250-esp32** | PlatformIO + Madgwick + Web-UI | [GitHub](https://github.com/NicHub/mpu9250-esp32) |
| **IMU-Webserial-Visualiser** | BNO080 → Web Serial → Three.js | [GitHub](https://github.com/MengLinMaker/IMU-Webserial-Visualiser) |
| **SlimeVR** | DIY Full-Body VR Tracking | [slimevr.dev](https://docs.slimevr.dev) |
| **VRController** | Arduino + MPU9250 + Bluetooth | [GitHub](https://github.com/dhfmzk/VRController) |

### Communities

| Community | Fokus | Link |
|-----------|-------|------|
| **XSimulator** | DIY Motion Simulators | [xsimulator.net](https://www.xsimulator.net) |
| **SimTools** | Motion Platform Software | [simtools.us](https://simtools.us) |
| **SlimeVR Discord** | DIY VR Tracking | [Discord](https://discord.gg/SlimeVR) |
| **r/DIYVRgear** | VR Hardware Projekte | [Reddit](https://www.reddit.com/r/DIYVRgear/) |
| **Arduino Forum** | Sensor-Fragen | [forum.arduino.cc](https://forum.arduino.cc) |

### SimTools

Motion-Simulator-Software für DIY-Plattformen:
- Extrahiert Telemetrie aus 100+ Spielen
- Unterstützt Arduino/ESP32 als Controller
- 3DOF bis 6DOF Plattformen
- Kostenlose DIY-Edition verfügbar

**Relevant für uns:** Plugin-Architektur könnte als Inspiration dienen.

---

## 8. Empfohlene Hardware-Konfigurationen

### Variante A: Einfach & Bewährt (BNO055)

```
Komponenten:
- ESP32 DevKit V1 (~10€)
- Adafruit BNO055 (~35€)
- USB-Powerbank
- Jumper-Kabel

Gesamt: ~50€

Pro: Einfachstes Setup, viele Tutorials
Contra: Höherer Drift als Alternativen
```

### Variante B: Preis-Leistung (ICM-20948)

```
Komponenten:
- ESP32 DevKit V1 (~10€)
- SparkFun ICM-20948 (~15€)
- USB-Powerbank
- Level Shifter (3.3V/1.8V)

Gesamt: ~30€

Pro: Günstiger, gute Qualität
Contra: Komplexeres Setup (Spannungspegel)
```

### Variante C: Best Quality (BNO085)

```
Komponenten:
- ESP32-S3 DevKit (~15€)
- Adafruit BNO085 (~25€)
- USB-Powerbank

Gesamt: ~45€

Pro: Beste Fusion, niedrigster Drift
Contra: Höchster Preis, Verfügbarkeit
```

### Variante D: Budget-Experiment (MPU6050 + Madgwick)

```
Komponenten:
- ESP32 DevKit V1 (~10€)
- MPU6050 Clone (~3€)
- Software: Madgwick-Filter

Gesamt: ~15€

Pro: Sehr günstig für Tests
Contra: Kein Magnetometer, braucht Filter-Tuning
```

---

## 9. Best Practices

### Hardware

1. **Sensor-Platzierung:** Nahe am Rotationszentrum montieren
2. **Kabel:** Kurz halten, abschirmen bei langen Strecken
3. **Stromversorgung:** Stabile 3.3V, Kondensator am Sensor
4. **Magnetische Störungen:** Abstand zu Motoren, Metall

### Software

1. **Kalibrierung:** Beim Start durchführen, Daten speichern
2. **Achsen-Mapping:** Sensor-Koordinaten zu Engine-Koordinaten
3. **Smoothing:** Leichtes Tiefpassfilter für ruhige Animation
4. **Dead Zone:** Kleine Bewegungen ignorieren

### Netzwerk

1. **Dediziertes 5GHz WiFi:** Keine Interferenz mit Besuchern
2. **Statische IP:** DHCP-Reservierung für alle Geräte
3. **Keep-Alive:** Heartbeat alle 5s senden
4. **Reconnect-Logik:** Automatische Wiederverbindung

---

## 10. Link-Sammlung

### Dokumentation & Tutorials

- [Adafruit BNO055 Guide](https://learn.adafruit.com/adafruit-bno055-absolute-orientation-sensor)
- [SparkFun ICM-20948 Hookup Guide](https://learn.sparkfun.com/tutorials/icm-20948-hookup-guide)
- [ESP32 WebSocket Tutorial](https://randomnerdtutorials.com/esp32-websocket-server-arduino/)
- [Three.js Quaternion Docs](https://threejs.org/docs/#api/en/math/Quaternion)
- [Madgwick Filter Paper](https://x-io.co.uk/open-source-imu-and-ahrs-algorithms/)

### Sensor-Datenblätter

- [BNO055 Datasheet (Bosch)](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bno055-ds000.pdf)
- [BNO085 Datasheet (Bosch)](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bno085-ds000.pdf)
- [ICM-20948 Datasheet (TDK)](https://invensense.tdk.com/products/motion-tracking/9-axis/icm-20948/)

### DIY-Projekte

- [ESP32-MPU9250-web-view](https://github.com/DominikN/ESP32-MPU9250-web-view)
- [SlimeVR Dokumentation](https://docs.slimevr.dev/diy/imu-comparison.html)
- [SimTools Motion Software](https://simtools.us)
- [XSimulator Forum](https://www.xsimulator.net/community/)

### Libraries

- [Adafruit BNO055 Arduino](https://github.com/adafruit/Adafruit_BNO055)
- [SparkFun ICM-20948 Arduino](https://github.com/sparkfun/SparkFun_ICM-20948_ArduinoLibrary)
- [MadgwickAHRS Arduino](https://github.com/arduino-libraries/MadgwickAHRS)
- [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)
- [ArduinoJson](https://arduinojson.org/)

### Shops

- [Berrybase (DE)](https://www.berrybase.de)
- [AZ-Delivery (DE)](https://www.az-delivery.de)
- [Reichelt (DE)](https://www.reichelt.de)
- [Adafruit (US)](https://www.adafruit.com)
- [SparkFun (US)](https://www.sparkfun.com)

---

## Nächste Schritte

1. **Hardware bestellen:** BNO055 oder ICM-20948 + ESP32
2. **Prototyp bauen:** Sensor → ESP32 → Serial Monitor
3. **WebSocket implementieren:** Daten an Browser senden
4. **Three.js testen:** Cube rotiert mit Sensor

Siehe auch: [M1_SENSOR_PROTOTYPE.md](../milestones/M1_SENSOR_PROTOTYPE.md)

---

*Teil des [Neural Flight](../../README.md) Projekts | Recherche abgeschlossen: 2026-01-05*
