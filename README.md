# steady-app

Steady is a private, on-device coach for how you come across in meetings. It watches your face and listens to your voice, and the whole screen warms from navy to red the moment a reaction might land badly.

## Live site

https://almostnicklee.github.io/steady-app/

The site is hosted free on GitHub Pages and updates automatically a minute or two after any change lands on the `main` branch.

## What it does

- **Setup (15 seconds):** a large camera view with a head-shaped outline. The outline turns green when your face is well framed, and the countdown starts on its own.
- **During the meeting:** a friendly face mirrors the strongest cue, the background shifts from navy through plum to red, and a short nudge appears (with a vibration on phones that support it).
- **Cues:** scowl, tight lips, smirk, eye roll, raised voice, rising pitch, rushing, and holding the floor too long. Every cue is measured against your own normal from setup.
- **Privacy:** nothing is recorded, saved, or uploaded. Only session scores are stored, in the browser on your device.

## How detection works

| Part | What is used | Notes |
| --- | --- | --- |
| Face | MediaPipe Face Landmarker (blendshapes), `@mediapipe/tasks-vision` 1.2.3 | Free (Apache 2.0). Runs in the browser on the GPU, with a CPU fallback. |
| Speech detection | Silero VAD v6 through `onnxruntime-web` 1.31.1 | Free (MIT). Tells speech from keyboard noise, hum, and clicks. Falls back to a loudness check if it cannot load. |
| Whose voice | Mouth movement from the face model | Voice cues only count while your own mouth is moving. |
| Pitch | YIN pitch tracker (in the page) | No download needed. |
| Raised voice | Loudness plus voice sharpness (energy above 1 kHz) | Getting closer to the microphone raises loudness only, so it is not mistaken for a raised voice. |
| Pace | Syllable peaks in the low-frequency loudness | Approximate. This cue is deliberately slow to trigger. |

The libraries and models load from public CDNs (jsDelivr and Google). They are fetched like any web file; no camera or microphone data is sent anywhere.

## Files

- `Steady.html`: the whole app (one self-contained page).
- `index.html`: sends visitors from the site's root address to `Steady.html`.
- `.nojekyll`: tells GitHub Pages to serve the files as-is.

## One-time GitHub Pages setup

1. Open the repository on GitHub and go to **Settings > Pages**.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
3. Set **Branch** to `main` and the folder to `/ (root)`, then click **Save**.
4. Wait about a minute, then open the live site link above.

## Intended use

Steady is a self-coaching aid for your own use. It reads visible expressions and voice level, not feelings, and its cues are estimates that will sometimes be wrong. It is not a medical or diagnostic tool, and it is not for monitoring or evaluating other people.
