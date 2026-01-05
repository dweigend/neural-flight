# Neural Flight

> VR flight experience on ICAROS — open-source project by [Futurium Lab](https://futurium.de)

---

## What is this?

An **immersive VR flight installation** for the Futurium exhibition opening **September 2026**. Visitors fly through virtual worlds using the [ICAROS](https://www.icaros.com) fitness simulator.

**Key features:**
- WebXR-based (no app store)
- ESP32 + IMU for real-time motion tracking
- AR onboarding with Quest 3 passthrough
- Open architecture for community contributions

---

## Progress

### Done
- [x] Sensor research ([SENSOR_RESEARCH.md](docs/hardware/SENSOR_RESEARCH.md))
- [x] WebXR framework evaluation ([FRAMEWORK_OPTIONS.md](docs/software/FRAMEWORK_OPTIONS.md))
- [x] System architecture ([SYSTEM_ARCHITECTURE.md](docs/integration/SYSTEM_ARCHITECTURE.md))
- [x] Quest 3 dev workflow ([META_QUEST_WORKFLOW.md](docs/integration/META_QUEST_WORKFLOW.md))

### Next
- [ ] Hardware prototype (ESP32 + BNO055)
- [ ] WebXR hello world on Quest 3
- [ ] Sensor-to-VR bridge

---

## Documentation

```
docs/
├── hardware/       # Sensors, components
├── software/       # WebXR, Three.js, frameworks
└── integration/    # Architecture, protocols, Quest workflow
```

| Document | Description |
|----------|-------------|
| [HARDWARE_LIST](docs/hardware/HARDWARE_LIST.md) | Components & vendors |
| [SENSOR_RESEARCH](docs/hardware/SENSOR_RESEARCH.md) | IMU comparison |
| [THREEJS_WEBXR_GUIDE](docs/software/THREEJS_WEBXR_GUIDE.md) | WebXR setup |
| [SYSTEM_ARCHITECTURE](docs/integration/SYSTEM_ARCHITECTURE.md) | Technical overview |
| [WEBSOCKET_PROTOCOL](docs/integration/WEBSOCKET_PROTOCOL.md) | Sensor data format |
| [ROADMAP](docs/ROADMAP.md) | Milestones & timeline |

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| VR | Meta Quest 3, WebXR |
| 3D | Three.js (Threlte, A-Frame optional) |
| Hardware | ESP32, BNO055/BNO085 IMU |
| Backend | Node.js, Socket.io |

---

## Background

Builds on our 2019 project ["Being a Drone"](https://www.imd.tu-bs.de/index.php/projects/vr-being-a-drone--flying-architecture/) with TU Braunschweig.

Part of the **"Human + Technology"** theme year 2026.

---

## Contact

**David Weigend** — Lab Lead, Futurium Berlin
weigend@futurium.de

---

**License:** MIT
