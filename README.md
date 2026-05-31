# Clarity — Image Upscaler

A lightweight, **browser-only** image upscaler. It enlarges images and reconstructs detail entirely in the browser — no uploads, no backend, no external app. It ships two engines: an AI super-resolution network (default) and a fast classical pipeline.

**Live demo:** [immortaleyes.github.io/image-upscaler](https://immortaleyes.github.io/image-upscaler/)

> **Important:** the AI engine loads its model from a CDN, so it only runs on the **deployed HTTPS site** (the live demo above). In a preview sandbox or when opening the file locally it falls back to the classical engine and shows a clear warning.

---

## Two engines

| Engine | What it does | Speed | Needs internet |
|--------|--------------|-------|----------------|
| **AI** *(default)* | ESRGAN neural network (TensorFlow.js) that reconstructs detail | Slower | Yes (loads model once) |
| **Sharp** | Lanczos-3 resampling + Contrast-Adaptive Sharpening (CAS) | Instant | No |

- **AI** — runs an **ESRGAN** model via TensorFlow.js / UpscalerJS that *reconstructs* plausible detail instead of merely interpolating. Three quality tiers: **Fast** (esrgan-slim), **Balanced** (esrgan-medium), **Max** (esrgan-thick, the default). The model auto-downloads from a CDN on first use and is cached afterward.
- **Sharp** — proper **Lanczos-3** interpolation (the windowed-sinc filter professional tools use for "preserve detail" enlargement) followed by **CAS** (AMD FidelityFX) edge-aware sharpening. Crisp, instant, fully offline, works on any image size.

---

## Features

- **AI super-resolution by default** — strongest model (Max) selected out of the box.
- **No silent fallback** — if the AI model can't load, the app says so with a prominent warning instead of quietly returning a soft resize, so you always know which engine produced the result.
- **Two resize modes** — scale by a factor (2× / 3× / 4× / custom) or set exact target pixel dimensions, with aspect-ratio lock.
- **Adjustable sharpening (clarity)** — contrast-adaptive, enhances detail without halos or blur.
- **Format options** — export as PNG, JPG, or WebP, with a quality slider for the lossy formats.
- **Live preview, size estimate, and progress bar.**
- **Download and Reset buttons** — Reset clears the current output (and loading a new image clears any previous result automatically).
- **Fully private** — images are processed locally and never leave the user's device.

---

## How it works

**AI engine**
1. Loads TensorFlow.js and an **ESRGAN** model (UpscalerJS), then processes the image in tiles with a progress indicator.
2. The network reconstructs a 4× output; it is then fitted to the exact requested size with Lanczos and given a gentle CAS finish.

**Sharp engine**
1. **Lanczos-3 resampling** — a separable, premultiplied-alpha, two-pass windowed-sinc resize that produces crisp edges rather than the soft output of a basic canvas resize.
2. **Contrast-Adaptive Sharpening (CAS)** — sharpens true detail adaptively while avoiding halos and noise amplification.

---

## Honest limitations

This is a **free, fully in-browser** tool, which means it can only run small AI models. The bundled ESRGAN models (including Max) are a few hundred thousand parameters.

- It will reconstruct **some** real detail and look clearly better than a plain resize, but it **cannot match dedicated cloud upscalers** (e.g. iLoveIMG), which run much larger **server-side Real-ESRGAN** models on GPUs, often with a dedicated **face-restoration** pass for portraits. That is an architectural ceiling, not a tuning issue.
- Neither engine can recover detail that was never captured (extreme blur, very tiny faces).
- For server-grade quality you would need a backend (e.g. a Real-ESRGAN + GFPGAN service); that is outside the scope of this browser-only project.

---

## Usage

No build step, no installation:

1. Open the **live demo** (recommended for AI), or open `index.html` in a modern browser.
2. Drag-and-drop an image (or click to browse). JPG, PNG, and WebP are supported.
3. Choose an engine (AI is default), output size, sharpening, format, and quality.
4. Click **Upscale image**, then **Download**. Use **Reset** to clear and start over.

> If you see the amber warning banner, the AI model could not load in your current environment — open the deployed HTTPS site and try again.

---

## Deploy your own (GitHub Pages)

1. Create a new **public** repository on GitHub.
2. Upload the app file and make sure it is named **`index.html`**.
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, select branch `main` and folder `/ (root)`, then **Save**.
5. After about a minute, your live URL appears:
   `https://<your-username>.github.io/<repo-name>/`

GitHub Pages serves over HTTPS, which the AI model loader and the browser canvas/file APIs require.

---

## Tech

- **HTML, CSS, and vanilla JavaScript** — a single self-contained file, no build tooling.
- **Canvas API** with a hand-written **Lanczos-3** resampler and **Contrast-Adaptive Sharpening**.
- **TensorFlow.js 4.11** + **UpscalerJS** (ESRGAN), loaded as UMD bundles on demand from a CDN.
- Fonts: *Fraunces* and *DM Mono* (Google Fonts).

---

## License & Copyright

© Ajay Shriram Kushwaha. All rights reserved.

You may host and share this tool; please retain the copyright notice. For other uses, contact the author.
