# Framework Options

> WebXR frameworks for the Neural Flight platform

**Status:** 🟢 Complete
**Last Updated:** January 2026

---

## Overview

The platform is **framework-agnostic**. Teams choose what fits their skills. Base is Vanilla Three.js.

```
┌─────────────────────────────────────┐
│       Neural Flight Platform        │
├─────────────────────────────────────┤
│ Threlte │ A-Frame │ R3F │ Custom   │
├─────────────────────────────────────┤
│            Three.js                 │
├─────────────────────────────────────┤
│           WebXR API                 │
└─────────────────────────────────────┘
```

---

## Comparison

| Framework | Best For | Learning Curve | Quest 3 |
|-----------|----------|----------------|---------|
| **Vanilla Three.js** | Max control | Medium | ✅ Excellent |
| **Threlte** | Svelte teams | Low | ✅ Good |
| **A-Frame** | Prototyping | Very low | ✅ Good |
| **React Three Fiber** | React teams | Medium | ✅ Good |
| **Babylon.js** | Gaming, Physics | Medium | ✅ Excellent |

---

## 1. Vanilla Three.js (Recommended)

```javascript
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';

const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.xr.enabled = true;
document.body.appendChild(VRButton.createButton(renderer));
```

✅ No dependencies, max performance, full control

---

## 2. Threlte (Svelte)

```bash
pnpm add @threlte/core @threlte/extras @threlte/xr three
```

```svelte
<script>
  import { Canvas } from '@threlte/core';
  import { XR, VRButton } from '@threlte/xr';
</script>

<Canvas>
  <XR>
    <slot />
  </XR>
</Canvas>
<VRButton />
```

✅ Declarative Svelte syntax, reactive state

---

## 3. A-Frame

```html
<script src="https://aframe.io/releases/1.6.0/aframe.min.js"></script>

<a-scene>
  <a-box position="0 1.6 -2" color="#00ff88"></a-box>
  <a-sky color="#1a1a2e"></a-sky>
</a-scene>
```

✅ HTML-based, very simple, great for prototypes

---

## 4. React Three Fiber

```jsx
import { Canvas } from '@react-three/fiber';
import { VRButton, XR } from '@react-three/xr';

function App() {
  return (
    <>
      <VRButton />
      <Canvas>
        <XR>
          <mesh><boxGeometry /><meshStandardMaterial /></mesh>
        </XR>
      </Canvas>
    </>
  );
}
```

✅ React ecosystem, good TypeScript support

---

## Recommendation by Use Case

| Use Case | Framework |
|----------|-----------|
| Core Platform | Vanilla Three.js |
| Svelte Team | Threlte |
| React Team | R3F |
| Quick Prototype | A-Frame |
| Complex Physics | Babylon.js |

---

## References

- [Three.js](https://threejs.org)
- [Threlte](https://threlte.xyz)
- [A-Frame](https://aframe.io)
- [React Three Fiber](https://docs.pmnd.rs/react-three-fiber)
- [Babylon.js](https://babylonjs.com)

---

*Part of the [Neural Flight](../README.md) project*
