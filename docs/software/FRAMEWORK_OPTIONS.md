# Framework-Optionen für Teams

> Übersicht über WebXR-Frameworks für die Neural Flight Plattform

**Status:** 🟢 Complete
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Die Neural Flight Plattform ist **framework-agnostisch**. Teams können das Framework wählen, das am besten zu ihren Skills passt. Die Basis ist Vanilla Three.js – alle anderen Frameworks bauen darauf auf.

```
┌─────────────────────────────────────────────────┐
│            Neural Flight Platform               │
├─────────────────────────────────────────────────┤
│   Threlte    │   A-Frame   │   R3F    │ Custom │
├─────────────────────────────────────────────────┤
│                  Three.js                       │
├─────────────────────────────────────────────────┤
│                 WebXR API                       │
└─────────────────────────────────────────────────┘
```

---

## Vergleichstabelle

| Framework | Basis | Ideal für | Lernkurve | Quest 3 Support |
|-----------|-------|-----------|-----------|-----------------|
| **Vanilla Three.js** | - | Maximale Kontrolle | Mittel | ✅ Excellent |
| **Threlte** | Svelte | Svelte-Teams | Niedrig | ✅ Gut |
| **A-Frame** | HTML | Prototyping, Beginners | Sehr niedrig | ✅ Gut |
| **React Three Fiber** | React | React-Teams | Mittel | ✅ Gut |
| **Babylon.js** | - | Enterprise, Gaming | Mittel | ✅ Excellent |

---

## 1. Vanilla Three.js (Empfohlen)

**Für:** Core Platform, Performance-kritische Anwendungen

### Pro
- ✅ Keine zusätzlichen Dependencies
- ✅ Maximale Performance
- ✅ Vollständige WebXR-Kontrolle
- ✅ Größte Community & Ressourcen
- ✅ Direkte Nutzung aller Three.js Examples

### Contra
- ❌ Mehr Boilerplate-Code
- ❌ Manuelles State Management
- ❌ Kein reaktives Rendering

### Schnellstart

```javascript
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';

const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.xr.enabled = true;
document.body.appendChild(VRButton.createButton(renderer));
```

**Dokumentation:** [THREEJS_WEBXR_GUIDE.md](./THREEJS_WEBXR_GUIDE.md)

---

## 2. Threlte (Svelte)

**Für:** Teams mit Svelte-Erfahrung, UI-lastige VR-Apps

### Pro
- ✅ Deklarative Svelte-Syntax
- ✅ Reaktives State Management
- ✅ `@threlte/xr` Package für WebXR
- ✅ Svelte 5 Runes Support
- ✅ Gute SvelteKit-Integration

### Contra
- ❌ Zusätzliche Abstraktionsschicht
- ❌ Kleinere Community als React
- ❌ XR-Features teilweise experimentell

### Schnellstart

```bash
pnpm add @threlte/core @threlte/extras @threlte/xr three
```

```svelte
<script>
  import { Canvas } from '@threlte/core';
  import { XR, VRButton } from '@threlte/xr';
  import Scene from './Scene.svelte';
</script>

<Canvas>
  <XR>
    <Scene />
  </XR>
</Canvas>
<VRButton />
```

**Dokumentation:** [threlte.xyz/docs/reference/xr](https://threlte.xyz/docs/reference/xr)

---

## 3. A-Frame

**Für:** Rapid Prototyping, Beginners, Content-Creator

### Pro
- ✅ HTML-basiert, sehr einfach
- ✅ Große Component-Library
- ✅ WebXR out-of-the-box
- ✅ Ideal für schnelle Prototypen
- ✅ Entity-Component-System

### Contra
- ❌ Performance-Overhead
- ❌ Weniger Kontrolle
- ❌ Schwieriger zu debuggen
- ❌ Abstraktion kann hinderlich sein

### Schnellstart

```html
<script src="https://aframe.io/releases/1.6.0/aframe.min.js"></script>

<a-scene>
  <a-box position="0 1.6 -2" color="#00ff88" animation="property: rotation; to: 0 360 0; loop: true; dur: 3000"></a-box>
  <a-sky color="#1a1a2e"></a-sky>
</a-scene>
```

**Dokumentation:** [aframe.io/docs](https://aframe.io/docs)

---

## 4. React Three Fiber (R3F)

**Für:** React-Teams, komplexe UIs

### Pro
- ✅ React-Ökosystem
- ✅ Deklarative Syntax
- ✅ `@react-three/xr` für WebXR
- ✅ Große Community
- ✅ Gute TypeScript-Unterstützung

### Contra
- ❌ React-Overhead
- ❌ Bundlesize größer
- ❌ Reconciler kann Performance kosten

### Schnellstart

```bash
npm install three @react-three/fiber @react-three/xr
```

```jsx
import { Canvas } from '@react-three/fiber';
import { VRButton, XR } from '@react-three/xr';

function App() {
  return (
    <>
      <VRButton />
      <Canvas>
        <XR>
          <mesh position={[0, 1.6, -2]}>
            <boxGeometry />
            <meshStandardMaterial color="#00ff88" />
          </mesh>
        </XR>
      </Canvas>
    </>
  );
}
```

**Dokumentation:** [docs.pmnd.rs/react-three-fiber](https://docs.pmnd.rs/react-three-fiber)

---

## 5. Babylon.js (Alternative Engine)

**Für:** Enterprise, komplexe Physik, Gaming

### Pro
- ✅ Vollständige Game Engine
- ✅ Eingebaute Physik-Engine
- ✅ WebXR nativ supported
- ✅ Playground & Inspector
- ✅ TypeScript-first

### Contra
- ❌ Nicht Three.js-kompatibel
- ❌ Größere Lernkurve
- ❌ Anderes Ökosystem

### Hinweis

Babylon.js ist eine **Alternative** zu Three.js, kein Wrapper. Wähle Babylon wenn:
- Komplexe Physik-Simulation nötig ist
- Enterprise-Support gewünscht ist
- Das Team bereits Babylon-Erfahrung hat

**Dokumentation:** [doc.babylonjs.com](https://doc.babylonjs.com)

---

## Empfehlungen nach Use Case

| Use Case | Empfohlenes Framework |
|----------|----------------------|
| Core Platform | Vanilla Three.js |
| Svelte-Team | Threlte |
| React-Team | React Three Fiber |
| Quick Prototype | A-Frame |
| Complex Physics | Babylon.js |
| Maximum Performance | Vanilla Three.js |
| Beginners | A-Frame → Vanilla |

---

## Integration mit Neural Flight

Alle Frameworks müssen das gemeinsame **Sensor-Protokoll** unterstützen:

```typescript
// Sensor-Daten Interface (alle Frameworks)
interface SensorData {
  timestamp: number;
  quat: { w: number; x: number; y: number; z: number };
  euler?: { pitch: number; roll: number; yaw: number };
  calibration: 0 | 1 | 2 | 3;
}
```

Die WebSocket-Verbindung zum ESP32 ist framework-unabhängig und kann in jedem Framework identisch implementiert werden.

---

## Referenzen

- [Three.js](https://threejs.org) - Basis-Engine
- [Threlte](https://threlte.xyz) - Svelte + Three.js
- [A-Frame](https://aframe.io) - Web VR Framework
- [React Three Fiber](https://docs.pmnd.rs/react-three-fiber) - React + Three.js
- [Babylon.js](https://babylonjs.com) - Alternative Engine

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
