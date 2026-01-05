# AR/MR Features

> Passthrough, Spatial Anchors und Mixed Reality auf Quest 3

**Status:** 🟢 Complete
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Die Meta Quest 3 unterstützt Mixed Reality (MR) Features über die WebXR API. Diese werden für das **AR-Onboarding** und die **Kalibrierungsphase** genutzt:

1. **Passthrough** – Sicht auf die reale Umgebung
2. **Ghost Model Overlay** – 3D-Wireframe des ICAROS über Passthrough
3. **Spatial Anchors** – Verankerung virtueller Objekte im Raum
4. **Hit-Testing** – Erkennung von Oberflächen

---

## WebXR Session Types

| Session Type | Beschreibung | Use Case |
|--------------|--------------|----------|
| `immersive-vr` | Vollständig virtuell | Flug-Erlebnis |
| `immersive-ar` | Passthrough + Virtuell | Kalibrierung, Onboarding |
| `inline` | Im Browser-Fenster | 3D-Preview |

### Session Flow für Neural Flight

```
┌─────────────────┐
│  Start Screen   │
│    (inline)     │
└────────┬────────┘
         │ "Enter AR"
         ▼
┌─────────────────┐
│  Calibration    │
│ (immersive-ar)  │
│   Passthrough   │
└────────┬────────┘
         │ Balance OK
         ▼
┌─────────────────┐
│  Flight Mode    │
│ (immersive-vr)  │
│   Full VR       │
└─────────────────┘
```

---

## 1. Passthrough aktivieren

### Three.js Setup

```javascript
import * as THREE from 'three';
import { ARButton } from 'three/addons/webxr/ARButton.js';

// Renderer für Passthrough konfigurieren
const renderer = new THREE.WebGLRenderer({
  antialias: true,
  alpha: true  // WICHTIG: Transparent für Passthrough
});
renderer.setClearColor(0x000000, 0); // Fully transparent
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.xr.enabled = true;

document.body.appendChild(renderer.domElement);

// AR Button mit Features
const arButton = ARButton.createButton(renderer, {
  requiredFeatures: ['local-floor'],
  optionalFeatures: ['hit-test', 'anchors', 'plane-detection']
});
document.body.appendChild(arButton);
```

### Scene ohne Background

```javascript
const scene = new THREE.Scene();
// KEIN scene.background setzen! Muss transparent bleiben

// Lighting für AR (schwächer, um mit Realität zu harmonieren)
const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
scene.add(ambientLight);
```

### Feature Detection

```javascript
// Prüfen ob AR unterstützt wird
async function checkARSupport() {
  if (!navigator.xr) {
    console.log('WebXR not supported');
    return false;
  }

  const supported = await navigator.xr.isSessionSupported('immersive-ar');
  console.log('AR supported:', supported);
  return supported;
}
```

---

## 2. Ghost Model Overlay

Das "Ghost Model" ist ein halbtransparentes 3D-Modell des ICAROS, das dem Nutzer hilft, sich korrekt zu positionieren.

### Wireframe Material

```javascript
// Semi-transparentes Wireframe für Ghost Effect
const ghostMaterial = new THREE.MeshBasicMaterial({
  color: 0x00ff88,
  wireframe: true,
  transparent: true,
  opacity: 0.5,
});

// ICAROS 3D-Modell laden
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

const loader = new GLTFLoader();
loader.load('models/icaros-ghost.glb', (gltf) => {
  const ghostModel = gltf.scene;

  // Alle Meshes zu Wireframe konvertieren
  ghostModel.traverse((child) => {
    if (child.isMesh) {
      child.material = ghostMaterial;
    }
  });

  // Position relativ zum Nutzer
  ghostModel.position.set(0, 0, -2);
  scene.add(ghostModel);
});
```

### Pulsierender Effekt

```javascript
let pulseTime = 0;

function animate() {
  pulseTime += 0.02;

  // Pulsierender Opacity-Effekt
  ghostMaterial.opacity = 0.3 + Math.sin(pulseTime) * 0.2;

  renderer.render(scene, camera);
}
```

---

## 3. Spatial Anchors

Spatial Anchors verankern virtuelle Objekte an festen Positionen im Raum.

### Anchor erstellen

```javascript
let currentAnchor = null;

async function createAnchor(position, orientation) {
  const session = renderer.xr.getSession();

  if (!session || !session.requestAnchor) {
    console.log('Anchors not supported');
    return null;
  }

  const referenceSpace = renderer.xr.getReferenceSpace();

  try {
    const anchor = await session.requestAnchor(
      new XRRigidTransform(position, orientation),
      referenceSpace
    );
    console.log('Anchor created:', anchor);
    return anchor;
  } catch (error) {
    console.error('Anchor creation failed:', error);
    return null;
  }
}
```

### Objekt an Anchor binden

```javascript
function updateAnchoredObject(frame, anchor, object) {
  const pose = frame.getPose(anchor.anchorSpace, renderer.xr.getReferenceSpace());

  if (pose) {
    object.matrix.fromArray(pose.transform.matrix);
    object.matrix.decompose(object.position, object.quaternion, object.scale);
  }
}
```

---

## 4. Hit-Testing

Hit-Testing ermöglicht die Erkennung von Oberflächen in der realen Welt.

### Hit-Test Source anfordern

```javascript
let hitTestSource = null;

renderer.xr.addEventListener('sessionstart', async () => {
  const session = renderer.xr.getSession();

  // Hit-Test anfordern
  const viewerSpace = await session.requestReferenceSpace('viewer');
  hitTestSource = await session.requestHitTestSource({
    space: viewerSpace
  });
});
```

### Hit-Test im Render-Loop

```javascript
function animate(timestamp, frame) {
  if (frame && hitTestSource) {
    const hitTestResults = frame.getHitTestResults(hitTestSource);

    if (hitTestResults.length > 0) {
      const hit = hitTestResults[0];
      const pose = hit.getPose(renderer.xr.getReferenceSpace());

      if (pose) {
        // Reticle (Ziel-Marker) an Hit-Position
        reticle.visible = true;
        reticle.matrix.fromArray(pose.transform.matrix);
      }
    } else {
      reticle.visible = false;
    }
  }

  renderer.render(scene, camera);
}
```

---

## 5. Kalibrierung UI Konzept

### Balance-Check Anzeige

```javascript
// HUD für Pitch/Roll Visualisierung
const hudCanvas = document.createElement('canvas');
hudCanvas.width = 256;
hudCanvas.height = 256;
const hudContext = hudCanvas.getContext('2d');

function drawBalanceIndicator(pitch, roll) {
  hudContext.clearRect(0, 0, 256, 256);

  // Hintergrund
  hudContext.fillStyle = 'rgba(0, 0, 0, 0.5)';
  hudContext.beginPath();
  hudContext.arc(128, 128, 100, 0, Math.PI * 2);
  hudContext.fill();

  // Toleranz-Zone (±3°)
  hudContext.strokeStyle = 'rgba(0, 255, 136, 0.5)';
  hudContext.lineWidth = 2;
  hudContext.beginPath();
  hudContext.arc(128, 128, 30, 0, Math.PI * 2);
  hudContext.stroke();

  // Aktueller Punkt
  const x = 128 + roll * 3; // Skalierung
  const y = 128 + pitch * 3;
  const inZone = Math.abs(pitch) < 3 && Math.abs(roll) < 3;

  hudContext.fillStyle = inZone ? '#00ff88' : '#ff4444';
  hudContext.beginPath();
  hudContext.arc(x, y, 10, 0, Math.PI * 2);
  hudContext.fill();

  return inZone;
}
```

### Balance Detection

```javascript
let balanceStartTime = null;
const BALANCE_DURATION = 2000; // 2 Sekunden stabil

function checkBalance(pitch, roll) {
  const isBalanced = Math.abs(pitch) < 3 && Math.abs(roll) < 3;

  if (isBalanced) {
    if (!balanceStartTime) {
      balanceStartTime = Date.now();
    } else if (Date.now() - balanceStartTime > BALANCE_DURATION) {
      // Kalibrierung erfolgreich!
      onCalibrationComplete();
    }
  } else {
    balanceStartTime = null;
  }
}

function onCalibrationComplete() {
  console.log('Calibration complete!');
  // Audio Feedback
  playSound('calibration-complete.mp3');
  // Transition zu VR
  transitionToVR();
}
```

---

## Limitationen

| Feature | Status | Hinweis |
|---------|--------|---------|
| Passthrough | ✅ Funktioniert | `alpha: true` + `immersive-ar` |
| Spatial Anchors | ✅ Funktioniert | Benötigt Quest OS 46+ |
| Hit-Testing | ✅ Funktioniert | `optionalFeatures: ['hit-test']` |
| Plane Detection | ⚠️ Eingeschränkt | Nur grobe Erkennung |
| Mesh Detection | ❌ Nicht verfügbar | Nicht in WebXR |
| Raw Camera Access | ❌ Nicht verfügbar | Kein Pixel-Zugriff |
| Hand Tracking | ⚠️ Nur in VR | Funktioniert nicht in AR |
| QR-Code Scanning | ❌ Nicht möglich | Kein Camera-Zugriff |

### Workarounds

**Für Hand Tracking in AR:**
```javascript
// Nicht möglich in immersive-ar
// Workaround: Controller verwenden oder nur in VR nutzen
```

**Für präzise Oberflächen:**
```javascript
// Plane Detection ist grob
// Alternative: Manuelle Kalibrierung durch Nutzer
```

---

## Reality Accelerator Toolkit (RATK)

RATK ist Metas offizielles Three.js Toolkit für Quest MR Features.

### Installation

```bash
npm install @meta-quest/reality-accelerator
```

### Verwendung

```javascript
import { RealityAccelerator } from '@meta-quest/reality-accelerator';

const ratk = new RealityAccelerator(renderer);

// Anchors verwalten
ratk.onAnchorCreate((anchor) => {
  console.log('New anchor:', anchor);
});

// Plane Detection
ratk.onPlaneDetect((plane) => {
  // Plane visualisieren
  const geometry = new THREE.PlaneGeometry(plane.width, plane.height);
  const mesh = new THREE.Mesh(geometry, planeMaterial);
  scene.add(mesh);
});
```

**Hinweis:** RATK ist optional. Für Basic Passthrough reicht die native WebXR API.

---

## Referenzen

- [WebXR AR Module](https://immersive-web.github.io/webxr-ar-module/)
- [WebXR Hit Test](https://immersive-web.github.io/hit-test/)
- [WebXR Anchors](https://immersive-web.github.io/anchors/)
- [Reality Accelerator Toolkit](https://github.com/meta-quest/reality-accelerator-toolkit)
- [Meta Quest MR Docs](https://developer.oculus.com/documentation/web/webxr-mixed-reality/)
- [Three.js AR Examples](https://threejs.org/examples/?q=webxr#webxr_ar_cones)

---

*Teil des [Neural Flight](../README.md) Projekts | Futurium gGmbH*
