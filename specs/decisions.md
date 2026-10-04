# ASR Pro 2.0 owner decisions

These are the binding owner decisions for the 2.0 mission. They override
older planning reports where they differ. When a feature settles a new
decision, add it here in the same change (D-26).

- **D-01 License.** `AGPL-3.0-or-later` in package.json, full AGPL v3
  LICENSE file. Third-party notices and the About "Open-source licenses"
  row ship with the 2.0 builds.
- **D-02 Version.** 2.0.0. The GitHub release v2.0.0 is published only
  after every milestone validation passed and every release file was
  checked.
- **D-03 CI budget.** `ci.yml` is manual only (`workflow_dispatch`, inputs
  `platforms` and `suite`). No push or pull_request triggers. Exactly 3
  planned runs in the whole mission, plus at most 2 re-runs of failed
  jobs. `build-release.yml` is deleted in M6.
- **D-04 Docs site.** `docs/**` deploys to GitHub Pages on push and is
  changed only in `m7-docs-site`. README changes in
  `m6-readme-whats-new`.
- **D-05 Lint.** oxlint 1.86.0 (exact devDependency) with
  `.oxlintrc.json`: plugins react, typescript, import, unicorn, oxc;
  correctness is error; `react-hooks/rules-of-hooks` error;
  `react-hooks/exhaustive-deps` warn; duplicate React Compiler rules off;
  `vitest/require-mock-type-parameters` off. Errors fail the gate;
  warnings are allowed.
- **D-06 Renderer origin.** The security baseline keeps `file://` and adds
  the build-time CSP meta tag. M2 moves the renderer to `app://asrpro`
  (privileged standard + secure scheme that serves only files under
  `dist/`) and migrates the old `file://` localStorage. `fetch()` to
  `asrpro-media://` needs `corsEnabled: true` and
  `Access-Control-Allow-Origin: app://asrpro`; `<audio>` needs no CORS.
  AudioWorklet modules ship as files.
- **D-07 Sandbox.** The main window runs with `sandbox: true`. The preload
  stays self-contained. `webUtils.getPathForFile` works in the sandboxed
  preload. Never post a transfer list that holds the message's own
  buffer: post `Int16Array` without a transfer list.
- **D-08 Auto-copy fix.** The permission handlers also allow
  `clipboard-sanitized-write` for trusted app URLs; `clipboard-read`
  stays denied.
- **D-09 History search.** One FTS5 table with
  `tokenize = "trigram remove_diacritics 1"`. A query with any token
  shorter than 3 characters uses `LIKE` over title, text, and original
  file name. `node:sqlite` tests run only in the vitest node project.
  Suppress only the SQLite ExperimentalWarning.
- **D-10 Audio copies.** Imported files keep a WebM/Opus 32 kbps mono copy
  encoded from the decoded 16 kHz PCM, streamed to main in chunks. If
  Opus encoding is not available, keep the 16 kHz WAV. Dictation keeps
  MediaRecorder webm/opus at 32 kbps.
- **D-11 Transcript editing.** Edit a row's transcript text and segment
  texts, and rename its title. The original engine text and segments stay
  stored, so "Revert to original" and Reprocess work. Search and exports
  use the edited text.
- **D-12 History tools.** Multi-select mode; single and bulk delete with
  an undo toast (about 8 s; files are removed after the window closes or
  at quit); "Delete audio only"; audio retention settings (max age, max
  total size), default off. The retention sweep removes only audio files,
  never rows or text; such rows show "Audio removed".
- **D-13 Import queue.** Picked or dropped files wait in a queue and run
  one at a time. Each item shows its state. The user can cancel one item
  or all. The queue survives switching views, not an app restart.
- **D-14 Small fixes.** About gets "Open data folder" and "Open log
  folder"; Configuration gets a "Show overlay" toggle; cancel on a model
  download removes the partial file; every dropdown supports full
  keyboard navigation; a local error log under `<dataDir>/logs` rotates
  at about 2 MB x 3 files and never holds transcript text or audio.
- **D-15 Window.** `resizable: false`, `maximizable: false`,
  `fullscreenable: false`.
- **D-16 Live captions.** Off by default. Captions model `auto` is
  `whisper-tiny-en` for English-only final models, `whisper-tiny`
  otherwise; downloaded after a prompt on first enable. Shown in a Home
  panel (committed normal, tentative muted) and in the overlay only when
  `captions.showInOverlay` is on. The final pass replaces the live text.
  Live text is never saved.
- **D-17 Cancelled transcription.** A cancelled transcription saves a row
  with status `cancelled`, audio kept, no text, shown as
  "Not transcribed".
- **D-18 GPU.** Metal on macOS only. Windows and Linux ship CPU engine
  packages and the GPU toggle is disabled there with a reason.
  `ASRPRO_DISABLE_GPU` really forces CPU. A GPU init failure falls back
  to CPU with a notice.
- **D-19 Speaker labels.** On request only, count Auto or 2-6.
  Segmentation pyannote 3.0 (MIT) is fetched at build time with a sha256
  check and bundled. Embedding TitaNet-small (CC-BY-4.0) downloads on
  demand with a pinned sha256. Limit 2 h of audio. Labels hidden when
  only 1 speaker is found. The UI says labels are estimated on the
  device. Windows arm64 uses the sherpa-onnx WASM package.
- **D-20 Data folder per build type.** Windows installed:
  `%LOCALAPPDATA%\ASR Pro\data`; portable: beside the exe. Linux
  deb/rpm/AppImage: `${XDG_DATA_HOME:-~/.local/share}/asrpro`; tar.gz:
  beside the app. macOS in Applications:
  `~/Library/Application Support/ASR Pro/data`; elsewhere beside the app,
  with the profile path as fallback when that place is read-only.
- **D-21 Linux packages.** deb/rpm carry homepage, license, maintainer,
  and dependencies; deb depends include `libgbm1` and `libasound2` (or
  `libasound2t64` as an alternative). The arm64 AppImage ships only if it
  is proven to start on a stock distro; otherwise it is skipped and the
  README says so.
- **D-22 Updater.** electron-updater 6.8.9, opt-in, off by default.
  Self-update for nsis, AppImage, deb, rpm; notice mode (GitHub
  releases/latest link) for macOS, Windows portable, and tar.gz. The
  `x-user-staging-id` header is empty; the cache lives under
  `<dataDir>/cache/updater`; at most one automatic check per 24 h; no
  network request at start while the toggle is off.
- **D-23 Test seams.** No in-app test switches. Allowed seams only:
  `ASRPRO_DATA_DIR`, `VITE_DEV_SERVER_URL`, `ASRPRO_TEST_ASSETS`,
  Chromium command-line switches, `--inspect`, build-time
  electron-builder config (`-c.*`), the engine host transport seam, and
  `dev-app-update.yml` with `forceDevUpdateConfig` in unpackaged runs.
  `ASRPRO_NO_ACTIVATE` and `ASRPRO_FAKE_AUDIO_FILE` are not added.
- **D-24 Home stats.** "This week" is a rolling last 7 days by
  `created_at`. Failed and cancelled rows are excluded from word and
  minute sums; the recordings count counts completed rows only.
- **D-25 What's new.** Home "What's new" comes from
  `shared/release-notes.json`, translated in all 8 locales.
- **D-26 Specs.** `specs/ddd.md` (glossary with the naming tree) and
  `specs/decisions.md` (this file) live at the repo root. Features update
  them when they settle naming or behavior.
- **D-27 Engine test resources.** Engine tests on the Mac host use CPU
  with 2 threads by default; GPU tests are opt-in with
  `ASRPRO_TEST_GPU=1`.
- **D-28 Validation harness.** The harness stays outside the repo at
  `/Volumes/External1TB/data/_custom/asrpro-validation/harness/`. Workers
  may extend it if it stays backward compatible and its README is
  updated. CI uses in-repo scripts.
- **D-29 Undo over confirm.** Delete confirmations are replaced by undo
  toasts (single and bulk).
- **D-30 Legacy engine files.** `scripts/prepare-whisper-addon.cjs`,
  `scripts/patch-elf-runpath.cjs`, and the `postinstall` script are
  deleted in M1; `after-pack` is rewritten; useful logic from
  `electron/whisper-engine.cjs` moves into the new model manager and
  engine host.
- **D-31 File windowing.** Long files transcribe in 120 s windows with
  10 s overlap: accept segments that end inside the window, shift times
  by the window start, account for MP3/HE-AAC decoder lead-in offsets.
  Resampling uses the app's own windowed-sinc resampler, not
  OfflineAudioContext.
- **D-32 Time and memory targets.** Cancel finishes in less than 1 s;
  live captions lag about 1-2 s; history pages hold 50 rows; search
  answers in less than 200 ms on 10,000 rows; a 3-hour import keeps the
  main window below about 1.3 GB; the UI never freezes during model load
  or transcription.
- **D-33 Privacy.** No telemetry. Network only for model downloads the
  user starts and update checks the user turns on.
- **D-34 Multilingual tiny model.** `whisper-tiny` (`ggml-tiny.bin`) is
  downloaded once into the test assets in M1, its whisper.cpp SHA-1 is
  verified, and it is pinned in `shared/models.json`.
- **D-35 Out of scope.** Resizable window, accounts and cloud sync,
  signing and notarization, app stores, hosted API, Intel Mac builds,
  auto-paste into other apps, light theme, VAD as a user feature, mic
  processing options, word-level highlight (segment highlight is in
  scope).
- **D-36 Settings and IPC foundation.** `settings.json` is
  `{schemaVersion: 2, values}` with flat dotted keys; keys this build does
  not know stay under `values._unknown` and survive every save. Each write
  is atomic (temp file, fsync, rename). A file that is not valid JSON or
  has the wrong shape is renamed to `settings.json.corrupt-<ms>`, defaults
  are written, and the log records it. Legacy `app-settings.json` and
  `overlay-settings.json` are read only when `settings.json` is missing,
  then renamed to `*.migrated`. Internal keys (`overlay.customBounds`,
  `startup.executablePath`, `updates.lastCheckAt`,
  `updates.dismissedVersion`, `migrations.legacyLocalStorage`) cannot be
  set from the renderer. The renderer reads the old `asrpro.selectedModel`
  and `asrpro.audioInputDevice` localStorage keys and hands them to
  `settings:import-legacy` once; the keys stay in place. The first call sets
  `migrations.legacyLocalStorage` even when no value is accepted, and the
  renderer skips the call when the marker is already set.
- **D-37 IPC contract.** The preload exposes only `invoke`, `send`, and
  `on`, limited to the channels in `shared/ipc-channels.json` (a drift test
  compares the lists). Every handler checks the sender window and the
  payload, and replies `{ok: true, value}` or `{ok: false, error: {code,
  params?, detail?}}` with a code from `shared/error-codes.json`. The
  renderer turns the reply into an `AppError` in `src/lib/bridge.ts` and
  picks the message from the code; English error text is never matched.
- **D-38 Error log.** `logs/asrpro.log` rotates at 2 MB and keeps 3 files.
  It records engine, IPC, and migration errors as timestamp, level, scope,
  code, and a detail capped at 500 characters. Validation messages name the
  failing field, never its value. Transcript text and audio never reach the
  log.
- **D-39 Linux window flags.** Electron ignores `setMaximizable` on Linux
  and `isMaximizable()` always answers true. The main window overrides
  `isMaximizable` to report false there; the window still cannot be
  maximized (not resizable, `maximize` is undone).
- **D-40 Security baseline on `file://`.** The sender check also compares
  the frame URL with the app URL (the built `dist/index.html`, or
  `VITE_DEV_SERVER_URL` when not packaged); the overlay role accepts only
  its `data:text/html` page. One `web-contents-created` guard blocks
  `will-navigate`, `will-frame-navigate`, `will-redirect`, and
  `will-attach-webview` for every URL except the app page, and denies every
  `window.open`. External pages and folders open only through the
  `shell:open` targets `repo`, `issues` (the new-issue form), `releases`,
  `data-folder`, and `log-folder`; `openExternal` accepts only
  `https://github.com/surajmandalcell/asrpro` and below. Both windows
  share `secureWebPreferences` (sandbox, context isolation, no Node). The
  production CSP meta tag comes from a build-only Vite plugin; the overlay
  data URL carries `default-src 'none'` with a sha256 of its inline script.
  Saved 1.x history audio (data URLs) plays through a blob URL because the
  CSP has no `data:` in `media-src`. A file dropped outside a drop zone is
  cancelled in the renderer, and the navigation guard backs it up.
