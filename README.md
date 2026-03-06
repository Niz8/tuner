# Uke Tuner

**Version 1.3.0**

A single-file browser-based ukulele tuner. No dependencies, no build step — open in any modern browser or host on GitHub Pages.

-----

## Features

- **Live pitch detection** via microphone using autocorrelation with parabolic interpolation
- **Standard concert tuning** — G4 · C4 · E4 · A4
- **Auto string identification** — nearest-note detection across all four strings simultaneously; no need to select which string you’re playing
- **Reference tones** — tap any string button to hear a ~2 second tone, then auto-resumes listening
- **Vertical bar meter** — horizontal track with needle sliding left/right of center, colored amber (off-pitch) to green (±5 cents in tune)
- **Per-string status** — active string highlights; glows green when in tune, blue during reference tone playback
- **Placeholder tabs** — Tuner / Chords / Library tabs stubbed for future features
- **No external libraries or fonts** — pure Web Audio API, Canvas 2D, vanilla JS, system fonts only

-----

## Usage

1. Open `ukulele-tuner.html` in a browser (Chrome, Safari, Firefox, Edge)
1. Tap **Enable Microphone** and grant mic access
1. Play a string — the tuner auto-identifies which string and shows cents deviation
1. Tap a string button to hear its reference tone, then tune by ear

> **Hosting:** GitHub Pages serves over HTTPS by default, satisfying the `getUserMedia` security requirement. Do not open via `file:///` — use a local dev server (e.g. `npx serve`) instead.

-----

## Technical Notes

|Detail           |Value                                                           |
|-----------------|----------------------------------------------------------------|
|Pitch algorithm  |Autocorrelation + parabolic interpolation                       |
|FFT buffer size  |4096 samples                                                    |
|Frequency range  |220 – 520 Hz (ukulele fundamental range)                        |
|Smoothing factor |0.14 (exponential moving average on cents)                      |
|In-tune threshold|±5 cents                                                        |
|Low-pass filter  |Removed — was blocking signal needed for autocorrelation on iOS |
|Reference tone   |Triangle wave + detuned sine layer (~1.0015×)                   |
|Fonts            |System fonts only (Georgia, Courier New) — no Google Fonts      |
|Canvas rendering |No CSS variables, no `roundRect` — full iOS Safari compatibility|
|AudioContext     |`webkitAudioContext` fallback for older iOS WebKit              |

-----

## Roadmap

### v1.2.0

- [ ] Light / dark mode toggle with `localStorage` preference persistence
- [ ] Chord diagram chart (Chords tab)

### v1.3.0

- [ ] Song / chord library (Library tab)
- [ ] Alternate tunings (D · G · B · E, etc.)

-----

## Disclaimer

> This tool was vibecoded. Use at your own risk.

-----

## Changelog

### v1.3.0 — Pitch detection iOS fix & comma glyphs

- Switched from `getFloatTimeDomainData` (Float32Array) to `getByteTimeDomainData` (Uint8Array) — iOS WebKit can return a flat/silent float buffer even when audio is flowing; the byte API has solid iOS support
- Converted Uint8 values (0–255, center 128) to float (-1 to +1) before correlation
- Removed `&nbsp;` from string-label and cents-display elements — was rendering as a visible comma glyph in Georgia on iOS

### v1.2.0 — iOS pitch detection fix

- Removed lowpass filter — it was stripping the waveform detail autocorrelation needs to function
- Lowered RMS silence threshold from 0.01 to 0.003 to catch quieter iOS mic levels
- Widened frequency detection window to 200–600 Hz
- Added `getUserMedia` fallback: tries full constraints first, retries with `audio: true` if iOS rejects the constraint object
- Fixed em-dash (`—`) rendering as commas in Georgia on iOS; replaced with plain hyphen
- Moved `AudioContext` creation into its own `initAudio()` helper for cleaner retry logic

### v1.1.0 — iOS Safari compatibility & display fixes

- Removed all Google Fonts; replaced with system fonts (Georgia, Courier New)
- Removed all CSS variable usage from canvas drawing; replaced with hardcoded hex/rgba values
- Replaced `roundRect` canvas call with plain `fillRect` for iOS Safari compatibility
- Added `webkitAudioContext` fallback for older iOS WebKit
- Moved `AudioContext` creation inside user gesture handler to satisfy iOS autoplay policy
- Added `v1.1.0` version badge in header
- Added “vibecoded · use at your own risk” disclaimer above tab bar
- Rewrote JS loops to avoid arrow functions and `const`/`let` for maximum compatibility

### v1.0.0 — Initial release

- Live pitch detection with autocorrelation
- Auto string identification (nearest-note, G4/C4/E4/A4)
- Reference tones via OscillatorNode (tap to play, auto-listen after)
- Vertical bar cents meter (amber → green)
- Placeholder tab navigation
- Single HTML file, no dependencies