# Threlte Setup

> Three.js + Svelte Integration für 3D-Rendering

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Threlte ist die Brücke zwischen Svelte und Three.js. Es ermöglicht deklaratives 3D-Rendering mit Svelte-Komponenten.

## Voraussetzungen

- [ ] SvelteKit Projekt initialisiert
- [ ] Grundkenntnisse Three.js

## Inhalt

### Installation

```bash
pnpm add @threlte/core @threlte/extras @threlte/xr three
pnpm add -D @types/three
```

### Basis-Setup

[Wird ergänzt: Canvas, Scene, Camera]

### XR-Integration

[Wird ergänzt: @threlte/xr für WebXR]

### Performance-Tipps

[Wird ergänzt: Optimierung für Quest 3]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Canvas nicht sichtbar | CSS height: 100% prüfen |
| Low FPS auf Quest | Geometrie vereinfachen, Texturen komprimieren |
| XR-Button fehlt | @threlte/xr Version prüfen |

## Referenzen

- [Threlte Documentation](https://threlte.xyz)
- [Threlte XR Package](https://threlte.xyz/docs/reference/xr/getting-started)
- [Three.js Docs](https://threejs.org/docs)

---

*Teil des [Neural Flight](../README.md) Projekts*
