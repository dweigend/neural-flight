# Sensor Research: Orientation Tracking for Motion Platforms

> Comprehensive research on IMU sensors, protocols, and DIY solutions

**Status:** 🟢 Complete
**Last Updated:** 2026-01-05

---

## 1. Problem Statement

### What are we measuring?

The ICAROS is a mechanical platform that can move in **3 axes**:
- **Pitch**: Forward/backward tilt
- **Roll**: Side tilt (left/right)
- **Yaw**: Rotation around vertical axis (limited)

We need this motion data in real-time to send to a VR application.

### Requirements

| Requirement | Target | Reason |
|-------------|--------|--------|
| **Latency** | <20ms end-to-end | VR comfort, motion sickness |
| **Update Rate** | 50-100 Hz | Smooth animation |
| **Accuracy** | ±1-2° | Intuitive control |
| **Drift** | Minimal over 5 min | Session duration |
| **Cost** | <€50 per sensor | Budget-friendly |

---

## 2. Sensor Types Overview

### A. IMU Sensors (Inertial Measurement Unit)

IMUs combine multiple sensors:
- **Accelerometer**: Measures acceleration (including gravity)
- **Gyroscope**: Measures rotation speed
- **Magnetometer**: Measures Earth's magnetic field (compass)

**Pros:**
- Compact, affordable, self-contained
- No external infrastructure needed
- Can be mounted directly on platform

**Cons:**
- Drift over time (gyro bias)
- Magnetic interference (metal, electronics)
- Orientation only, no absolute position

### B. Optical Tracking

External cameras track markers or recognize objects.

**Examples:**
- TrackIR (IR camera + reflectors, ~€150)
- Webcam + AI (OpenTrack, free)
- Vive Tracker (Steam Lighthouse, ~€200)

**Pros:**
- No drift
- Absolute position possible
- Higher precision

**Cons:**
- Line of sight required
- Setup effort
- More expensive

### Recommendation

**IMU-based** is the best choice:
- Simple mounting directly on ICAROS
- No external infrastructure in exhibition space
- Cost-effective for prototyping
- For short sessions (2-5 min), drift is not critical

---

## 3. Sensor Comparison

| Sensor | DoF | Fusion | Price | Drift | Recommendation |
|--------|-----|--------|-------|-------|----------------|
| **BNO085** | 9 | ✅ Onboard | ~€25 | Very low | ⭐ Best choice |
| **ICM-20948** | 9 | ✅ DMP | ~€8 | Low | ⭐ Price-performance |
| **BNO055** | 9 | ✅ Onboard | ~€35 | Medium | Okay, outdated |
| **LSM6DSO** | 6 | ❌ External | ~€5 | High | Only with filter |
| **MPU6050** | 6 | ❌ External | ~€3 | Very high | ❌ Not recommended |

### BNO085 (Bosch) – Top Recommendation

- Best fusion quality
- Very low drift
- Production-ready
- ~€25

### ICM-20948 (TDK InvenSense) – Price-Performance Winner

- Cheaper than BNO085
- Good fusion quality (DMP)
- Low power consumption
- ~€8

### BNO055 (Bosch) – The Classic

- Easiest setup (plug & play)
- Many tutorials available
- Direct quaternion output
- Higher drift than successors
- ~€35

### MPU6050 – Not Recommended

- No magnetometer → strong yaw drift
- No integrated fusion
- High failure rate with cheap clones
- Outdated (2012)

---

## 4. Data Formats for 3D Engines

### Quaternions vs. Euler Angles

| Aspect | Quaternion | Euler |
|--------|------------|-------|
| **Format** | (w, x, y, z) | (x, y, z) in degrees |
| **Gimbal Lock** | ❌ No problem | ⚠️ At ±90° pitch |
| **Interpolation** | ✅ Smooth (SLERP) | ⚠️ Jumps |

**Recommendation:** Quaternions for transmission, Euler for debugging.

### Engine-specific Formats

#### Three.js
```javascript
// Quaternion order: (x, y, z, w)
object.quaternion.set(x, y, z, w);
// Three.js is Y-up, right-handed
```

#### Godot 4
```gdscript
# Quaternion: Quat(x, y, z, w)
transform.basis = Basis(Quaternion(x, y, z, w))
# Godot is Y-up, right-handed
```

#### Unity
```csharp
// Unity is Y-up, left-handed (!)
// Axis conversion needed: z = -z
transform.rotation = new Quaternion(x, y, z, w);
```

---

## 5. Transmission Protocols

| Protocol | Latency | Use Case | Complexity |
|----------|---------|----------|------------|
| **WebSocket** | 5-15ms | Web, VR | Low |
| **OSC** | <5ms | Audio/VJ | Medium |
| **MQTT** | 10-30ms | IoT, Multi-Device | Medium |
| **Serial** | <1ms | Desktop, Wired | Very low |

### Recommendation: WebSocket

- Native browser support (Three.js, WebXR)
- Bidirectional
- ESP32 libraries available
- Well documented

---

## 6. Recommended Configurations

### Option A: Simple & Proven (BNO055)
- ESP32 DevKit V1 (~€10)
- Adafruit BNO055 (~€35)
- **Total: ~€50**
- Pro: Easiest setup, many tutorials

### Option B: Price-Performance (ICM-20948)
- ESP32 DevKit V1 (~€10)
- SparkFun ICM-20948 (~€15)
- **Total: ~€30**
- Pro: Cheaper, good quality

### Option C: Best Quality (BNO085)
- ESP32-S3 DevKit (~€15)
- Adafruit BNO085 (~€25)
- **Total: ~€45**
- Pro: Best fusion, lowest drift

---

## 7. German Vendors

| Vendor | Specialization | Link |
|--------|----------------|------|
| **Berrybase** | Adafruit, Raspberry Pi | [berrybase.de](https://www.berrybase.de) |
| **AZ-Delivery** | ESP32, Arduino | [az-delivery.de](https://www.az-delivery.de) |
| **Reichelt** | ICM-20948 | [reichelt.de](https://www.reichelt.de) |
| **Eckstein-Shop** | Adafruit Original | [eckstein-shop.de](https://eckstein-shop.de) |
| **Mouser/Digikey** | BNO085 | [mouser.de](https://www.mouser.de) |

---

## 8. References

### Documentation
- [Adafruit BNO055 Guide](https://learn.adafruit.com/adafruit-bno055-absolute-orientation-sensor)
- [SparkFun ICM-20948 Hookup Guide](https://learn.sparkfun.com/tutorials/icm-20948-hookup-guide)
- [ESP32 WebSocket Tutorial](https://randomnerdtutorials.com/esp32-websocket-server-arduino/)

### DIY Projects
- [SlimeVR Documentation](https://docs.slimevr.dev/diy/imu-comparison.html)
- [XSimulator Forum](https://www.xsimulator.net/community/)

### Libraries
- [Adafruit BNO055 Arduino](https://github.com/adafruit/Adafruit_BNO055)
- [MadgwickAHRS Arduino](https://github.com/arduino-libraries/MadgwickAHRS)
- [ArduinoJson](https://arduinojson.org/)

---

*Part of the [Neural Flight](../README.md) project*
