# Software Requirements

> Alle Dependencies, Tools und Versionen für die Entwicklung

**Status:** 🟢 Complete  
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Dieses Dokument listet alle Software-Komponenten für:
1. **Development Machine** – Lokale Entwicklungsumgebung
2. **Frontend** – SvelteKit WebXR App
3. **Backend** – Node.js WebSocket Server
4. **Embedded** – ESP32 Arduino Code
5. **Quest 3** – VR Headset Setup

---

## 1. Development Machine

### System-Anforderungen

| Komponente | Minimum | Empfohlen |
|------------|---------|-----------|
| **OS** | Windows 10 / macOS 12 / Ubuntu 22.04 | Ubuntu 24.04 LTS |
| **RAM** | 8 GB | 16 GB |
| **Speicher** | 20 GB frei | SSD |
| **GPU** | Integriert | Dediziert (für lokales VR-Testing) |

### Basis-Tools

| Tool | Version | Installation | Zweck |
|------|---------|--------------|-------|
| **Node.js** | ≥20 LTS | [nodejs.org](https://nodejs.org) | JavaScript Runtime |
| **pnpm** | ≥8.0 | `npm install -g pnpm` | Package Manager |
| **Git** | ≥2.40 | [git-scm.com](https://git-scm.com) | Version Control |
| **VS Code** | Latest | [code.visualstudio.com](https://code.visualstudio.com) | IDE |

### VS Code Extensions

| Extension | ID | Zweck |
|-----------|-----|-------|
| Svelte for VS Code | `svelte.svelte-vscode` | Svelte Syntax Highlighting |
| PlatformIO IDE | `platformio.platformio-ide` | ESP32 Development |
| ESLint | `dbaeumer.vscode-eslint` | JavaScript Linting |
| Prettier | `esbenp.prettier-vscode` | Code Formatting |
| Mermaid Editor | `tomoyukim.vscode-mermaid-editor` | Diagramme Preview |
| Live Share | `ms-vsliveshare.vsliveshare` | Pair Programming |

**settings.json Empfehlungen:**

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[svelte]": {
    "editor.defaultFormatter": "svelte.svelte-vscode"
  },
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

---

## 2. Frontend Stack (WebXR App)

### SvelteKit Projekt initialisieren

```bash
# Projekt erstellen
pnpm create svelte@latest icaros-webxr
cd icaros-webxr

# TypeScript, ESLint, Prettier auswählen
# Skeleton project wählen
```

### package.json

```json
{
  "name": "icaros-webxr",
  "version": "0.1.0",
  "type": "module",
  "scripts": {
    "dev": "vite dev --host",
    "dev:https": "vite dev --host --https",
    "dev:adb": "adb reverse tcp:5173 tcp:5173 && vite dev --host",
    "build": "vite build",
    "preview": "vite preview --host",
    "check": "svelte-kit sync && svelte-check --tsconfig ./tsconfig.json",
    "lint": "eslint ."
  },
  "dependencies": {
    "@sveltejs/kit": "^2.0.0",
    "@threlte/core": "^7.0.0",
    "@threlte/extras": "^8.0.0",
    "@threlte/xr": "^0.1.0",
    "three": "^0.160.0",
    "socket.io-client": "^4.7.0"
  },
  "devDependencies": {
    "@sveltejs/adapter-node": "^2.0.0",
    "@types/three": "^0.160.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "vite-plugin-mkcert": "^1.17.0",
    "svelte": "^4.2.0",
    "svelte-check": "^3.6.0",
    "eslint": "^8.56.0",
    "prettier": "^3.2.0",
    "prettier-plugin-svelte": "^3.1.0"
  }
}
```

### Dependencies im Detail

| Package | Version | Zweck |
|---------|---------|-------|
| `@sveltejs/kit` | ^2.0 | Full-Stack Framework |
| `@threlte/core` | ^7.0 | Three.js + Svelte Integration |
| `@threlte/extras` | ^8.0 | Zusätzliche Components |
| `@threlte/xr` | ^0.1 | WebXR Support für Threlte |
| `three` | ^0.160 | 3D Engine |
| `socket.io-client` | ^4.7 | WebSocket Client |
| `vite-plugin-mkcert` | ^1.17 | Lokale HTTPS Zertifikate |

### Optionale WebXR Libraries

| Library | NPM | Zweck |
|---------|-----|-------|
| RATK | `ratk` | Quest 3 MR Features (Passthrough, Anchors) |
| three-mesh-ui | `three-mesh-ui` | VR UI Components |
| tweakpane | `tweakpane` | Debug GUI |

### vite.config.ts

```typescript
import { sveltekit } from '@sveltejs/kit/vite';
import { defineConfig } from 'vite';
import mkcert from 'vite-plugin-mkcert';

export default defineConfig({
  plugins: [sveltekit(), mkcert()],
  server: {
    https: true,
    host: true, // 0.0.0.0 für Netzwerk-Zugriff
    port: 5173,
  },
  optimizeDeps: {
    exclude: ['three'], // Prevent bundling issues
  },
});
```

### svelte.config.js

```javascript
import adapter from '@sveltejs/adapter-node';
import { vitePreprocess } from '@sveltejs/kit/vite';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter({
      out: 'build',
      precompress: true,
    }),
  },
};

export default config;
```

---

## 3. Backend Stack (WebSocket Server)

### Standalone Server

Für Production: Separater Node.js Server.

```json
{
  "name": "icaros-server",
  "version": "0.1.0",
  "type": "module",
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js"
  },
  "dependencies": {
    "socket.io": "^4.7.0",
    "express": "^4.18.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0"
  }
}
```

### Integriert in SvelteKit

Für Development kann Socket.io in SvelteKit hooks integriert werden:

```typescript
// src/hooks.server.ts
import { Server } from 'socket.io';
import type { Handle } from '@sveltejs/kit';

let io: Server;

export const handle: Handle = async ({ event, resolve }) => {
  if (!io) {
    // Socket.io initialisieren
  }
  return resolve(event);
};
```

---

## 4. Embedded Stack (ESP32)

### PlatformIO (Empfohlen)

**platformio.ini:**

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
upload_speed = 921600

lib_deps =
    adafruit/Adafruit BNO055@^1.6.3
    adafruit/Adafruit Unified Sensor@^1.1.14
    links2004/WebSockets@^2.4.1
    bblanchon/ArduinoJson@^7.0.0

build_flags =
    -DCORE_DEBUG_LEVEL=3
    -DARDUINOJSON_ENABLE_ARDUINO_STRING=1
```

### Arduino IDE Alternative

Falls PlatformIO nicht gewünscht:

1. **Board Manager URL hinzufügen:**
   ```
   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
   ```

2. **Board installieren:**
   - Tools → Board → Boards Manager
   - "esp32" suchen und installieren

3. **Libraries installieren:**
   
   | Library | Version | Quelle |
   |---------|---------|--------|
   | Adafruit BNO055 | ^1.6.3 | Library Manager |
   | Adafruit Unified Sensor | ^1.1.14 | Library Manager |
   | WebSockets (Markus Sattler) | ^2.4.1 | Library Manager |
   | ArduinoJson | ^7.0.0 | Library Manager |

4. **Board Settings:**
   - Board: "ESP32 Dev Module"
   - Upload Speed: 921600
   - CPU Frequency: 240 MHz
   - Flash Frequency: 80 MHz
   - Flash Size: 4MB

---

## 5. Quest 3 Setup

### Developer Mode aktivieren

1. [Meta Developer Account](https://developer.oculus.com) erstellen
2. Meta Quest App auf Smartphone öffnen
3. Devices → Quest 3 → Developer Mode → ON

### ADB installieren

**Option A: SideQuest (Einfachste)**
- [sidequestvr.com](https://sidequestvr.com) → Easy Installer
- ADB ist integriert

**Option B: Standalone ADB**
- [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools)
- PATH Umgebungsvariable setzen

**Verbindung testen:**

```bash
# USB-Kabel anschließen
adb devices

# Sollte zeigen:
# List of devices attached
# 1WMHH123456789    device
```

### ADB Reverse Tunnel

Für lokale Entwicklung:

```bash
# Port 5173 (Vite Dev Server) tunneln
adb reverse tcp:5173 tcp:5173

# Port 3000 (WebSocket Server) tunneln
adb reverse tcp:3000 tcp:3000
```

### Quest Browser

| Browser | WebXR | Passthrough | Empfehlung |
|---------|-------|-------------|------------|
| **Meta Quest Browser** | ✅ | ✅ | ⭐ Primär |
| Wolvic | ✅ | ✅ | Alternative |
| Firefox Reality | ❌ | ❌ | Eingestellt |

**URL im Quest Browser:**
```
https://localhost:5173
```

---

## 6. HTTPS Zertifikate

WebXR erfordert HTTPS. Optionen:

### Option A: vite-plugin-mkcert (Empfohlen)

Automatisch via Vite Plugin (siehe vite.config.ts oben).

### Option B: mkcert CLI

```bash
# Installation
brew install mkcert       # macOS
choco install mkcert      # Windows
sudo apt install mkcert   # Ubuntu

# Root CA installieren
mkcert -install

# Zertifikate erstellen
mkcert localhost 127.0.0.1 ::1 192.168.10.100

# Dateien: localhost+3.pem, localhost+3-key.pem
```

### Option C: Selbstsigniert (Nicht empfohlen)

Quest Browser zeigt Warnung, die manuell bestätigt werden muss.

---

## 7. Version Matrix

Getestete Kombinationen:

| Komponente | Minimum | Getestet | Latest |
|------------|---------|----------|--------|
| Node.js | 18 LTS | 20 LTS | 22 |
| SvelteKit | 2.0 | 2.0 | 2.x |
| Svelte | 4.0 | 4.2 | 5.x (Beta) |
| Three.js | 0.150 | 0.160 | 0.162 |
| Threlte | 7.0 | 7.3 | 7.x |
| Socket.io | 4.5 | 4.7 | 4.7 |
| ESP32 Core | 2.0 | 2.0.14 | 2.0.x |
| Quest OS | v57 | v62 | v67 |

---

## 8. Troubleshooting

### Node.js / npm

| Problem | Lösung |
|---------|--------|
| `EACCES` Permission Error | `sudo chown -R $USER ~/.npm` |
| Module nicht gefunden | `rm -rf node_modules && pnpm install` |
| Old Node version | `nvm install 20 && nvm use 20` |

### HTTPS / Zertifikate

| Problem | Lösung |
|---------|--------|
| Zertifikat nicht vertraut | `mkcert -install` ausführen |
| Quest zeigt SEC_ERROR | Zertifikat manuell akzeptieren |
| Port bereits belegt | `lsof -i :5173` → Prozess beenden |

### ADB / Quest

| Problem | Lösung |
|---------|--------|
| "no devices" | USB-Debugging erlauben (Popup auf Quest) |
| "unauthorized" | Quest abnehmen, neu verbinden, bestätigen |
| Reverse Tunnel funktioniert nicht | Quest mit PC im selben WiFi |

### ESP32 / Arduino

| Problem | Lösung |
|---------|--------|
| Upload fehlgeschlagen | BOOT Button beim Upload gedrückt halten |
| BNO055 nicht erkannt | I2C Adresse prüfen (0x28 oder 0x29) |
| WiFi verbindet nicht | 2.4 GHz WiFi verwenden |

---

## 9. Entwicklungsumgebung Checkliste

```bash
# 1. Node.js prüfen
node --version  # >= 20.x

# 2. pnpm installieren
npm install -g pnpm
pnpm --version  # >= 8.x

# 3. Git prüfen
git --version  # >= 2.40

# 4. VS Code Extensions installieren
code --install-extension svelte.svelte-vscode
code --install-extension platformio.platformio-ide
code --install-extension dbaeumer.vscode-eslint

# 5. ADB prüfen
adb version

# 6. Quest verbinden & Developer Mode
adb devices  # Quest sollte gelistet sein

# 7. Projekt klonen & starten
git clone <repo>
cd icaros-webxr
pnpm install
pnpm dev:adb
```

---

## Referenzen

- [SvelteKit Docs](https://kit.svelte.dev/docs)
- [Threlte Docs](https://threlte.xyz)
- [Three.js Docs](https://threejs.org/docs)
- [Socket.io Docs](https://socket.io/docs/v4/)
- [ESP32 Arduino Core](https://docs.espressif.com/projects/arduino-esp32)
- [PlatformIO Docs](https://docs.platformio.org)
- [Meta Quest Developer](https://developer.oculus.com)

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
