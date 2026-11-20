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
- Success is measured by recovery time and self-catches (the user tapping I caught it before Steady nudges), not by a calm score. Support fades as self-catches rise.
- Research behind these choices: `reports/Emotion signals and workplace judgment.md`.
