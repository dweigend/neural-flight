# Three.js WebXR Guide

> Native Three.js + WebXR setup for Quest 3

**Status:** 🟢 Complete
**Last Updated:** January 2026

---

## Overview

Vanilla Three.js as the **platform base** – teams can build on this or use their own frameworks (Threlte, A-Frame, etc.).

**Why Vanilla Three.js?**
- No framework dependencies
- Maximum control & performance
- Easy onboarding for new teams

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
  <style>body { margin: 0; overflow: hidden; }</style>
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

// Renderer
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.xr.enabled = true;
document.body.appendChild(renderer.domElement);
document.body.appendChild(VRButton.createButton(renderer));

// Scene & Camera
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x1a1a2e);
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(0, 1.6, 3);

// Lighting
scene.add(new THREE.AmbientLight(0xffffff, 0.5));
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(5, 10, 7);
scene.add(light);

// Test Cube
const cube = new THREE.Mesh(
  new THREE.BoxGeometry(0.5, 0.5, 0.5),
  new THREE.MeshStandardMaterial({ color: 0x00ff88 })
);
cube.position.set(0, 1.6, -2);
scene.add(cube);

// Animation
function animate() {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
renderer.setAnimationLoop(animate);
```

---

## Sensor Integration

### WebSocket Client

```javascript
const SENSOR_URL = 'ws://192.168.10.10:81';
let sensorData = { quat: { w: 1, x: 0, y: 0, z: 0 } };

const socket = new WebSocket(SENSOR_URL);
socket.onmessage = (e) => sensorData = JSON.parse(e.data);
socket.onclose = () => setTimeout(() => location.reload(), 2000);
```

### Apply Quaternion with Smoothing

```javascript
const targetQuat = new THREE.Quaternion();
const LERP = 0.15;

function animate() {
  const { w, x, y, z } = sensorData.quat;
  targetQuat.set(x, y, z, w);
  cube.quaternion.slerp(targetQuat, LERP);
  renderer.render(scene, camera);
}
```

---

## AR Mode (Passthrough)

```javascript
import { ARButton } from 'three/addons/webxr/ARButton.js';

const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
renderer.setClearColor(0x000000, 0);
renderer.xr.enabled = true;

document.body.appendChild(ARButton.createButton(renderer, {
  requiredFeatures: ['local-floor'],
  optionalFeatures: ['hit-test']
}));
```

---

## Performance Tips

| Aspect | Recommendation |
|--------|----------------|
| Target FPS | 72Hz (Quest default) |
| Draw Calls | < 100 per frame |
| Triangles | < 500k per frame |
| Textures | Power-of-2, max 2048² |

```javascript
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
renderer.shadowMap.enabled = false; // Only if needed
```

---

## References

- [Three.js Documentation](https://threejs.org/docs/)
- [Three.js WebXR Examples](https://threejs.org/examples/?q=webxr)
- [WebXR Device API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/WebXR_Device_API)
- [Meta WebXR First Steps](https://github.com/meta-quest/webxr-first-steps)

---

*Part of the [Neural Flight](../README.md) project*
