# Übergabe

> Aktueller Stand für die nächste Session

**Letzte Aktualisierung:** 2026-01-05

---

## Was wurde gemacht?

### Session: 2026-01-05 (Initialisierung + Hardware-Recherche)

1. **Repository-Struktur aufgebaut**
   - Ordner: `docs/`, `assets/`, `dev/`
   - 26 Markdown-Dateien erstellt
   - 7 Mermaid-Diagramme extrahiert

2. **Dokumentation vorbereitet**
   - 7 Dateien vollständig (Hardware-Liste, Software-Requirements, etc.)
   - 19 Dateien als Draft-Templates
   - README.md komplett neu geschrieben

3. **Workflow etabliert**
   - `dev/WORKFLOW.md` – Maschinenlesbare Anweisungen
   - `dev/README-WORKFLOW.md` – Menschenlesbare Erklärung
   - `dev/UEBERGABE.md` – Dieses Dokument

4. **Hardware-Recherche abgeschlossen** ✅
   - Umfassende Sensor-Recherche: `docs/hardware/SENSOR_RESEARCH.md`
   - Vergleich: BNO055 vs BNO085 vs ICM-20948 vs MPU6050
   - Protokolle: WebSocket, OSC, MQTT
   - Datenformate: Quaternionen für Three.js/Godot/Unity
   - DIY-Projekte und Communities gesammelt
   - Bezugsquellen Deutschland

### Session: 2026-01-05 (WebXR-Recherche)

5. **WebXR & Software-Recherche abgeschlossen** ✅
   - **Entscheidung:** Vanilla Three.js als Plattform-Basis
   - Framework-Optionen für Teams dokumentiert
   - Quest 3 Developer Workflow vollständig dokumentiert
   - AR/MR Features (Passthrough, Anchors) recherchiert

6. **Neue Dokumentation erstellt**
   - `docs/software/THREEJS_WEBXR_GUIDE.md` – Native Three.js + WebXR
   - `docs/software/FRAMEWORK_OPTIONS.md` – Threlte, A-Frame, R3F, Babylon
   - `docs/integration/META_QUEST_WORKFLOW.md` – ADB, HTTPS, Debugging
   - `docs/integration/AR_MR_FEATURES.md` – Passthrough, Anchors, Hit-Testing

7. **Roadmap aktualisiert** ✅
   - Phase 0 (Recherche) vollständig abgeschlossen
   - R1 + R2 als erledigt markiert
   - M1 + M2 bereit zum Start

---

## Aktueller Stand

### Projektphase
**Phase 0: Recherche** ✅ Abgeschlossen
**Phase 1: Prototyping** 🔄 Bereit zum Start

### Abgeschlossene Meilensteine
| Meilenstein | Status | Ergebnis |
|-------------|--------|----------|
| R1: Sensor & Hardware-Recherche | ✅ | `docs/hardware/SENSOR_RESEARCH.md` |
| R2: WebXR & Software-Recherche | ✅ | `docs/software/THREEJS_WEBXR_GUIDE.md` |

### Nächste Meilensteine
| Meilenstein | Status | Fokus |
|-------------|--------|-------|
| M1: Sensor-Prototyp | ⏳ Bereit | ESP32 + BNO085 → WebSocket |
| M2: WebXR Hello World | ⏳ Bereit | Cube in VR auf Quest 3 |

---

## Nächste Prioritäten

| Priorität | Thema | Referenz |
|-----------|-------|----------|
| 1 | **Hardware bestellen** | `docs/hardware/SENSOR_RESEARCH.md` (David bestellt selbst) |
| 2 | **Quest 3 einrichten** | `docs/integration/META_QUEST_WORKFLOW.md` |
| 3 | **M2 starten:** WebXR Hello World | `docs/software/THREEJS_WEBXR_GUIDE.md` |
| 4 | **M1 starten:** ESP32 + Sensor | Wartet auf Hardware |

---

## Wichtige Entscheidungen

| Entscheidung | Details |
|--------------|---------|
| **Framework** | Vanilla Three.js als Plattform-Basis |
| **Plattform-Ansatz** | Teams können eigene Frameworks wählen (Threlte, A-Frame, etc.) |
| **Hardware** | David bestellt selbst (BNO085 oder ICM-20948 + ESP32) |
| **Quest 3** | Ab morgen verfügbar für Tests |

---

## Dateien Status

### 🟢 Complete (Recherche)
| Datei | Beschreibung |
|-------|--------------|
| `README.md` | Projekt-Übersicht |
| `docs/PROJEKT_STRUKTUR.md` | Ordnerstruktur |
| `docs/ROADMAP.md` | Meilensteine & Timeline |
| `docs/hardware/HARDWARE_LIST.md` | Komponenten-Liste |
| `docs/hardware/SENSOR_RESEARCH.md` | IMU-Vergleich (R1) |
| `docs/software/SOFTWARE_REQUIREMENTS.md` | Tech Stack |
| `docs/software/THREEJS_WEBXR_GUIDE.md` | **NEU** Native Three.js + WebXR |
| `docs/software/FRAMEWORK_OPTIONS.md` | **NEU** Framework-Vergleich |
| `docs/integration/SYSTEM_ARCHITECTURE.md` | Architektur-Übersicht |
| `docs/integration/WEBSOCKET_PROTOCOL.md` | Sensor-Protokoll |
| `docs/integration/META_QUEST_WORKFLOW.md` | **AKTUALISIERT** Quest 3 Workflow |
| `docs/integration/AR_MR_FEATURES.md` | **AKTUALISIERT** Passthrough & MR |

### 🔴 Draft (noch zu füllen)
```
docs/hardware/
  - ESP32_SETUP.md
  - BNO055_SENSOR.md
  - ICAROS_MOUNTING.md
  - NETWORK_INFRASTRUCTURE.md

docs/software/
  - DEV_ENVIRONMENT_SETUP.md
  - ESP32_ARDUINO_GUIDE.md
  - WEBSOCKET_SERVER.md

docs/milestones/
  - M1_SENSOR_PROTOTYPE.md (Priorität!)
  - M2_WEBXR_HELLO_WORLD.md
  - M3_SENSOR_TO_VR_BRIDGE.md
  - M4_AR_CALIBRATION.md
  - M5_FLIGHT_PHYSICS.md

docs/research/
  - BODY_OWNERSHIP_ILLUSION.md
  - VESTIBULAR_SYSTEM.md
  - RELATED_PROJECTS.md

docs/collaboration/
  - UNI_PARTNERSHIPS.md
  - CONTRIBUTION_GUIDE.md
```

### 🗑️ Deprecated (können gelöscht werden)
```
docs/software/SVELTEKIT_WEBXR_GUIDE.md  → ersetzt durch THREEJS_WEBXR_GUIDE.md
docs/software/THRELTE_SETUP.md          → integriert in FRAMEWORK_OPTIONS.md
```

---

## Offene Fragen

- [x] ~~Welches WebXR-Framework bevorzugen?~~ → **Vanilla Three.js**
- [x] ~~Quest 3 zum Testen vorhanden?~~ → **Ja, ab morgen**
- [ ] Hardware-Bestellung abgeschlossen?
- [ ] Dediziertes WLAN für Low-Latency Setup?

---

## Hinweise für die nächste Session

1. **Quest 3 Setup testen:** Developer Mode, ADB Verbindung
2. **M2 Hello World starten:** Code aus `THREEJS_WEBXR_GUIDE.md` verwenden
3. **Hardware-Status prüfen:** Ist ESP32 + Sensor bestellt?
4. **Diese Datei aktualisieren** am Session-Ende

---

## Quick Start nächste Session

```bash
# 1. Quest 3 per USB verbinden
adb devices

# 2. Reverse Tunnel einrichten
adb reverse tcp:5173 tcp:5173

# 3. Dev Server starten (wenn Projekt existiert)
pnpm dev --host

# 4. Im Quest Browser öffnen
# https://localhost:5173
```

---

*Aktualisiert am Ende jeder Session*
