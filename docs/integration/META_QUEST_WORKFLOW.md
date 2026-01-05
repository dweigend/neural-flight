# Meta Quest 3 Developer Workflow

> Complete workflow: Code → Quest 3 Test

**Status:** 🟢 Complete
**Last Updated:** January 2026

---

## Overview

```
Dev PC (localhost) ──▶ ADB Tunnel ──▶ Quest 3 (Browser)
```

---

## 1. Enable Developer Mode

1. Create account at [developer.oculus.com](https://developer.oculus.com)
2. Open **Meta Quest App** on smartphone
3. **Devices** → Quest 3 → **Headset Settings** → **Developer Mode** → ON
4. Restart Quest 3

---

## 2. ADB Setup

### Install ADB

```bash
# macOS
brew install android-platform-tools

# Windows: Download from developer.android.com/tools/releases/platform-tools
```

### First Connection (USB)

```bash
adb devices
# Put on Quest and confirm USB debugging dialog!
```

---

## 3. HTTPS with mkcert

WebXR requires HTTPS.

```bash
# Install
brew install mkcert
mkcert -install

# Generate certificates
mkcert localhost 127.0.0.1 ::1
```

### Vite Config

```javascript
// vite.config.js
import fs from 'fs';

export default {
  server: {
    https: {
      key: fs.readFileSync('localhost+2-key.pem'),
      cert: fs.readFileSync('localhost+2.pem'),
    },
    host: true,
    port: 5173,
  }
};
```

---

## 4. ADB Reverse Tunnel

```bash
adb reverse tcp:5173 tcp:5173
adb reverse tcp:3000 tcp:3000  # WebSocket
```

Then open in Quest Browser: `https://localhost:5173`

---

## 5. Wireless ADB

```bash
# 1. Connect via USB first
adb tcpip 5555

# 2. Find Quest IP (Settings → WiFi → connected network)
# 3. Disconnect USB, connect wirelessly
adb connect 192.168.1.XXX:5555

# 4. Set up reverse tunnel as usual
adb reverse tcp:5173 tcp:5173
```

---

## 6. Remote Debugging

1. Open Quest Browser, navigate to your page
2. On PC: Open Chrome → `chrome://inspect/#devices`
3. Find Quest Browser tab → click **inspect**

---

## Quick Start Script

```bash
#!/bin/bash
adb reverse tcp:5173 tcp:5173
adb reverse tcp:3000 tcp:3000
pnpm dev --host
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `adb: no devices` | Check USB cable, enable Developer Mode |
| `unauthorized` | Put on Quest, confirm USB dialog |
| WebXR not available | Check HTTPS (must be secure context) |
| Black screen | Restart Quest Browser, clear cache |

---

*Part of the [Neural Flight](../README.md) project*
