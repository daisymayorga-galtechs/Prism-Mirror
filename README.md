# Prism Mirror — Kinetic Glass & Hand Vision

A single-file, zero-build-step interactive WebGL application powered by **Three.js** and **MediaPipe Tasks Vision**. **Prism Mirror** turns your browser into a physical optical mirror where a levitating, light-refracting 3D glass polyhedron bends your live webcam reflection (or a procedural aurora light-orb field) in real time—controlled directly via mouse/touch dragging or computer-vision hand gestures.

## ✨ Features

* **Physical 3D Optical Refraction (Three.js):** Uses `MeshPhysicalMaterial` (`transmission: 1.0`, `thickness`, `ior`, `roughness`, and `clearcoat`) so the central geometric prism and orbiting companion shards bend, magnify, and disperse whatever is behind them at 60 FPS.
* **Webcam Mirror & Procedural Fallback:** Starts immediately with an animated aurora light-orb mirror field; clicking **Enable Camera & Hand Tracking** activates `getUserMedia` and maps a horizontally flipped `VideoTexture` onto the full-bleed mirror backdrop.
* **Computer Vision Hand Control (MediaPipe `HandLandmarker`):** Tracks 21 hand landmarks in real time on the GPU:
  * **Open Hand Glide:** Move your palm across the frame to smoothly levitate the 3D glass prism across your reflection.
  * **Thumb-to-Index Pinch Grab:** Pinch your thumb and index finger together to lock onto the prism, ignite an iridescent cyan/violet rim glow, and spin it with wrist twist and hand motion.
  * **Geometric Hand-Skeleton HUD:** Overlays a subtle, translucent wireframe skeleton and dynamic geometric reticle over your detected hand.
* **Direct Mouse & Touch Spring Physics:** Click-and-drag (or touch-drag) directly on the 3D glass prism via raycasting to move and spin it with spring-damped inertia, or scroll the mouse wheel to scale its size.
* **Dynamic Atmospheric Backdrops:** Switch seamlessly between **Clear Prism** (floating golden motes), **Misty Rain** (soft drizzle streaks and frosted glass), and **Storm Glass** (high-IOR diagonal rain streaks and deep storm lighting).

## 🚀 Getting Started

Because **Prism Mirror** loads Three.js, Tailwind CSS, and MediaPipe Vision via CDN inside a single self-contained HTML file (`index.html`), no bundler or `npm install` step is required.

> **Important Note on Webcam & WASM Permissions:** Modern browsers require a secure context (`https://` or `http://localhost`) to grant webcam access (`getUserMedia`) and load MediaPipe WebAssembly modules. Serve the file over a local HTTP server rather than opening it via `file://`.

```bash
# Using Python 3
python3 -m http.server 8080

# Or using Node.js (npx)
npx serve .
```

Then open `http://localhost:8080` in Chrome, Edge, Safari, or Firefox.

## 🎛 Controls & Interaction

### Hand Gestures (Camera Mode)
| Gesture | Action |
| :--- | :--- |
| **Open Hand** | Glides the 3D glass prism smoothly across the mirror plane with gentle aerodynamic rotation. |
| **Pinch (Thumb + Index)** | Engages tight spring tracking, boosts rim light dispersion, and transfers hand movement and wrist rotation directly into 3D prism spin. |

### Mouse & Touch (Works Anytime)
* **Click & Drag / Touch Drag:** Raycasts onto the hero glass prism to drag it across the viewport and impart rotational torque.
* **Scroll Wheel:** Smoothly scales the hero glass prism between `0.55×` and `1.75×`.

### Floating Glassmorphic Dock
* **Enable Camera & Hand Tracking:** Toggles the live webcam mirror and MediaPipe `HandLandmarker` pipeline.
* **Hand Tracking: ON/OFF (Top Right):** Pauses computer-vision tracking while keeping the live webcam mirror active.
* **Center Prism:** Resets the prism to the center of the mirror at default scale.
* **Geometric Prism:** Morphs the central crystal between **Icosahedron**, **Octahedron**, **Torus Knot**, and **Dodecahedron**.
* **Weather Backdrop:** Transitions lighting, fog density, glass tint, and 3D particle physics between **Clear Prism**, **Misty Rain**, and **Storm Glass**.
* **Refraction (IOR) (`1.10` – `2.33`):** Adjusts the optical Index of Refraction from subtle water-like bending (`1.33`) to diamond-level dispersion (`2.33`).
* **Glass Frost (`0.00` – `0.55`):** Controls surface micro-roughness from crystal-clear polished glass to frosted sea glass.

## 🧠 Technical Architecture

1. **Three.js Refraction Pipeline:**
   * An opaque backdrop plane at $z = -4.5$ renders either the horizontally mirrored `THREE.VideoTexture` or an offscreen 2D canvas running animated radial light orbs and architectural grid lines.
   * Three.js renders opaque background objects into an internal transmission render target before shading the transmissive `MeshPhysicalMaterial` foreground meshes, producing authentic real-time background distortion.
2. **MediaPipe Vision Coordinate Mapping:**
   * Normalized video landmarks $(x, y) \in [0, 1]$ are horizontally mirrored ($1 - x$), aspect-ratio corrected to match the `object-fit: cover` mirror plane, and projected onto the $z = 0$ interaction plane in 3D world space.
   * Pinch detection normalizes thumb-to-index tip distance against palm size (`wrist` to `middle_finger_mcp`) with hysteresis thresholds (`0.32` engage / `0.42` release) so distance from the camera does not trigger false grabs.
3. **2D Overlay HUD (`#hud-canvas`):**
   * A synchronized full-viewport 2D canvas renders the 21-point hand skeleton and rotating geometric pinch reticle cleanly above the WebGL layer without interfering with pointer raycasting.

## 🎨 Customization Ideas

* **Two-Hand Scaling & Dual Prisms:** Increase `numHands: 2` in `HandLandmarker.createFromOptions` so pinching with both hands stretches and scales the glass geometry between your palms.
* **Live Local Weather Sync:** Connect the free, no-auth [Open-Meteo API](https://open-meteo.com/) to automatically switch between *Clear Prism*, *Misty Rain*, and *Storm Glass* based on your city's current precipitation.
* **Snapshot Capture:** Add a button calling `renderer.domElement.toDataURL('image/png')` (with `preserveDrawingBuffer: true` on `WebGLRenderer`) to save crystal-refracted self-portraits.

## 📄 License

Free to use, remix, and build upon for personal and creative projects.
