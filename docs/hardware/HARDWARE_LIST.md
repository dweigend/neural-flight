# Hardware Shopping List

> Components for the Icaros WebXR sensor system

**Status:** 🟢 Complete  
**Last Updated:** January 2026

---

## Prototype

| Component | Specification | Link | Price |
|-----------|---------------|------|-------|
| **ESP32 DevKit V1** | WROOM-32, 38 Pins | [AZ-Delivery](https://www.az-delivery.de/products/esp32-developmentboard) | ~€10 |
| **Adafruit BNO055** | 9-DOF IMU, I2C | [BerryBase](https://www.berrybase.de/adafruit-9-dof-absolute-orientation-imu-fusion-breakout-bno055) | ~€35 |
| **Breadboard + Jumper Cables** | For prototyping | [AZ-Delivery](https://www.az-delivery.de/products/breadboard-830) | ~€10 |
| **USB Power Bank** | 5V, 2A+ | Amazon | ~€15 |
| **WiFi 6 Router** | 5GHz, dedicated | [TP-Link Archer AX23](https://www.amazon.de/dp/B09CDHNB4Q) | ~€50 |

**Prototype Total: ~€120**

---

## Production (additional)

| Component | Specification | Link | Price |
|-----------|---------------|------|-------|
| **Raspberry Pi 5** | 8GB, as server | [BerryBase](https://www.berrybase.de/raspberry-pi-5-8gb-ram) | ~€95 |

---

## Pinout: ESP32 ↔ BNO055

| ESP32 | BNO055 |
|-------|--------|
| 3V3 | VIN |
| GND | GND |
| GPIO21 | SDA |
| GPIO22 | SCL |

---

## Why BNO055?

| | BNO055 | MPU6050 |
|-|--------|---------|
| **Sensor Fusion** | ✅ Onboard | ❌ Software |
| **Output** | Quaternions | Raw data |
| **Drift** | ✅ Auto-corrected | ❌ Manual |
| **VR Ready** | ✅ | ❌ |

---

*Part of the [Neural Flight](../README.md) project*
