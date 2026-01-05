# WebSocket Server

> Node.js Backend Setup für den Socket.io Hub

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Der WebSocket-Server fungiert als Hub zwischen ESP32-Sensoren und WebXR-Clients. Er empfängt Telemetrie-Daten und broadcasted sie an alle VR-Clients.

## Voraussetzungen

- [ ] Node.js ≥20 LTS
- [ ] Grundkenntnisse Socket.io

## Inhalt

### Projekt-Setup

```bash
mkdir icaros-server
cd icaros-server
npm init -y
npm install socket.io express
```

### Server-Architektur

[Wird ergänzt: Namespace-Konzept /sensor, /vr, /admin]

### Event-Handling

[Wird ergänzt: Telemetry-Broadcast]

### Development vs Production

[Wird ergänzt: Integriert in SvelteKit vs Standalone]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| CORS-Fehler | `cors: { origin: '*' }` in Socket.io Config |
| Port bereits belegt | `lsof -i :3000` und Prozess beenden |
| Client verbindet nicht | WebSocket URL prüfen (ws:// vs wss://) |

## Referenzen

- [Socket.io Documentation](https://socket.io/docs/v4/)
- [Socket.io + SvelteKit](https://socket.io/how-to/use-with-sveltekit)

---

*Teil des [Neural Flight](../README.md) Projekts*
