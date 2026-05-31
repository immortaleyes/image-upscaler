# Clarity — Image Upscaler

A lightweight, browser-based tool that upscales low-resolution images into larger, sharper versions. It increases an image's pixel dimensions and applies a clarity-boosting sharpening pass — all **entirely in the browser**, with no uploads and no server.

**Live demo:** `[https://immortaleyes.github.io/image-upscaler/]`

---

## Features

- **Two resize modes** — scale by a factor (2× / 3× / 4× / custom) or set exact target pixel dimensions.
- **Aspect-ratio lock** — keep proportions correct when typing exact sizes.
- **Sharpening control** — an adjustable unsharp-mask pass that boosts perceived clarity.
- **Format options** — export as PNG, JPG, or WebP, with a quality slider for the lossy formats.
- **Live preview + size estimate** — see the result and its file size before downloading.
- **One-click download** — saves with a descriptive filename (e.g. `photo_3840x2160.png`).
- **Fully private** — images are processed locally; nothing is ever sent to a server.

---

## How it works

1. The image is enlarged using **stepped high-quality resampling** — the size is doubled in stages rather than in a single jump, which avoids blocky artifacts and produces smoother edges.
2. An **unsharp mask** (blur, then subtract from the original) restores perceived crispness that softens during enlargement.
3. The result is encoded to the chosen format and offered as a download.

> **Honest limitation:** Interpolation *enhances* detail but cannot *invent* information the original image never captured. A blurry, unreadable license plate will not become legible. For that level of reconstruction, AI super-resolution (e.g. Real-ESRGAN) is the appropriate next step.

---

## Usage

No build step, no dependencies. Just open the file:

1. Open `index.html` in any modern browser, **or** visit the live demo link above.
2. Drag-and-drop an image (or click to browse). JPG, PNG, and WebP are supported.
3. Choose your output size, sharpening, format, and quality.
4. Click **Upscale image**, then **Download**.

---

## Deploy your own (GitHub Pages)

1. Create a new **public** repository on GitHub.
2. Upload this file and make sure it is named **`index.html`**.
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, select branch `main` and folder `/ (root)`, then **Save**.
5. After about a minute, your live URL appears:
   `https://<your-username>.github.io/<repo-name>/`

GitHub Pages serves over HTTPS by default, which the browser's canvas and file APIs require.

---

## Tech

- Plain **HTML, CSS, and vanilla JavaScript** — a single self-contained file, zero dependencies.
- Uses the **Canvas API** for resampling and a hand-written separable box-blur for the unsharp mask.
- Fonts: *Fraunces* and *DM Mono* (loaded from Google Fonts).

---

## License & Copyright

© Ajay Shriram Kushwaha. All rights reserved.

You may host and share this tool; please retain the copyright notice. For other uses, contact the author.
