# Software Requirements

> Dependencies and tools for development

**Status:** 🟢 Complete  
**Last Updated:** January 2026

---

## Development Machine

| Tool | Version | Purpose |
|------|---------|---------|
| **Node.js** | ≥20 LTS | JavaScript Runtime |
| **pnpm** | ≥8.0 | Package Manager |
| **Git** | ≥2.40 | Version Control |
| **VS Code** | Latest | IDE |

### VS Code Extensions

| Extension | Purpose |
|-----------|---------|
| `svelte.svelte-vscode` | Svelte Syntax |
| `platformio.platformio-ide` | ESP32 Development |
| `dbaeumer.vscode-eslint` | Linting |

---

## Frontend Stack (WebXR App)

```json
{
  "dependencies": {
    "@sveltejs/kit": "^2.0.0",
    "@threlte/core": "^7.0.0",
    "@threlte/extras": "^8.0.0",
    "three": "^0.160.0",
    "socket.io-client": "^4.7.0"
  },
  "devDependencies": {
    "vite-plugin-mkcert": "^1.17.0",
    "typescript": "^5.3.0"
  }
}
```

---

## Embedded Stack (ESP32)

### PlatformIO

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
lib_deps =
    adafruit/Adafruit BNO055@^1.6.3
    links2004/WebSockets@^2.4.1
    bblanchon/ArduinoJson@^7.0.0
```

---

## Quest 3 Setup

1. Create [Meta Developer Account](https://developer.oculus.com)
2. Enable Developer Mode in Meta Quest App
3. Install ADB via [SideQuest](https://sidequestvr.com)
4. Test: `adb devices`

### ADB Reverse Tunnel

```bash
adb reverse tcp:5173 tcp:5173
```

---

## HTTPS for WebXR

WebXR requires HTTPS. Use `vite-plugin-mkcert`:

```typescript
// vite.config.ts
import mkcert from 'vite-plugin-mkcert';

export default {
  plugins: [sveltekit(), mkcert()],
  server: { https: true, host: true }
};
```

---

## Version Matrix

| Component | Tested With |
|-----------|-------------|
| Node.js | 20 LTS |
| SvelteKit | 2.0 |
| Three.js | 0.160 |
| Socket.io | 4.7 |
| ESP32 Arduino Core | 2.0.14 |
| Quest OS | v62+ |

---

*Part of the [Neural Flight](../README.md) project*
