# SvelteKit WebXR Guide

> Frontend-Stack Step-by-Step für die WebXR-Anwendung

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Anleitung zur Entwicklung der WebXR-Anwendung mit SvelteKit, Threlte und Three.js für die Meta Quest 3.

## Voraussetzungen

- [ ] Node.js ≥20 LTS
- [ ] pnpm installiert
- [ ] Grundkenntnisse Svelte/SvelteKit

## Inhalt

### Projekt initialisieren

```bash
pnpm create svelte@latest icaros-webxr
cd icaros-webxr
pnpm install
```

### Threlte Integration

[Wird ergänzt: Three.js + Svelte Setup]

### WebXR Session

[Wird ergänzt: VR-Modus aktivieren]

### HTTPS für WebXR

[Wird ergänzt: vite-plugin-mkcert Konfiguration]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| WebXR nicht verfügbar | HTTPS prüfen, Quest Browser verwenden |
| Three.js Bundling-Fehler | `optimizeDeps.exclude: ['three']` |
| HMR funktioniert nicht | `--host` Flag für Vite Dev Server |

## Referenzen

- [SvelteKit Docs](https://kit.svelte.dev/docs)
- [Threlte Docs](https://threlte.xyz)
- [WebXR Device API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/WebXR_Device_API)

---

*Teil des [Neural Flight](../README.md) Projekts*
