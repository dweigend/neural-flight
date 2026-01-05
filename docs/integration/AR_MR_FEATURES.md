# AR/MR Features

> Passthrough, Spatial Anchors, and Mixed Reality on Quest 3

**Status:** 🟢 Complete
**Last Updated:** January 2026

---

## Overview

Quest 3 MR features for **AR onboarding** and **calibration phase**:

1. **Passthrough** – View of real environment
2. **Ghost Model Overlay** – 3D wireframe of ICAROS over passthrough
3. **Spatial Anchors** – Anchor virtual objects in space
4. **Hit-Testing** – Surface detection

---

## Session Flow

```
Start Screen (inline)
       │ "Enter AR"
       ▼
Calibration (immersive-ar) ── Passthrough
       │ Balance OK
       ▼
Flight Mode (immersive-vr) ── Full VR
```

---

## Passthrough Setup

```javascript
import * as THREE from 'three';
import { ARButton } from 'three/addons/webxr/ARButton.js';

const renderer = new THREE.WebGLRenderer({
  antialias: true,
  alpha: true  // Required for passthrough
});
renderer.setClearColor(0x000000, 0);
renderer.xr.enabled = true;

document.body.appendChild(ARButton.createButton(renderer, {
  requiredFeatures: ['local-floor'],
  optionalFeatures: ['hit-test', 'anchors']
}));
```

---

## Ghost Model Overlay

```javascript
const ghostMaterial = new THREE.MeshBasicMaterial({
  color: 0x00ff88,
  wireframe: true,
  transparent: true,
  opacity: 0.5,
});

// Load ICAROS model and apply ghost material
loader.load('models/icaros-ghost.glb', (gltf) => {
  gltf.scene.traverse((child) => {
    if (child.isMesh) child.material = ghostMaterial;
  });
  scene.add(gltf.scene);
});
```

---

## Balance Detection

```javascript
let balanceStartTime = null;
const BALANCE_DURATION = 2000; // 2 seconds stable

function checkBalance(pitch, roll) {
  const isBalanced = Math.abs(pitch) < 3 && Math.abs(roll) < 3;

  if (isBalanced) {
    if (!balanceStartTime) balanceStartTime = Date.now();
    else if (Date.now() - balanceStartTime > BALANCE_DURATION) {
      onCalibrationComplete();
    }
  } else {
    balanceStartTime = null;
  }
}
```

---

## Feature Support

| Feature | Status | Notes |
|---------|--------|-------|
| Passthrough | ✅ | `alpha: true` + `immersive-ar` |
| Spatial Anchors | ✅ | Quest OS 46+ |
| Hit-Testing | ✅ | `optionalFeatures: ['hit-test']` |
| Plane Detection | ⚠️ | Rough detection only |
| Hand Tracking | ⚠️ | VR only, not in AR |
| Camera Access | ❌ | Not available in WebXR |

---

## References

- [WebXR AR Module](https://immersive-web.github.io/webxr-ar-module/)
- [Reality Accelerator Toolkit](https://github.com/meta-quest/reality-accelerator-toolkit)
- [Meta Quest MR Docs](https://developer.oculus.com/documentation/web/webxr-mixed-reality/)

---

*Part of the [Neural Flight](../README.md) project*
