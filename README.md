# Montezer Digital Voice Pedal

A browser prototype of a guitar and bass pedal that turns every note you play into a human voice. Built with the Web Audio API in plain JavaScript, with no libraries and no build step.

Dedicated to my mentor, Ti Plume.

**Live demo:** https://blackpantherjazz.github.io/montezer/

## How it works

The Montezer uses the **source-filter model** of speech: a voice is a buzz (the vocal cords) shaped by resonant filters (the throat and mouth).

1. **Input and loudness:** the instrument signal is split into dry, talk-box, and analysis paths. RMS loudness drives a noise gate.
2. **Pitch tracking (YIN):** each frame is compared against delayed copies of itself to find its period, with sub-sample interpolation and a median filter against octave glitches.
3. **Vocal cords:** a custom `PeriodicWave` with harmonics falling off as 1/n^1.1 plays at the tracked pitch, with delayed vibrato, breath noise, and up to five detuned choir voices.
4. **Vocal tract:** four parallel bandpass filters at formants F1 to F4. Dragging around the F1/F2 vowel chart retunes them, and picking dynamics can open the vowel like a talking wah.
5. **Output:** a dry/wet blend, a limiter, and a live spectrum display.

**Modes:** *Singer* resynthesizes one voice from the tracked pitch. *Talk box* distorts the instrument itself and runs it through the formant filters, so chords work.

## Run it

- **Online:** open the live demo, click **Plug in live**, and allow microphone access.
- **Locally:** run `npx serve` in this folder and open the localhost link. Opening `index.html` directly works in Chrome, but Safari blocks microphone access for local files.

Use an audio interface with an instrument (Hi-Z) input, such as a Focusrite Scarlett with **INST** on, and wear headphones to avoid feedback.

## Roadmap

- [ ] Move pitch detection into an `AudioWorklet` for lower latency
- [ ] Consonant attacks ("d", "b", "m") with fast filter sweeps
- [ ] Expression pedal support through Web MIDI
- [ ] Port to hardware on an Electrosmith Daisy Seed

## Tech

HTML, CSS, and JavaScript · Web Audio API (`AnalyserNode`, `BiquadFilterNode`, `PeriodicWave`, `WaveShaperNode`, `DynamicsCompressorNode`) · YIN pitch detection · Karplus-Strong string synthesis for the demo riffs
