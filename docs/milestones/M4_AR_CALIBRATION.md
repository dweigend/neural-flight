# M4: AR Calibration

> Quest 3 Passthrough mit Balance-Check UI

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026
**Zeitraum:** KW 4-6 (Februar 2026)

---

## Übersicht

Vierter Meilenstein: AR-Onboarding mit Quest 3 Passthrough. Nutzer sehen reale Umgebung + virtuelle Overlays für Positionierung und Kalibrierung.

## Erfolgskriterium

> Nutzer sieht realen ICAROS + Overlay, System erkennt stabile Position

## Voraussetzungen

- [ ] M3 abgeschlossen (Sensor → VR)
- [ ] Quest 3 mit Passthrough-Fähigkeit
- [ ] 3D-Modell des ICAROS (Wireframe)

## Tasks

- [ ] Passthrough-Modus aktivieren (immersive-ar)
- [ ] Ghost Model Overlay (ICAROS Wireframe)
- [ ] Wasserwaage HUD (Pitch/Roll Visualisierung)
- [ ] Balance Detection (±3° für 2-3 Sekunden)
- [ ] Audio Feedback ("System kalibriert")
- [ ] Transition AR → VR (Crossfade)

## Technische Details

[Wird ergänzt: AR Session Setup, UI-Komponenten]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Passthrough schwarz | Permissions prüfen |
| Overlay driftet | Anchor-System verwenden |
| Balance nie erreicht | Threshold anpassen |

## Referenzen

- [AR_MR_FEATURES.md](../03_integration/AR_MR_FEATURES.md)
- [SYSTEM_ARCHITECTURE.md](../03_integration/SYSTEM_ARCHITECTURE.md)

---

*Teil des [Neural Flight](../README.md) Projekts*
