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

5. **Roadmap aktualisiert** ✅
   - Phase 0 (Recherche) hinzugefügt
   - R1 (Sensor & Hardware) als abgeschlossen markiert
   - R2 (WebXR & Software) als nächster Schritt definiert

---

## Aktueller Stand

### Projektphase
**Phase 0: Recherche** 🔄

### Abgeschlossene Meilensteine
| Meilenstein | Status | Ergebnis |
|-------------|--------|----------|
| R1: Sensor & Hardware-Recherche | ✅ Abgeschlossen | `docs/hardware/SENSOR_RESEARCH.md` |

### Laufende/Geplante Recherchen
| Meilenstein | Status | Fokus |
|-------------|--------|-------|
| R2: WebXR & Software-Recherche | ⏳ Geplant | Threlte vs A-Frame vs Three.js, Quest 3 Workflow |

### Nächste Prioritäten

| Priorität | Thema | Datei |
|-----------|-------|-------|
| 1 | WebXR-Frameworks vergleichen | `docs/software/SVELTEKIT_WEBXR_GUIDE.md`, `docs/software/THRELTE_SETUP.md` |
| 2 | Quest 3 Developer Workflow | `docs/integration/META_QUEST_WORKFLOW.md` |
| 3 | AR/MR Features (Passthrough, RATK) | `docs/integration/AR_MR_FEATURES.md` |
| 4 | Prototyp-Hardware bestellen | Siehe `docs/hardware/SENSOR_RESEARCH.md` |

### Dateien mit Status 🔴 Draft (noch leer)

```
docs/hardware/
  - ESP32_SETUP.md
  - BNO055_SENSOR.md
  - ICAROS_MOUNTING.md
  - NETWORK_INFRASTRUCTURE.md

docs/software/
  - DEV_ENVIRONMENT_SETUP.md
  - SVELTEKIT_WEBXR_GUIDE.md
  - THRELTE_SETUP.md
  - ESP32_ARDUINO_GUIDE.md
  - WEBSOCKET_SERVER.md

docs/integration/
  - META_QUEST_WORKFLOW.md
  - AR_MR_FEATURES.md

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

---

## Offene Fragen

- [ ] Hardware bestellen? (Empfehlung: ESP32-S3 + BNO085 oder ICM-20948)
- [ ] Welches WebXR-Framework bevorzugen? (Threlte empfohlen wegen Svelte-Integration)
- [ ] Quest 3 zum Testen vorhanden?
- [ ] Dediziertes WLAN für Low-Latency Setup einplanen?

---

## Hinweise für die nächste Session

1. **Workflow beachten:** `dev/WORKFLOW.md` lesen
2. **Checkpoint machen** vor größeren Änderungen
3. **Diese Datei aktualisieren** am Session-Ende

---

## Verzeichnis der fertigen Dateien

| Datei | Status |
|-------|--------|
| `README.md` | 🟢 Complete |
| `docs/PROJEKT_STRUKTUR.md` | 🟢 Complete |
| `docs/ROADMAP.md` | 🟢 Complete |
| `docs/hardware/HARDWARE_LIST.md` | 🟢 Complete |
| `docs/software/SOFTWARE_REQUIREMENTS.md` | 🟢 Complete |
| `docs/integration/SYSTEM_ARCHITECTURE.md` | 🟢 Complete |
| `docs/integration/WEBSOCKET_PROTOCOL.md` | 🟢 Complete |

---

*Aktualisiert am Ende jeder Session*
