# steady-app

Steady is a private, on-device coach for how you come across in meetings. It watches your face and listens to your voice, and the whole screen warms from navy to red the moment a reaction might land badly.

## Live site

https://almostnicklee.github.io/steady-app/

The site is hosted free on GitHub Pages and updates automatically a minute or two after any change lands on the `main` branch.

## What it does

Steady's goal is to help people who have trouble with emotional regulation at work notice a reaction early, interrupt it, and build their own ability to self-correct, so they need the app less over time.

- **Personal setup (one time, about two minutes):** guided steps on camera and microphone (resting face straight on, turned left, turned right, tipped down, listening, normal read, lively story, rushed read, smile, frown, pressed lips). You tap Start and Done for each step.
- **During the meeting:** the screen stays navy while you look and sound steady and warms toward red when something slips, and a face mirrors the cue. You also get a vibration and a short action tip, such as "Brow tight. Long exhale." or "Louder than your usual. Exhale before the next sentence." Your saved plan is added to the tip. A label describes the behavior ("Brow lowered: can read as disagreement"), never your emotion.
- **I caught it:** tap it (or press C, or tap the dot in discreet mode) when you notice first. Self-catches are the main measure of progress.
- **That nudge was wrong:** marks a false alarm. Steady becomes a little less sensitive to that cue, or ignores that phrase.
- **Nudges:** **All** (default) nudges every time a cue triggers; each cue has a short cooldown so a held expression does not repeat every second. **Auto** starts the same and switches to big moments only once you catch at least half yourself over three sessions, with a full check-up every fourth session. **Big** nudges only big moments, and **Off** gives a review only.
- **After the meeting:** rate the meeting before seeing any data, then see what stood out and how fast you came back. An optional two-minute review asks what set it off, what you noticed first in your body, and a word for what you felt, and ends with an if-then plan ("If someone questions my numbers, I will take one long exhale"). The plan is shown before your next meeting and added to nudges. A short check-in links to support if you are not okay.
- **Sharp words (on by default, needs the camera):** catches blaming, hostile, dismissive and defensive phrases and suggests a better phrasing. Self-critical phrases ("I'm so stupid") get a kind note instead. Repair phrases ("help me understand") are credited in the summary.
- **Options:** Meeting view Private (the screen stays dark, for when others can see it), Gentle mode, separate Face cues and Voice cues switches, sensitivity, vibration, chime.
- **Privacy:** nothing is recorded or uploaded. Settings, session counts, the optional profile and your plan are stored only in this browser; the profile and plan are encrypted with your sign-in. Review answers are not saved.

## How detection works

| Part | What is used | Notes |
| --- | --- | --- |
| Face | MediaPipe Face Landmarker (blendshapes), `@mediapipe/tasks-vision` 1.2.3 | Free (Apache 2.0). Runs in the browser on the GPU, with a CPU fallback. |
| Speech detection | Silero VAD v6 through `onnxruntime-web` 1.31.1 | Free (MIT). Tells speech from keyboard noise, hum, and clicks. Falls back to a loudness check if it cannot load. |
| Whose voice | Mouth movement from the face model | Voice cues only count while your own mouth is moving. |
| Sharp words | Moonshine tiny speech-to-text (MIT, `@usefulsensors/moonshine-js` 0.1.21 model files, about 28 MB) on `onnxruntime-web`, in a background worker | Runs on the device. Text is matched against a phrase list in `Steady.html` (`SHARP`) and discarded. Phrases are listed in that session's summary and never saved. |
| Pitch | YIN pitch tracker (in the page) | Judged over the last 5 seconds and must stay high for 2 seconds, so normal emphasis is ignored. Default trigger is 4.5 semitones; personal setup sets it above your own lively range. |
| Raised voice | Loudness plus voice sharpness (energy above 1 kHz) | Getting closer to the microphone raises loudness only, so it is not mistaken for a raised voice. |
| Pace | Syllable peaks in the low-frequency loudness, over 12 seconds | Approximate, and it understates real speed-ups, so the default trigger is 18% over your normal. Personal setup calibrates it from your own rushed read (never below 12%). |

The libraries and models load from public CDNs (jsDelivr and Google). They are fetched like any web file; no camera or microphone data is sent anywhere. A Content Security Policy in `Steady.html` only lets the page connect to those two sites. This also blocks the face library's built-in usage reports to Google (MediaPipe 1.2.x sends them every 60 seconds and has no switch to turn them off).

## Signing in

The page asks for an email and password before it opens. This is a **basic lock, not real security**:

- The check runs in the visitor's browser, because GitHub Pages cannot run server code. It keeps casual visitors out. Someone technical can bypass the form, and this repository's code is public either way.
- The email and password are **not** stored in this repository. `Steady.html` holds only a salted, deliberately slow one-way check value (`AUTH`).
- Wrong tries are slowed down in the form: a 2 second pause each time, then a lockout from the 5th wrong try that doubles from 30 seconds up to 15 minutes. This limit lives in the visitor's own browser, so it does not stop someone who skips the form. The real protection against guessing is a long, unique password.
- Use a password that you do not use anywhere else.

For real access control, put the site behind a service that checks sign-in on a server (for example Cloudflare Access, which has a free tier) and make the repository private.

To change the email or password, replace the three values in `AUTH` near the end of `Steady.html` with a new set. Open any https page, paste this into the browser console, and enter the new email and password when asked:

```js
(async()=>{const e=new TextEncoder(),h=b=>Array.from(new Uint8Array(b),v=>v.toString(16).padStart(2,'0')).join(''),
j=(a,b)=>{const o=new Uint8Array(a.length+b.length);o.set(a);o.set(b,a.length);return o},
salt=crypto.getRandomValues(new Uint8Array(16)),iter=3000000,
k=await crypto.subtle.importKey('raw',e.encode(prompt('New password')),'PBKDF2',false,['deriveBits']),
m=new Uint8Array(await crypto.subtle.deriveBits({name:'PBKDF2',hash:'SHA-256',salt:j(salt,e.encode(prompt('Email').trim().toLowerCase())),iterations:iter},k,256));
console.log(`const AUTH={salt:'${h(salt)}',iter:${iter},check:'${h(await crypto.subtle.digest('SHA-256',j(e.encode('steady-check'),m)))}'};`)})()
```

Changing the password makes any saved profile unreadable, so it starts fresh.

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
