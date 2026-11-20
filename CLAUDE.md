# Steady: project memory

## Goal

Help people who have trouble with emotional regulation in the workplace learn to adjust their behaviors, so they can interrupt them and hopefully stop them in the future on their own.

Every feature should serve that goal: noticing a reaction early, interrupting it in the moment, and building the person's own ability to self-correct, so they rely on the app less over time.

## Product facts

- Single-page web app (`Steady.html`) hosted on GitHub Pages from `main`: https://almostnicklee.github.io/steady-app/
- Everything runs on the user's device. No video, audio or transcripts are recorded, saved or sent. A Content Security Policy limits connections to jsDelivr and Google model storage.
- It is a self-coaching aid for the user's own use, not a tool for monitoring or evaluating other people, and not a medical or diagnostic tool.
- Wording describes visible behavior and, at most, how it may come across in stance terms ("Brow lowered", "Can read as disagreement"), never the person's inner emotion. The user names their own feelings in the review. See README and the research report for why (accuracy, and EU AI Act emotion recognition rules).
- Nudges cue an action that lowers arousal or carries out the user's plan ("Brow tight. Long exhale."). They never ask the user to fake or fix their face, because suppression is linked to worse memory, strained rapport and burnout.
- Success is measured by recovery time and fewer moments over time, not by a calm score. (An "I caught it" button was tried and removed at the owner's request as too distracting.)
- Signals are read together: face, body (hand to forehead, eyes or temple; head shaking; eyes shut), voice (loudness, pitch, pace, sighs) and words. Signals from different channels within 3 s boost each other and give one combined nudge.
- The review stays short: a self-rating, a donut of the session (good, warning, nudge time) around the face of the session's most common signal, and one question, "How were the nudges?" (Too many, About right, Too few, then which channels), which tunes thresholds. Per-moment right or wrong marks and plans were removed at the owner's request (hard to judge from memory).
- Every page has a Font Awesome account icon at the top right (sign out, delete this device's data).
- Owner decisions: the meeting view defaults to Full color (navy to red screen plus the face), and every triggered cue nudges (no global gap or per-meeting cap). Private view and Auto fading exist only as options the user can choose.
- Research behind these choices: `reports/Emotion signals and workplace judgment.md`.
