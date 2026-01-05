# M5: Flight Physics

> Implementierung des Flugmodells

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026
**Zeitraum:** KW 7-10 (Februar/März 2026)

---

## Übersicht

Fünfter Meilenstein: Körperneigung wird in Flugbewegung übersetzt. Pitch → Geschwindigkeit, Roll → Kurve. Entkoppelte Blick- und Flugrichtung.

## Erfolgskriterium

> Intuitive Flugsteuerung über Körperneigung, komfortabel, ohne Motion Sickness

## Voraussetzungen

- [ ] M4 abgeschlossen (Kalibrierung)
- [ ] Sensor-Daten stabil und latenzarm
- [ ] Test-Umgebung (Skybox + Landmarks)

## Tasks

- [ ] Pitch → Vorwärts/Rückwärts Geschwindigkeit
- [ ] Roll → Kurvenbewegung (Links/Rechts)
- [ ] Decoupled View (Blick ≠ Flugrichtung)
- [ ] Speed Limits (Soft Caps für Comfort)
- [ ] Acceleration Curves (Smoothing)
- [ ] Boundary-System (Soft Walls)

## Technische Details

[Wird ergänzt: Physics-Formeln, Comfort-Parameter]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Motion Sickness | Geschwindigkeit reduzieren, Vignette einblenden |
| Steuerung zu sensitiv | Dead Zone implementieren |
| Drift nach Kalibrierung | Re-Kalibrierung Option |

## Referenzen

- [VESTIBULAR_SYSTEM.md](../05_research/VESTIBULAR_SYSTEM.md)
- [SYSTEM_ARCHITECTURE.md](../03_integration/SYSTEM_ARCHITECTURE.md)

---

*Teil des [Neural Flight](../README.md) Projekts*
