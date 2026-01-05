# M3: Sensor to VR Bridge

> VR-Objekt rotiert synchron zur Sensor-Bewegung

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026
**Zeitraum:** KW 3-4 (Januar/Februar 2026)

---

## Übersicht

Dritter Meilenstein: Verbindung von M1 (Sensor) und M2 (WebXR). Ein 3D-Objekt in VR rotiert in Echtzeit basierend auf den Sensordaten.

## Erfolgskriterium

> Sensor in Hand = Cube in VR rotiert identisch, flüssig, ohne Lag

## Voraussetzungen

- [ ] M1 abgeschlossen (Sensor → Server)
- [ ] M2 abgeschlossen (WebXR Hello World)
- [ ] Socket.io Client in SvelteKit

## Tasks

- [ ] Socket.io Client Integration
- [ ] Svelte Store für Sensor-Daten
- [ ] Three.js Rotation Binding
- [ ] Smoothing/Lerping implementieren
- [ ] Latenz-Messung (<20ms End-to-End)

## Technische Details

[Wird ergänzt: Store-Struktur, Rotation-Mapping]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Rotation ruckelt | Lerping/Smoothing aktivieren |
| Latenz >50ms | Netzwerk-Konfiguration prüfen |
| Quaternion-Fehler | Normalisierung prüfen |

## Referenzen

- [WEBSOCKET_PROTOCOL.md](../03_integration/WEBSOCKET_PROTOCOL.md)
- [SYSTEM_ARCHITECTURE.md](../03_integration/SYSTEM_ARCHITECTURE.md)

---

*Teil des [Neural Flight](../README.md) Projekts*
