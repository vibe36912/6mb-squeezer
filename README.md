# Audio-Priority 6MB Squeezer

A purely client-side web utility that compresses MP4/WEBM files to strictly under 6 MB (targeting 5.0 MB). It maximizes audio fidelity by aggressively degrading the video track, making it optimal for audio-centric or static-image videos.

## Technical Details

- **Video Pipeline:** Uses the WebCodecs API (`VP9`, profile `vp09.00.31.08`). Downscales max dimension to ~320px (mod 2 aligned), restricts framerate to 1 FPS, and hard-caps video bitrate at 15 kbps.
- **Audio Pipeline:** Extracts PCM data via `AudioContext` (48kHz resample). Encodes via WebCodecs API (`Opus`). Calculates available bit budget (targeting 5 MB total) and dynamically allocates remaining bandwidth to audio (32 kbps to 384 kbps).
- **Muxing:** Interleaves frames and packages into a `.webm` container using `webm-muxer`.
- **Execution:** 100% local browser processing. Includes asynchronous event-loop yielding to maintain main-thread responsiveness during the chunk encoding loop.

## Dependencies

- [webm-muxer](https://github.com/Vanilagy/webm-muxer) (Loaded via unpkg CDN)
- Browser with WebCodecs API support.
