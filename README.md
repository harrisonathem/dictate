# Dictate

A prototype of in-browser clinical dictation. You speak, it transcribes, and it tidies the transcript into a note. Everything runs on your own device using WebGPU. The page uploads nothing and stores nothing; the only network traffic is downloading the code and the open model weights.

- **Standard** (`index.html`): Moonshine base + Qwen3.5 0.8B, about 0.6 GB to download the first time.
- **Pro** (`index.html?pro`, or `pro.html`): Whisper large-v3-turbo + a larger note-writing model, about 2 GB.
- **Bench** (`bench.html`): try different transcribers and note writers, including Chrome's built-in Gemini Nano where it's available.

After the first visit, the models load from the browser cache in seconds.

## Requirements

Chrome or Edge on a computer with a GPU. On Linux, turn on `chrome://flags/#enable-unsafe-webgpu` and `chrome://flags/#enable-vulkan`, then relaunch.

## How it works

1. The microphone audio is split at pauses into chunks of 6–25 seconds. Each chunk is transcribed while you keep talking.
2. Code, not the model, handles spoken punctuation ("full stop", "new paragraph"), "scratch that", filler words, and misheard drug names. Drug names are fuzzy-matched against a list of common UK drugs, so "by soprolol" becomes bisoprolol.
3. A small language model tidies the text into prose, SOAP or a GP letter.
4. Code then checks the note against the transcript and flags any changed numbers, sides (left/right), negations, or drug names it wasn't given.

This is a prototype, not a medical device. Read every note before using it.

## Run locally

```
python3 -m http.server 8000
```

Then open http://localhost:8000. Opening the file directly won't cache the models.
