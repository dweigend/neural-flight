# Neural Flight

> Open-Source-Plattform für immersive VR-Erfahrungen an der Schnittstelle von Mensch und Technik

---

## Was ist das hier?

Dieses Repository ist die **Wissensbasis und Experimentierwerkstatt** für das Neural Flight Projekt. Hier sammeln wir Ideen, recherchieren Technologien und dokumentieren unsere Erkenntnisse – bevor wir in die eigentliche Entwicklung gehen.

**Der Fokus liegt auf:**
- Experimentieren und Ausprobieren
- Verstehen, welcher Software-Stack funktioniert
- Herausfinden, wie die Hardware-Integration gelingen kann
- Schrittweises Aufbauen einer soliden Infrastruktur

Das Projekt wächst organisch. Wir starten mit Recherche und Prototypen, bevor wir uns auf einen finalen Tech-Stack festlegen.

---

## Die Vision

Wir bauen eine offene Plattform für immersive VR-Erfahrungen, die den menschlichen Körper ins Zentrum stellen. Konkret: VR-Flugerlebnisse auf ICAROS-Simulatoren, bei denen physische Bewegung und virtuelle Wahrnehmung verschmelzen.

**Kernfragen, die uns antreiben:**
- Wie akzeptiert das Gehirn virtuelle Körper als eigene?
- Wie können wir Motion Sickness vermeiden, indem wir echte Bewegung einbeziehen?
- Wie bauen wir eine Infrastruktur, die modular und erweiterbar ist?

---

## Projektphasen

| Phase | Was passiert | Status |
|-------|--------------|--------|
| **1. Recherche** | Wissen sammeln, Technologien evaluieren, Architektur planen | 🔄 Jetzt |
| **2. Prototypen** | Hardware testen, erste Software-Experimente | ⏳ Bald |
| **3. Entwicklung** | Kerninfrastruktur aufbauen | ⏳ Geplant |
| **4. Integration** | Alles zusammenführen, testen, iterieren | ⏳ Geplant |

---

## Repository-Struktur

```
neuralFlight/
│
├── docs/                    # Recherche & Dokumentation
│   ├── hardware/            # ESP32, Sensoren, Netzwerk
│   ├── software/            # WebXR, Frameworks, Tools
│   ├── integration/         # Systemarchitektur, Protokolle
│   ├── milestones/          # Meilenstein-Definitionen
│   ├── research/            # Wissenschaftlicher Hintergrund
│   └── collaboration/       # Kooperationen, Contribution Guide
│
├── assets/                  # Medien & Diagramme
│   ├── diagrams/            # Mermaid-Diagramme (.mmd)
│   └── images/              # Screenshots, Fotos
│
└── README.md                # Du bist hier
```

**Zukünftig kommen hinzu:**
- `src/` – Code (Frontend, Backend, Firmware)
- `prototypes/` – Schnelle Experimente
- `apps/` – Fertige Anwendungen

Die Struktur wächst mit dem Projekt.

---

## Einstiegspunkte

| Wenn du wissen willst... | Dann lies... |
|-------------------------|--------------|
| Wie das System funktionieren soll | [docs/integration/SYSTEM_ARCHITECTURE.md](docs/integration/SYSTEM_ARCHITECTURE.md) |
| Welche Hardware wir brauchen | [docs/hardware/HARDWARE_LIST.md](docs/hardware/HARDWARE_LIST.md) |
| Was die Meilensteine sind | [docs/ROADMAP.md](docs/ROADMAP.md) |
| Wie die Kommunikation läuft | [docs/integration/WEBSOCKET_PROTOCOL.md](docs/integration/WEBSOCKET_PROTOCOL.md) |
| Was es mit Embodiment auf sich hat | [docs/research/BODY_OWNERSHIP_ILLUSION.md](docs/research/BODY_OWNERSHIP_ILLUSION.md) |

---

## Kontext

**Entstehung:** Dieses Projekt entsteht im Rahmen des Themenjahrs "Mensch + Technik" 2026 am [Futurium Berlin](https://futurium.de).

**Referenz:** 2019 haben wir mit der TU Braunschweig das Projekt ["Being a Drone – Flying Architecture"](https://www.imd.tu-bs.de/index.php/projects/vr-being-a-drone--flying-architecture/) realisiert. Neural Flight baut auf diesen Erfahrungen auf.

---

## Open Source

Dieses Projekt ist Open Source (MIT License). Wir glauben daran, dass gute Infrastruktur geteilt werden sollte – besonders wenn es um Experimente an der Schnittstelle von Mensch und Technik geht.

Beiträge sind willkommen. Siehe [docs/collaboration/CONTRIBUTION_GUIDE.md](docs/collaboration/CONTRIBUTION_GUIDE.md).

---

## Kontakt

**David Weigend**
Leiter Lab, Futurium Berlin
weigend@futurium.de

---

*Work in Progress. Dieses Repository wächst.*
