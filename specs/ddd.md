# ASR Pro domain glossary

This glossary is the shared vocabulary for the code, the tests, and the
specs. Use these terms in names and in UI copy. When a feature settles a new
term or changes one, update this file in the same change (D-26).

## Naming tree

```
ASR Pro
└── data folder                     all app data on disk (D-20)
    ├── settings                    config/settings.json (schema v2)
    ├── model                       one speech or speaker model
    │   ├── Whisper model           models/whisper/*.bin (pinned hash)
    │   └── diarization model       models/diarization/*.onnx
    ├── history row                 one entry in history/history.db
    │   ├── audio file              history/audio/<id>.webm|wav
    │   ├── transcript              the row text (edited text wins)
    │   │   └── segment             one timed part of a transcript
    │   │       └── speaker         who speaks in a segment
    │   └── transcript file         transcripts/<id>.txt copy
    └── session WAV                 cache/sessions/<sessionId>.wav
        └── recording session       one open capture stream
```

## Terms

- **recording**: the act of capturing microphone audio in the app. A
  recording starts from Home, the tray, the menu, or the global shortcut,
  and it ends as a history row or as a cancelled row ("Not transcribed").
- **session**: one open audio stream between the renderer and the engine
  process, keyed by a session id. A session writes its 16 kHz PCM to a
  session WAV in `cache/sessions/`. Session purposes: dictation, import,
  reprocess.
- **transcript**: the text of a history row. The edited transcript is what
  search and exports use; the original engine text stays stored for
  "Revert to original" and Reprocess.
- **segment**: one timed part of a transcript, with start and end in
  milliseconds. A row without segments displays as one segment that covers
  the full duration.
- **speaker**: a voice that the diarization process found in the audio.
  Speakers carry a stable key and a display name the user can edit. A
  single detected speaker hides the labels in the UI.
- **history row**: one entry in the SQLite history database: text, audio
  file, model, language, timing, status, segments, and speakers. There is
  no row limit; audio retention removes only audio files.
- **bundle**: one `.asrpro-bundle` zip file with selected history rows and
  optionally their audio, for moving history between computers.
- **model**: a speech model (Whisper, roles final or live) or a speaker
  model (segmentation or embedding). Every model has a pinned hash and
  downloads only when the user asks.
- **data folder**: the root of all app data. Its location depends on the
  build type (D-20); the UI shows it as `~/...` when it is under the user
  home.

## Process names

- **main process**: owns the data folder, settings, the history database,
  the windows, the tray, the menu, and the IPC router.
- **renderer**: the sandboxed main window. It captures the microphone and
  decodes media files. It never touches Node APIs or raw file paths.
- **engine process**: the utilityProcess that runs Whisper.
- **speaker process**: the utilityProcess that runs diarization.
- **overlay**: the small always-on-top waveform window.
