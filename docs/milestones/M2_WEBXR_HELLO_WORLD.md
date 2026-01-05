# M2: WebXR Hello World

> Cube in VR auf Meta Quest 3 anzeigen

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026
**Zeitraum:** KW 2-3 (Januar 2026)

---

## Übersicht

Zweiter Meilenstein: Minimale WebXR-Anwendung mit SvelteKit + Threlte, die einen 3D-Würfel im VR-Modus auf der Quest 3 anzeigt.

## Erfolgskriterium

> Cube in VR sichtbar, Head-Tracking funktioniert

## Voraussetzungen

- [ ] SvelteKit Projekt initialisiert
- [ ] Threlte + Three.js installiert
- [ ] Quest 3 mit ADB verbunden
- [ ] HTTPS-Zertifikate konfiguriert

## Tasks

- [ ] SvelteKit Skeleton erstellen
- [ ] Threlte Integration
- [ ] vite-plugin-mkcert für HTTPS
- [ ] ADB Reverse Tunnel einrichten
- [ ] WebXR Session starten (VR Button)
- [ ] Test auf Quest Browser

## Technische Details

[Wird ergänzt: Basis-Code für Threlte + XR]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| VR-Button fehlt | WebXR API nicht verfügbar (HTTPS?) |
| Schwarzer Screen in VR | Camera-Position prüfen |
| Quest findet localhost nicht | adb reverse ausführen |

## Referenzen

- [SVELTEKIT_WEBXR_GUIDE.md](../02_software/SVELTEKIT_WEBXR_GUIDE.md)
- [THRELTE_SETUP.md](../02_software/THRELTE_SETUP.md)
- [META_QUEST_WORKFLOW.md](../03_integration/META_QUEST_WORKFLOW.md)

---

*Teil des [Neural Flight](../README.md) Projekts*
