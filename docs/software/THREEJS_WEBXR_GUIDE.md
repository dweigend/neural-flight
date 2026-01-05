# Three.js WebXR Guide

> Native Three.js + WebXR Setup für Quest 3

**Status:** 🟢 Complete
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Dieses Dokument beschreibt das Vanilla Three.js Setup für WebXR-Anwendungen auf der Meta Quest 3. Es dient als **Plattform-Basis** – Teams können darauf aufbauen oder eigene Frameworks (Threlte, A-Frame, etc.) verwenden.

**Warum Vanilla Three.js?**
- Keine Framework-Abhängigkeiten
- Maximale Kontrolle
- Beste Performance
- Einfaches Onboarding für neue Teams

---

## Minimal Setup

### HTML Boilerplate

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neural Flight - WebXR</title>
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; }
  </style>
</head>
<body>
  <script type="importmap">
  {
    "imports": {
      "three": "https://cdn.jsdelivr.net/npm/three@0.170/build/three.module.js",
      "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.170/examples/jsm/"
    }
  }
  </script>
  <script type="module" src="main.js"></script>
</body>
</html>
```

### main.js - VR Scene

```javascript
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';

// === Renderer Setup ===
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.xr.enabled = true;
document.body.appendChild(renderer.domElement);
document.body.appendChild(VRButton.createButton(renderer));

// === Scene & Camera ===
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x1a1a2e);

const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.set(0, 1.6, 3); // Eye height ~1.6m

// === Lighting ===
const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
scene.add(ambientLight);

const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
directionalLight.position.set(5, 10, 7);
scene.add(directionalLight);

// === Test Object ===
const geometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
const material = new THREE.MeshStandardMaterial({ color: 0x00ff88 });
const cube = new THREE.Mesh(geometry, material);
cube.position.set(0, 1.6, -2);
scene.add(cube);

// === Floor Grid ===
const gridHelper = new THREE.GridHelper(10, 10, 0x444444, 0x222222);
scene.add(gridHelper);

// === Animation Loop ===
function animate() {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}

renderer.setAnimationLoop(animate);

// === Resize Handler ===
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
```

---

## Sensor-Daten Integration

### WebSocket Client für ESP32 Telemetrie

```javascript
// === WebSocket Connection ===
const SENSOR_URL = 'ws://192.168.1.100:81'; // ESP32 IP
let socket = null;
let sensorData = { quat: { w: 1, x: 0, y: 0, z: 0 } };

function connectSensor() {
  socket = new WebSocket(SENSOR_URL);

  socket.onopen = () => console.log('Sensor connected');
  socket.onclose = () => setTimeout(connectSensor, 2000); // Reconnect
  socket.onerror = (e) => console.error('Sensor error:', e);

  socket.onmessage = (event) => {
    try {
      sensorData = JSON.parse(event.data);
    } catch (e) {
      console.error('Parse error:', e);
    }
  };
}

connectSensor();
```

### Quaternion auf Objekt anwenden

```javascript
// In der animate() Funktion:
function animate() {
  // Sensor-Quaternion auf Objekt übertragen
  const { w, x, y, z } = sensorData.quat;
  cube.quaternion.set(x, y, z, w); // Three.js: (x, y, z, w)

  renderer.render(scene, camera);
}
```

### Smoothing (Lerp)

```javascript
// Smooth interpolation für flüssige Animation
const targetQuaternion = new THREE.Quaternion();
const LERP_FACTOR = 0.15; // 0.1 = smooth, 0.3 = responsive

function animate() {
  const { w, x, y, z } = sensorData.quat;
  targetQuaternion.set(x, y, z, w);

  // Spherical Linear Interpolation
  cube.quaternion.slerp(targetQuaternion, LERP_FACTOR);

  renderer.render(scene, camera);
}
```

---

## WebXR Session Types

### VR Mode (Immersive VR)

```javascript
import { VRButton } from 'three/addons/webxr/VRButton.js';

renderer.xr.enabled = true;
document.body.appendChild(VRButton.createButton(renderer));

// Optional: Reference Space
renderer.xr.setReferenceSpaceType('local-floor');
```

### AR Mode (Passthrough)

```javascript
import { ARButton } from 'three/addons/webxr/ARButton.js';

// Renderer für Passthrough konfigurieren
const renderer = new THREE.WebGLRenderer({
  antialias: true,
  alpha: true  // Transparent background
});
renderer.setClearColor(0x000000, 0); // Fully transparent
renderer.xr.enabled = true;

// AR Button mit Features
document.body.appendChild(ARButton.createButton(renderer, {
  requiredFeatures: ['local-floor'],
  optionalFeatures: ['hit-test', 'anchors']
}));
```

### Session Events

```javascript
renderer.xr.addEventListener('sessionstart', () => {
  console.log('XR Session started');
});

renderer.xr.addEventListener('sessionend', () => {
  console.log('XR Session ended');
});
```

---

## Performance Best Practices

### Quest 3 Optimierungen

| Aspekt | Empfehlung |
|--------|------------|
| **Target FPS** | 72Hz (default), 90Hz oder 120Hz möglich |
| **Draw Calls** | < 100 pro Frame |
| **Triangles** | < 500k pro Frame |
| **Textures** | Power-of-2, max 2048x2048 |
| **Antialias** | `{ antialias: true }` für Quest |

### Renderer-Optimierungen

```javascript
// Pixel Ratio begrenzen (Quest hat hohe native Auflösung)
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

// Tone Mapping für bessere Visuals
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.toneMappingExposure = 1;

// Shadowmap nur wenn nötig
renderer.shadowMap.enabled = false;
```

### Object Pooling

```javascript
// Objekte wiederverwenden statt neu erstellen
const pool = [];
const POOL_SIZE = 100;

function getFromPool() {
  return pool.length > 0 ? pool.pop() : createNewObject();
}

function returnToPool(obj) {
  if (pool.length < POOL_SIZE) {
    obj.visible = false;
    pool.push(obj);
  }
}
```

---

## Debugging

### Browser Console im Quest

```javascript
// Custom logger für Remote-Debugging
const log = (...args) => {
  console.log(...args);
  // Optional: An Server senden
  // fetch('/log', { method: 'POST', body: JSON.stringify(args) });
};
```

### XR Frame Info

```javascript
function animate(timestamp, frame) {
  if (frame) {
    const session = renderer.xr.getSession();
    const pose = frame.getViewerPose(renderer.xr.getReferenceSpace());

    if (pose) {
      // Head position/rotation verfügbar
      const { position, orientation } = pose.transform;
      log('Head:', position.x, position.y, position.z);
    }
  }

  renderer.render(scene, camera);
}
```

---

## Projektstruktur

```
project/
├── index.html
├── main.js
├── src/
│   ├── scene.js       # Scene setup
│   ├── sensor.js      # WebSocket client
│   ├── physics.js     # Flight model
│   └── utils.js       # Helper functions
├── assets/
│   ├── models/
│   └── textures/
└── server/
    └── sensor-relay.js  # Optional: Node.js Relay
```

---

## Nächste Schritte

1. **M2: WebXR Hello World** – Diese Datei als Basis
2. **M3: Sensor-to-VR Bridge** – WebSocket Integration testen
3. **M4: AR Calibration** – Passthrough aktivieren

---

## Referenzen

- [Three.js Documentation](https://threejs.org/docs/)
- [Three.js WebXR Examples](https://threejs.org/examples/?q=webxr)
- [WebXR Device API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/WebXR_Device_API)
- [Meta WebXR First Steps](https://github.com/meta-quest/webxr-first-steps)

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
