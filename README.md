# Uke Tuner

**Version 1.0.0**

A single-file browser-based ukulele tuner. No dependencies, no build step — open in any modern browser or host on GitHub Pages.

-----

## Features

- **Live pitch detection** via microphone using autocorrelation with parabolic interpolation
- **Standard concert tuning** — G4 · C4 · E4 · A4
- **Auto string identification** — nearest-note detection across all four strings simultaneously; no need to select which string you’re playing
- **Reference tones** — tap any string button to hear a ~2 second tone, then auto-resumes listening
- **Vertical bar meter** — horizontal track with needle sliding left/right of center, colored amber (off-pitch) to green (±5 cents in tune)
- **Per-string status** — active string highlights green; glows on in-tune
- **Placeholder tabs** — Tuner / Chords / Library tabs stubbed for future features
- **No external libraries** — pure Web Audio API, Canvas 2D, vanilla JS

-----

## Usage

1. Open `ukulele-tuner.html` in a browser (Chrome, Safari, Firefox, Edge)
1. Click **Enable Microphone** and grant mic access
1. Play a string — the tuner auto-identifies which string and shows cents deviation
1. Tap a string button to hear its reference tone, then tune by ear

> **Hosting:** GitHub Pages serves over HTTPS by default, satisfying the `getUserMedia` security requirement. Do not open via `file:///` — use a local dev server (e.g. `npx serve`) instead.

-----

## Technical Notes

|Detail           |Value                                        |
|-----------------|---------------------------------------------|
|Pitch algorithm  |Autocorrelation + parabolic interpolation    |
|FFT buffer size  |4096 samples                                 |
|Frequency range  |220 – 520 Hz (ukulele fundamental range)     |
|Smoothing factor |0.14 (exponential moving average on cents)   |
|In-tune threshold|±5 cents                                     |
|Low-pass filter  |600 Hz (reduces room noise before analysis)  |
|Reference tone   |Triangle wave + detuned sine layer (~1.0015×)|

-----

## Roadmap

### v1.1.0

- [ ] Light / dark mode toggle with `localStorage` preference persistence
- [ ] Version number displayed in UI footer
- [ ] Disclaimer footer (vibecoded — use at your own risk)

### v1.2.0

- [ ] Chord diagram chart (Chords tab)
- [ ] Song / chord library (Library tab)
- [ ] Alternate tunings (D · G · B · E, etc.)

-----

## Disclaimer

> This tool was vibecoded. Use at your own risk.

-----

## Changelog

### v1.0.0 — Initial release

- Live pitch detection with autocorrelation
- Auto string identification (nearest-note, G4/C4/E4/A4)
- Reference tones via OscillatorNode (tap to play, auto-listen after)
- Vertical bar cents meter (amber → green)
- Placeholder tab navigation
- Single HTML file, no dependencies
