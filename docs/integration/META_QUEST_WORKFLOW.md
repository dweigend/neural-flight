# Meta Quest 3 Developer Workflow

> Vollständiger Workflow: Code → Quest 3 Test

**Status:** 🟢 Complete
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Dieser Guide beschreibt den kompletten Entwicklungs-Workflow für WebXR auf der Meta Quest 3. Von der ersten Einrichtung bis zum Remote-Debugging.

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Dev PC     │───▶│  ADB Tunnel │───▶│  Quest 3    │
│  (localhost)│    │  (USB/WiFi) │    │  (Browser)  │
└─────────────┘    └─────────────┘    └─────────────┘
```

---

## Voraussetzungen

- [ ] Meta Quest 3
- [ ] Meta Account mit Developer Mode
- [ ] ADB Platform Tools
- [ ] USB-C Kabel (für erste Verbindung)
- [ ] Node.js ≥20 (für Dev Server)
- [ ] mkcert (für HTTPS)

---

## 1. Developer Mode aktivieren

### Meta Developer Account erstellen

1. Gehe zu [developer.oculus.com](https://developer.oculus.com)
2. Logge dich mit deinem Meta Account ein
3. Akzeptiere die Developer Terms

### Developer Mode in Quest aktivieren

1. **Meta Quest App** auf Smartphone öffnen
2. **Geräte** → Quest 3 auswählen
3. **Headset-Einstellungen** → **Entwicklermodus**
4. **Entwicklermodus** aktivieren → Toggle ON
5. Quest 3 neustarten

> ⚠️ Ohne Developer Mode: Kein ADB, kein Sideloading, kein USB-Debugging

---

## 2. ADB Setup

### ADB installieren

**macOS:**
```bash
brew install android-platform-tools
```

**Windows:**
1. Download: [ADB Platform Tools](https://developer.android.com/tools/releases/platform-tools)
2. Entpacken nach `C:\adb\`
3. Zu PATH hinzufügen

**Linux:**
```bash
sudo apt install adb
```

### Erste Verbindung (USB)

```bash
# Quest per USB-C verbinden
adb devices

# Erwartete Ausgabe:
# List of devices attached
# 1WMHH123456789    device
```

> 💡 Beim ersten Verbinden: Quest aufsetzen und USB-Debugging-Dialog bestätigen!

### USB-Debugging autorisieren

Wenn `unauthorized` erscheint:
1. Quest aufsetzen
2. Dialog "USB-Debugging erlauben?" → **Immer erlauben** auswählen
3. `adb devices` erneut ausführen

---

## 3. HTTPS mit mkcert

WebXR benötigt eine **Secure Context** (HTTPS). Mit mkcert erstellst du vertrauenswürdige lokale Zertifikate.

### mkcert installieren

**macOS:**
```bash
brew install mkcert
mkcert -install  # Root CA installieren
```

**Windows:**
```powershell
choco install mkcert
mkcert -install
```

### Zertifikate generieren

```bash
# Im Projektverzeichnis
mkcert localhost 127.0.0.1 ::1

# Erstellt:
# - localhost+2.pem (Zertifikat)
# - localhost+2-key.pem (Private Key)
```

### Vite Dev Server konfigurieren

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import fs from 'fs';

export default defineConfig({
  server: {
    https: {
      key: fs.readFileSync('localhost+2-key.pem'),
      cert: fs.readFileSync('localhost+2.pem'),
    },
    host: true,  // Für externe Zugriffe
    port: 5173,
  }
});
```

### Einfacher HTTPS Server (ohne Vite)

```bash
# Mit npx (einmalige Installation)
npx http-server -S -C localhost+2.pem -K localhost+2-key.pem -p 5173

# Oder mit Python (falls installiert)
# python -m http.server --ssl-context localhost+2.pem 5173
```

---

## 4. ADB Reverse Tunnel

Der Reverse Tunnel leitet Quest-Anfragen an deinen lokalen Server weiter.

### USB Tunnel

```bash
# Einzelner Port
adb reverse tcp:5173 tcp:5173

# Mehrere Ports (z.B. für WebSocket)
adb reverse tcp:5173 tcp:5173
adb reverse tcp:3000 tcp:3000

# Tunnel überprüfen
adb reverse --list

# Tunnel entfernen
adb reverse --remove-all
```

### Im Quest Browser öffnen

1. Quest Browser öffnen
2. URL eingeben: `https://localhost:5173`
3. Bei Zertifikatswarnung: "Trotzdem fortfahren"

> 💡 Tipp: Quest Browser Lesezeichen anlegen für schnellen Zugriff

---

## 5. Wireless ADB (Kabellos)

Nach der initialen USB-Verbindung kann ADB kabellos verwendet werden.

### WiFi-Modus aktivieren

```bash
# Quest und PC müssen im selben WiFi sein!

# 1. Quest per USB verbinden
adb devices

# 2. TCP/IP Modus aktivieren
adb tcpip 5555

# 3. Quest IP-Adresse finden (Quest: Einstellungen → WLAN → verbundenes Netzwerk)
# Oder:
adb shell ip route | grep wlan0

# 4. USB-Kabel entfernen, drahtlos verbinden
adb connect 192.168.1.XXX:5555

# 5. Reverse Tunnel einrichten (wie oben)
adb reverse tcp:5173 tcp:5173
```

### Verbindung überprüfen

```bash
adb devices
# Sollte zeigen:
# 192.168.1.XXX:5555    device
```

### Verbindung trennen

```bash
adb disconnect 192.168.1.XXX:5555
```

> ⚠️ Wireless ADB ist langsamer als USB. Für schnelles Debugging: USB bevorzugen.

---

## 6. Remote Debugging (Chrome DevTools)

### Voraussetzungen

- Quest per ADB verbunden (USB oder WiFi)
- Chrome auf dem PC installiert

### Debugging starten

1. **Quest Browser** öffnen und zur WebXR-Seite navigieren
2. **Chrome auf PC** öffnen
3. URL eingeben: `chrome://inspect/#devices`
4. Unter "Remote Target" → Quest Browser Tab finden
5. **inspect** klicken

### Verfügbare Features

- Console (Logs, Errors)
- Elements (DOM)
- Network (Requests)
- Performance (Profiling)
- Sources (Debugging)

### Debug-Tipps

```javascript
// Im Code: Hilfreiche Logs für Remote-Debugging
console.log('[XR] Session started');
console.log('[Sensor] Data:', JSON.stringify(sensorData));
console.error('[Error] WebSocket failed');
```

---

## 7. Vollständiger Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    Development Workflow                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Terminal: pnpm dev (HTTPS Server starten)               │
│                    ↓                                         │
│  2. Terminal: adb reverse tcp:5173 tcp:5173                 │
│                    ↓                                         │
│  3. Quest Browser: https://localhost:5173                   │
│                    ↓                                         │
│  4. VR Button klicken → Immersive Session                   │
│                    ↓                                         │
│  5. Chrome DevTools: chrome://inspect (Debug)               │
│                    ↓                                         │
│  6. Code ändern → HMR → Quest Browser refresht              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Schnellstart-Script

```bash
#!/bin/bash
# start-dev.sh

# ADB Tunnel einrichten
adb reverse tcp:5173 tcp:5173
adb reverse tcp:3000 tcp:3000  # WebSocket

# Dev Server starten
pnpm dev --host
```

---

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| `adb: no devices` | USB-Kabel prüfen, Developer Mode aktivieren |
| `unauthorized` | Quest aufsetzen, USB-Dialog bestätigen |
| `connection refused` | Firewall prüfen, Port bereits belegt? |
| WebXR nicht verfügbar | HTTPS prüfen (muss Secure Context sein) |
| Zertifikatswarnung | Mit mkcert-Zertifikat beheben oder manuell akzeptieren |
| Schwarzer Bildschirm | Quest Browser neustarten, Cache löschen |
| Langsames Debugging | USB statt WiFi verwenden |
| ADB findet Quest nicht | Quest neustarten, ADB Server neustarten: `adb kill-server && adb start-server` |

### Quest Browser Cache löschen

1. Quest: **Einstellungen** → **Apps** → **Quest Browser**
2. **Speicher** → **Cache leeren**
3. Browser neu öffnen

---

## Referenzen

- [Meta Quest Developer Docs](https://developer.oculus.com/documentation/)
- [ADB Platform Tools](https://developer.android.com/tools/releases/platform-tools)
- [mkcert GitHub](https://github.com/FiloSottile/mkcert)
- [Chrome Remote Debugging](https://developer.chrome.com/docs/devtools/remote-debugging/)
- [WebXR First Steps (Meta)](https://github.com/meta-quest/webxr-first-steps)

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
