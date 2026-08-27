# Arena Gate — Gesture-Activated Portal Reveal

A single-file, browser-based interactive experience that opens a creaking
stone gate using nothing but your webcam and your hands. Trace a circle in
the air, and the gate creaks open to reveal a full-screen video, playing at
full quality with sound.

No installs, no backend, no uploads — everything runs locally in the
browser using [MediaPipe Hands](https://developers.google.com/mediapipe) for
real-time hand tracking and the Web Audio API for the door sound effects.

## Demo

Open `index.html` in a browser (or visit the GitHub Pages link once
enabled), grant camera access, and follow the on-screen instructions:

1. **Open your palm** to the camera — small runes orbit your hand,
   confirming it's being tracked.
2. **Make a fist with your left hand** to anchor the gate in place.
3. **Trace a circle in the air with your right index finger** while the
   fist is held — completing the circle triggers the gate.
4. The gate creaks open with an old wooden-door sound (~9 seconds to fully
   swing open). **Five seconds after the doors are fully open**, the reveal
   video plays full-screen with sound.
5. **Release the fist** (or click "Close Gate") to close the gate and reset.

## Features

- 🖐️ **Real-time hand tracking** via MediaPipe Hands, entirely client-side
- 🚪 **Procedural door-creak audio** synthesized with the Web Audio API — no
  external audio files
- 🎬 **Full-screen HD video reveal**, embedded directly in the page (single
  portable file, no external assets to keep track of)
- 🔒 **Privacy-respecting** — the webcam feed never leaves the browser; there
  is no server component at all
- 🎛️ **Built-in dev controls** — mute/unmute, a "Test Door Sound" button, and
  a "Preview Reveal" button to trigger the full sequence without the hand
  gesture (useful while tweaking timing/visuals)

## Tech stack

- Vanilla JavaScript, HTML5 Canvas, and CSS animations
- [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)
  (loaded via CDN) for hand landmark detection
- Web Audio API for procedural sound design (no audio files, no libraries)
- Video is transcoded to H.264/AAC and embedded as a base64 data URI, so the
  whole experience ships as one `.html` file

## Getting started

Clone the repo and open the file directly — no build step, no server
required:

```bash
git clone https://github.com/<your-username>/arena-gate-portal-reveal.git
cd arena-gate-portal-reveal
open index.html   # or just double-click it / drag it into a browser tab
```

> Camera access requires a secure context. Opening the file directly
> (`file://`) works in most browsers for local testing; if your browser
> blocks camera access on `file://` URLs, serve it locally instead:
> ```bash
> python3 -m http.server 8000
> ```
> then visit `http://localhost:8000`.

### Hosting on GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Set the source to the `main` branch, root folder.
4. Your gate will be live at `https://<your-username>.github.io/<repo-name>/`.

## Customizing the reveal video

The video is embedded as a base64 data URI so the page is fully portable.
To swap in your own clip:

1. Encode your video as H.264/AAC MP4 (for broad browser compatibility):
   ```bash
   ffmpeg -i your-clip.mp4 -c:v libx264 -crf 18 -preset medium \
     -c:a aac -b:a 192k -movflags +faststart output.mp4
   ```
2. Base64-encode it and replace the `portalVideo.src` data URI in
   `index.html`:
   ```bash
   base64 -w0 output.mp4 > output.b64
   ```
3. Adjust `VIDEO_START_DELAY_MS` in the script if you want the video to
   start sooner or later after the gate begins opening.

## Browser support

Tested on recent versions of Chrome, Edge, and Firefox (desktop). Requires
a browser with `getUserMedia` (webcam) and Web Audio API support. Safari on
iOS/macOS is expected to work but audio autoplay policies may require an
extra tap in some versions.

## License

MIT — do whatever you like with it, attribution appreciated but not required.

## Disclaimer

The included reveal video is a user-supplied fan edit and is provided as-is
for personal/demo purposes. Swap it out with your own licensed content
before distributing this project publicly.
