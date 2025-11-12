# AR Flash Card

A browser-based Augmented Reality (AR) learning experience. Show printed “flash card” images to your camera and see 3D models appear on top of them, with matching sound effects. Built with MindAR (image tracking) and Three.js (3D rendering).

- Recognizes multiple cards at once
- Renders interactive 3D models anchored to each card
- Plays per-object audio that changes with distance for a more immersive effect

## Demo

- Project link: see the “About” section of the repository (or deploy with GitHub Pages and add the link here).
- Requirements: camera permission and a modern browser. Prefer HTTPS (or localhost) so the camera works.

## Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Targets and Models](#targets-and-models)
- [Tech Stack](#tech-stack)
- [Performance Tips](#performance-tips)
- [Troubleshooting](#troubleshooting)
- [Extend the Project](#extend-the-project)
- [Credits and Licenses](#credits-and-licenses)
- [Contributors](#contributors)
- [License](#license)

## Overview

This app turns printed images into interactive AR flash cards. When you show a target image to your webcam, the app detects it and overlays a matching 3D model with sound. You can:

- Move the card closer/farther to perceive model size changes and audio volume differences.
- Present multiple cards simultaneously to view multiple models at once (with animations and sound).

Use cases:
- Early education for object recognition
- Vocabulary building
- Fun demos of web-based AR

## How It Works

- MindAR detects known images in the live camera feed using a precompiled file of targets (`abcd.mind`).
- For each recognized image, an “anchor” is created in 3D space at the card’s position and orientation.
- Three.js renders the corresponding 3D model at that anchor so it appears glued to the image.
- Audio plays for each detected model and can be modulated with distance for realism.

High-level flow:

1) Open `index.html` and grant camera permission.  
2) MindAR loads `abcd.mind` and starts tracking the camera frames.  
3) When a target image is recognized, the corresponding model is shown.  
4) The render loop updates model animations and audio volume as the card moves.  
5) Multiple targets can be recognized and displayed at the same time.

## Project Structure

```text
AR_Flash_Card/
├─ README.md
├─ abcd.mind                  # MindAR image target dataset (compiled from images/)
├─ index.html                 # Entry HTML page (includes libraries and main.js)
├─ main.js                    # App logic: MindAR + Three.js + models + audio
├─ airplane/
│  ├─ scene.bin
│  └─ scene.gltf
├─ ball/
│  ├─ scene.bin
│  ├─ scene.gltf
│  └─ textures/
│     └─ Material.002_baseColor.png
├─ car/
│  ├─ scene.bin
│  ├─ scene.gltf
│  └─ textures/
│     └─ material_0_baseColor.png
├─ dog/
│  ├─ scene.bin
│  ├─ scene.gltf
│  └─ textures/
│     ├─ Doggo_Base_baseColor.png
│     ├─ Doggo_Base_normal.png
│     └─ Doggo_Fuzzies_baseColor.png
├─ images/                    # Flash card images (targets)
│  ├─ a.jpg
│  ├─ b.jpg
│  ├─ c.jpg
│  └─ d.jpg
├─ libs/                      # Third-party libraries
│  ├─ GLTFLoader.js
│  ├─ loader.js
│  ├─ mindar-image-three.prod.js
│  └─ three.module.js
└─ sound/                     # Audio for each model
   ├─ airplane.mp3
   ├─ ball.mp3
   └─ car.mp3
```

## Getting Started

Prerequisites:
- A modern browser (Chrome, Edge, Firefox, Safari). Use HTTPS or localhost so camera access works.
- A simple static web server to serve the files (don’t open index.html directly from the filesystem).
- A webcam-enabled device.

Run locally (pick one):

- Node.js http-server:
  ```bash
  npx http-server -p 8080 .
  ```
  Then open http://localhost:8080 in your browser.

- Python (3.x):
  ```bash
  python -m http.server 8080
  ```
  Then open http://localhost:8080.

Grant camera permission when prompted.

## Usage

1) Download/print the images from the `images/` folder (a.jpg, b.jpg, c.jpg, d.jpg).  
2) Open the site (localhost or deployed link).  
3) Allow camera access.  
4) Hold a target image up to the camera.  
5) You should see the corresponding 3D model appear with sound.  
6) Move the image closer/farther to see size/volume changes.  
7) Try showing multiple images at once to view multiple models simultaneously.  
8) For the best audio effect, use earphones.

## Targets and Models

- Targets: `images/a.jpg`, `images/b.jpg`, `images/c.jpg`, `images/d.jpg`
- Models: folders `airplane/`, `ball/`, `car/`, `dog/`
- Audio: `sound/airplane.mp3`, `sound/ball.mp3`, `sound/car.mp3`

Note: Each target is mapped to a model in `main.js` using MindAR anchors (one anchor per target index). The `.mind` file (`abcd.mind`) is the compiled target database made from the images in `images/`.

## Tech Stack

- MindAR (Image Tracking + WebAR integration)
- Three.js (3D rendering in the browser)
- glTF 2.0 models (scene.gltf + scene.bin + textures)
- Web Audio (for sound effects and volume control)

## Performance Tips

- Ensure good, even lighting for reliable image detection.
- Keep the whole target image in frame; move steadily to avoid jitter.
- Heavy textures/models (e.g., high-res dog textures) may take longer to load on mobile.
- Prefer Wi‑Fi and a reasonably modern device for best performance.

## Troubleshooting

- Camera blocked or not working:  
  - Use HTTPS or `http://localhost`.  
  - Check browser permissions to allow camera access.

- Nothing appears when showing a card:  
  - Verify you’re using the correct images from `images/`.  
  - Improve lighting and avoid glare.  
  - Keep the image flat and fully visible.

- Audio not playing:  
  - Some browsers require a user interaction (tap/click) before enabling audio.  
  - Use earphones for better clarity.

- Performance is choppy:  
  - Close other heavy browser tabs.  
  - Try on a desktop or a newer phone.  
  - Show fewer targets at once.

## Extend the Project

Add a new flash card and model:

1) Place a new target image into `images/` (e.g., `e.jpg`).  
2) Add a new model folder (e.g., `elephant/`) with `scene.gltf`, `scene.bin`, and textures.  
3) Add an optional sound file to `sound/` (e.g., `elephant.mp3`).  
4) Rebuild the MindAR targets file to include your new image(s) and save as `abcd.mind`.  
   - Refer to the MindAR documentation on creating image targets and compiling `.mind` files.  
5) Update `main.js` to:
   - Add a new MindAR anchor for the new target index.  
   - Load the new model and bind the audio.  
6) Test locally and iterate.

## Credits and Licenses

- Libraries:
  - MindAR: Image tracking and AR integration for the web.
  - Three.js: 3D rendering.
  - GLTFLoader from Three.js for loading glTF 2.0 models.

- 3D models:
  - Each model folder (`airplane/`, `ball/`, `car/`, `dog/`) contains a `license.txt`.  
    Please review and honor the original asset licenses when reusing or distributing.

- Sounds:
  - Audio files in `sound/` are intended for this demo. Ensure you have rights to redistribute if you fork or publish publicly.

## Contributors

- Dipanshu Patel 
- Kartikey Saxena

(Original contributors listed in the project’s README. Repository owner/maintainer: see GitHub repo page.)

## License

This project’s license is provided in the [LICENSE](./LICENSE) file. Please review it before using or distributing the code and assets.

---
Made with MindAR + Three.js
