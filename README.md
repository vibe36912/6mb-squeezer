# 6MB Squeezer

A purely client-side video compressor that severely degrades video quality to preserve audio. It targets a strict `< 6MB` file size.

## How it Works
Instead of lowering the overall quality evenly, this tool:
1. Crushes the video track to **1 Frame Per Second** and a microscopic **15 kbps**.
2. Calculates the remaining bandwidth available within a 5MB target limit.
3. Allocates 100% of that remaining bandwidth to the audio track (usually resulting in transparent, CD-quality AAC/Opus).

## Features
- **Zero Server Processing:** Everything runs locally in your browser using the modern WebCodecs API and WebAssembly. Your files never leave your device.
- **Dynamic Bitrate:** Automatically calculates the max audio bitrate based on the video's duration.
- **Smart Scaling:** Caps maximum resolution at 1080p and ensures strict H.264 divisibility constraints to prevent encoder crashes.
- **Auto-Fallback:** Uses `H.264/AAC` (MP4) if supported by your browser, and gracefully falls back to `VP9/Opus` (WebM) if not.

## Limitations
- **Memory Constraint:** The app uses `AudioContext.decodeAudioData()`, which decodes the entire audio track into RAM at once. It works flawlessly for standard videos (under 10-15 minutes), but attempting to process a 3-hour podcast may crash your browser tab due to memory limits.
- **WebCodecs Requirement:** Requires a modern, up-to-date browser that supports the WebCodecs API.
