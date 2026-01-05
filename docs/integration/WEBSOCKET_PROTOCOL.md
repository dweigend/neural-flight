# WebSocket Protocol

> Communication specification between ESP32, Server, and WebXR Client

**Status:** 🟢 Complete  
**Last Updated:** January 2026

---

## Overview

```
ESP32 ──WebSocket──▶ Server ──WebSocket──▶ Quest Browser
        (Client)      (Hub)                  (Client)
```

---

## Connections

| Endpoint | URL | Protocol |
|----------|-----|----------|
| ESP32 → Server | `ws://192.168.10.100:3000` | WebSocket |
| Quest → Server | `wss://192.168.10.100:3000` | WebSocket Secure |

---

## Events & Payloads

### Telemetry (20 Hz)

**Event:** `telemetry`  
**Direction:** ESP32 → Server → Quest

```json
{
  "type": "telemetry",
  "timestamp": 1704067200000,
  "quaternion": { "w": 1.0, "x": 0.0, "y": 0.0, "z": 0.0 },
  "euler": { "x": 0.0, "y": 0.0, "z": 0.0 },
  "calibration": { "sys": 3, "gyro": 3, "accel": 3, "mag": 3 }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `quaternion.w/x/y/z` | number | Rotation quaternion |
| `euler.x/y/z` | number | Heading, Roll, Pitch in degrees |
| `calibration.*` | 0-3 | Calibration status (3 = optimal) |

### Session Control

**Event:** `session:start`
```json
{ "type": "session:start", "mode": "vr", "duration": 180000 }
```

**Event:** `session:end`
```json
{ "type": "session:end", "reason": "timeout" }
```

### Calibration Status

**Event:** `calibration:status`
```json
{
  "type": "calibration:status",
  "ready": true,
  "stableFor": 2500,
  "threshold": 3.0
}
```

---

## TypeScript Interface

```typescript
interface TelemetryData {
  type: 'telemetry';
  timestamp: number;
  quaternion: { w: number; x: number; y: number; z: number };
  euler: { x: number; y: number; z: number };
  calibration: { sys: number; gyro: number; accel: number; mag: number };
}
```

---

## ESP32 Example

```cpp
#include <ArduinoJson.h>
#include <WebSocketsClient.h>

void sendTelemetry() {
  StaticJsonDocument<256> doc;
  doc["type"] = "telemetry";
  doc["timestamp"] = millis();
  
  JsonObject quat = doc.createNestedObject("quaternion");
  quat["w"] = quaternion.w();
  quat["x"] = quaternion.x();
  quat["y"] = quaternion.y();
  quat["z"] = quaternion.z();
  
  String output;
  serializeJson(doc, output);
  webSocket.sendTXT(output);
}
```

---

## Client Example (TypeScript)

```typescript
import { io } from 'socket.io-client';
import { writable } from 'svelte/store';

export const telemetry = writable<TelemetryData | null>(null);

const socket = io('wss://192.168.10.100:3000');
socket.on('telemetry', (data) => telemetry.set(data));
```

---

## Error Handling

| Code | Description | Action |
|------|-------------|--------|
| `SENSOR_DISCONNECTED` | Sensor unreachable | AR fallback |
| `CALIBRATION_FAILED` | Sensor not calibrated | Show instructions |
| `SESSION_TIMEOUT` | Time expired | End session |

---

*Part of the [Neural Flight](../README.md) project*
