# WebSocket Protocol

> Spezifikation der Kommunikation zwischen ESP32, Server und WebXR-Client

**Status:** 🟢 Complete  
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Das System verwendet WebSockets für bidirektionale Echtzeit-Kommunikation. Der Node.js Server fungiert als Hub zwischen Sensor (ESP32) und VR-Client (Quest Browser).

```
ESP32 ──WebSocket──▶ Server ──WebSocket──▶ Quest Browser
        (Client)      (Hub)                  (Client)
```

---

## Verbindungen

### ESP32 → Server

| Parameter | Wert |
|-----------|------|
| URL | `ws://192.168.10.100:3000` |
| Protokoll | WebSocket (ws://) |
| Reconnect | Automatisch, 5s Intervall |
| Heartbeat | Alle 10s |

### Quest Browser → Server

| Parameter | Wert |
|-----------|------|
| URL | `wss://192.168.10.100:3000` |
| Protokoll | WebSocket Secure (wss://) |
| Bibliothek | Socket.io Client |
| Room | `/vr` |

---

## Events & Payloads

### 1. Telemetrie (Sensor → Server → Client)

**Event:** `telemetry`  
**Frequenz:** 20 Hz (alle 50ms)  
**Richtung:** ESP32 → Server → Quest

```json
{
  "type": "telemetry",
  "timestamp": 1704067200000,
  "euler": {
    "x": 0.0,
    "y": 0.0,
    "z": 0.0
  },
  "quaternion": {
    "w": 1.0,
    "x": 0.0,
    "y": 0.0,
    "z": 0.0
  },
  "linearAccel": {
    "x": 0.0,
    "y": 0.0,
    "z": 0.0
  },
  "calibration": {
    "sys": 3,
    "gyro": 3,
    "accel": 3,
    "mag": 3
  }
}
```

**Felder:**

| Feld | Typ | Einheit | Beschreibung |
|------|-----|---------|--------------|
| `timestamp` | number | ms | Unix Timestamp (ESP32 millis) |
| `euler.x` | number | Grad | Heading (0-360°) |
| `euler.y` | number | Grad | Roll (-180 bis +180°) |
| `euler.z` | number | Grad | Pitch (-90 bis +90°) |
| `quaternion.w/x/y/z` | number | - | Quaternion für präzise Rotation |
| `linearAccel.x/y/z` | number | m/s² | Beschleunigung ohne Gravitation |
| `calibration.sys` | number | 0-3 | System-Kalibrierung (3 = optimal) |
| `calibration.gyro` | number | 0-3 | Gyro-Kalibrierung |
| `calibration.accel` | number | 0-3 | Accelerometer-Kalibrierung |
| `calibration.mag` | number | 0-3 | Magnetometer-Kalibrierung |

**TypeScript Interface:**

```typescript
interface TelemetryData {
  type: 'telemetry';
  timestamp: number;
  euler: { x: number; y: number; z: number };
  quaternion: { w: number; x: number; y: number; z: number };
  linearAccel: { x: number; y: number; z: number };
  calibration: {
    sys: number;  // 0-3
    gyro: number;
    accel: number;
    mag: number;
  };
}
```

---

### 2. Sensor Status (ESP32 → Server)

**Event:** `sensor:status`  
**Frequenz:** Bei Änderung  
**Richtung:** ESP32 → Server

```json
{
  "type": "sensor:status",
  "status": "connected",
  "battery": 85,
  "rssi": -45,
  "uptime": 3600000
}
```

| Feld | Typ | Beschreibung |
|------|-----|--------------|
| `status` | string | `connected`, `calibrating`, `error` |
| `battery` | number | Batterie-Level in % (falls verfügbar) |
| `rssi` | number | WiFi Signal Stärke in dBm |
| `uptime` | number | Laufzeit in ms |

---

### 3. Session Control (Server → Client)

**Event:** `session:start`  
**Richtung:** Server → Quest

```json
{
  "type": "session:start",
  "mode": "vr",
  "duration": 180000,
  "level": "sky"
}
```

**Event:** `session:end`  
**Richtung:** Server → Quest

```json
{
  "type": "session:end",
  "reason": "timeout",
  "stats": {
    "duration": 180000,
    "maxPitch": 35.2,
    "maxRoll": 22.1
  }
}
```

---

### 4. Calibration (Server → Client)

**Event:** `calibration:status`  
**Richtung:** Server → Quest

```json
{
  "type": "calibration:status",
  "ready": true,
  "stableFor": 2500,
  "requiredStability": 3000,
  "currentPitch": 1.2,
  "currentRoll": -0.8,
  "threshold": 3.0
}
```

| Feld | Typ | Beschreibung |
|------|-----|--------------|
| `ready` | boolean | Kalibrierung abgeschlossen |
| `stableFor` | number | ms stabil innerhalb Threshold |
| `requiredStability` | number | ms benötigt für "ready" |
| `currentPitch` | number | Aktueller Pitch in ° |
| `currentRoll` | number | Aktueller Roll in ° |
| `threshold` | number | Erlaubte Abweichung in ° |

---

### 5. Admin/Debug (Bidirektional)

**Event:** `admin:command`  
**Richtung:** Admin Dashboard → Server

```json
{
  "type": "admin:command",
  "command": "reset",
  "target": "sensor:1"
}
```

**Commands:**
- `reset` – Sensor/Session zurücksetzen
- `calibrate` – Kalibrierung erzwingen
- `setLevel` – Level wechseln
- `getStats` – Statistiken abrufen

---

## JSON Schema (Vollständig)

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "IcarosWebXRProtocol",
  "oneOf": [
    {
      "type": "object",
      "properties": {
        "type": { "const": "telemetry" },
        "timestamp": { "type": "number" },
        "euler": {
          "type": "object",
          "properties": {
            "x": { "type": "number", "minimum": 0, "maximum": 360 },
            "y": { "type": "number", "minimum": -180, "maximum": 180 },
            "z": { "type": "number", "minimum": -90, "maximum": 90 }
          },
          "required": ["x", "y", "z"]
        },
        "quaternion": {
          "type": "object",
          "properties": {
            "w": { "type": "number" },
            "x": { "type": "number" },
            "y": { "type": "number" },
            "z": { "type": "number" }
          },
          "required": ["w", "x", "y", "z"]
        },
        "calibration": {
          "type": "object",
          "properties": {
            "sys": { "type": "integer", "minimum": 0, "maximum": 3 },
            "gyro": { "type": "integer", "minimum": 0, "maximum": 3 },
            "accel": { "type": "integer", "minimum": 0, "maximum": 3 },
            "mag": { "type": "integer", "minimum": 0, "maximum": 3 }
          },
          "required": ["sys", "gyro", "accel", "mag"]
        }
      },
      "required": ["type", "timestamp", "euler", "quaternion", "calibration"]
    },
    {
      "type": "object",
      "properties": {
        "type": { "const": "session:start" },
        "mode": { "enum": ["ar", "vr"] },
        "duration": { "type": "number" },
        "level": { "type": "string" }
      },
      "required": ["type", "mode"]
    },
    {
      "type": "object",
      "properties": {
        "type": { "const": "session:end" },
        "reason": { "enum": ["timeout", "user", "error", "target"] }
      },
      "required": ["type", "reason"]
    }
  ]
}
```

---

## Beispiel: ESP32 Arduino Code

```cpp
#include <ArduinoJson.h>
#include <WebSocketsClient.h>

WebSocketsClient webSocket;
StaticJsonDocument<384> doc;

void sendTelemetry(sensors_event_t* euler, imu::Quaternion* quat) {
  doc.clear();
  doc["type"] = "telemetry";
  doc["timestamp"] = millis();
  
  JsonObject eulerObj = doc.createNestedObject("euler");
  eulerObj["x"] = euler->orientation.x;
  eulerObj["y"] = euler->orientation.y;
  eulerObj["z"] = euler->orientation.z;
  
  JsonObject quatObj = doc.createNestedObject("quaternion");
  quatObj["w"] = quat->w();
  quatObj["x"] = quat->x();
  quatObj["y"] = quat->y();
  quatObj["z"] = quat->z();
  
  JsonObject calObj = doc.createNestedObject("calibration");
  uint8_t sys, gyro, accel, mag;
  bno.getCalibration(&sys, &gyro, &accel, &mag);
  calObj["sys"] = sys;
  calObj["gyro"] = gyro;
  calObj["accel"] = accel;
  calObj["mag"] = mag;
  
  String output;
  serializeJson(doc, output);
  webSocket.sendTXT(output);
}
```

---

## Beispiel: TypeScript Client

```typescript
import { io, Socket } from 'socket.io-client';
import { writable } from 'svelte/store';
import type { TelemetryData } from './types';

class SensorService {
  private socket: Socket;
  public telemetry = writable<TelemetryData | null>(null);

  constructor(serverUrl: string) {
    this.socket = io(serverUrl, {
      transports: ['websocket'],
      reconnection: true,
      reconnectionDelay: 1000,
    });

    this.socket.on('telemetry', (data: TelemetryData) => {
      this.telemetry.set(data);
    });

    this.socket.on('connect', () => {
      console.log('Connected to sensor server');
      this.socket.emit('join', { room: 'vr' });
    });
  }

  disconnect() {
    this.socket.disconnect();
  }
}

export const sensorService = new SensorService('wss://192.168.10.100:3000');
```

---

## Beispiel: Node.js Server

```typescript
import { Server } from 'socket.io';
import { createServer } from 'http';

const httpServer = createServer();
const io = new Server(httpServer, {
  cors: { origin: '*' }
});

// Sensor namespace
const sensorNs = io.of('/sensor');
sensorNs.on('connection', (socket) => {
  console.log('Sensor connected:', socket.id);
  
  socket.on('telemetry', (data) => {
    // Broadcast zu allen VR-Clients
    io.of('/vr').emit('telemetry', data);
  });
});

// VR Client namespace
const vrNs = io.of('/vr');
vrNs.on('connection', (socket) => {
  console.log('VR client connected:', socket.id);
  
  socket.on('join', ({ room }) => {
    socket.join(room);
  });
});

httpServer.listen(3000, () => {
  console.log('WebSocket server running on :3000');
});
```

---

## Fehlerbehandlung

### Verbindungsabbruch

| Szenario | Client-Verhalten | Server-Verhalten |
|----------|------------------|------------------|
| ESP32 verliert WiFi | Auto-Reconnect 5s | Timeout nach 10s, Warnung an VR |
| Quest verliert Verbindung | Auto-Reconnect 1s | Session pausieren |
| Server neu gestartet | Alle Clients reconnecten | State reset |

### Fehler-Events

```json
{
  "type": "error",
  "code": "SENSOR_DISCONNECTED",
  "message": "Sensor connection lost",
  "timestamp": 1704067200000
}
```

| Code | Beschreibung | Aktion |
|------|--------------|--------|
| `SENSOR_DISCONNECTED` | Sensor nicht erreichbar | AR-Fallback |
| `CALIBRATION_FAILED` | Sensor nicht kalibriert | Anweisung zeigen |
| `SESSION_TIMEOUT` | Zeit abgelaufen | Session beenden |
| `INVALID_DATA` | Korrupte Telemetrie | Frame skippen |

---

## Performance-Optimierungen

### 1. Message Batching (optional)

Für höhere Frequenzen (>50 Hz):

```json
{
  "type": "telemetry_batch",
  "samples": [
    { "t": 0, "e": [0, 0, 0], "q": [1, 0, 0, 0] },
    { "t": 25, "e": [0.1, 0.2, 0.1], "q": [0.99, 0.01, 0.01, 0] },
    { "t": 50, "e": [0.2, 0.4, 0.2], "q": [0.98, 0.02, 0.02, 0] }
  ]
}
```

### 2. Binary Protocol (optional)

Für minimale Latenz:

```
Byte 0:     Message Type (0x01 = Telemetry)
Bytes 1-4:  Timestamp (uint32)
Bytes 5-8:  Euler X (float32)
Bytes 9-12: Euler Y (float32)
...
```

---

## Referenzen

- [Socket.io Documentation](https://socket.io/docs/v4/)
- [WebSocket API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [ArduinoJson](https://arduinojson.org/)
- [JSON Schema](https://json-schema.org/)

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
