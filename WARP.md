# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

ASR Pro is a professional desktop application for AI-powered speech recognition and transcription built with PySide6. It provides real-time transcription via global hotkeys and batch SRT generation for media files.

## Development Commands

### Setup and Installation
```bash
# Clone and setup
python -m pip install -r requirements.txt

# For Parakeet models (optional)
python -m pip install -r requirements-nemo.txt
```

### Running the Application
```bash
# Production mode (module entry point)
python -m asrpro

# Development mode with hot-reload (recommended)
python run.py  # Press 'r' to restart, 'q' to quit

# Direct launcher
python asrpro/main.py
```

### Building
```bash
# Build standalone executable with Nuitka
python build_nuitka.py
# Output: dist/asrpro.exe
```

### Testing and Verification
```bash
# Health check (when server is running)
curl http://127.0.0.1:7341/health

# Test server endpoints
curl http://127.0.0.1:7341/v1/models
curl -X POST http://127.0.0.1:7341/v1/audio/transcriptions -F file=@audio.wav
```

### Single Test Execution
Currently no formal test suite exists. For testing new functionality:
- Launch app and verify tray icon, hotkey functionality
- Test UI components and pages manually
- Verify server endpoints with curl commands above

## Architecture Overview

### Entry Points and Process Management
- **`run.py`**: Development launcher with hot-reload (spawns `python -m asrpro`)
- **`asrpro/__main__.py`**: Module entry point
- **`asrpro/main.py`**: Application bootstrap with single instance enforcement
- **Single instance enforcement**: Automatically kills existing processes with PID locking

### Core Architecture Patterns

#### Model Management System
- **`models.py`**: Consolidated model loaders for different ASR engines
  - `Parakeet06BLoader`, `Parakeet11BLoader` (NVIDIA NeMo)
  - `WhisperMediumOnnxLoader` (faster-whisper/CTranslate2)
- **`model_manager.py`**: Lazy single-resident model manager
  - Device auto-detection: CUDA → Vulkan → CPU
  - Thread-safe loading/unloading with 30-minute auto-unload
  - Handles mixed precision GPU inference and int8 CPU quantization

#### Configuration System
- **`config.py`**: Centralized JSON-based configuration with dot notation access
- **Location**: `config/config.json` (project-local, migrates from `~/.asrpro/config.json`)
- **Device preferences**: `["cuda", "vulkan", "cpu"]` with automatic fallback
- **Hotkey settings**: Default `<ctrl>+<alt>+t` with overlay and auto-paste

#### Server Component
- **`server.py`**: FastAPI server with OpenAI-compatible endpoints
- **Port**: 7341 (configurable)
- **Key endpoints**:
  - `/v1/models` - List available models
  - `/v1/audio/transcriptions` - Transcribe audio
  - `/v1/srt` - Generate subtitle files
  - `/health` - System health check

### UI Architecture (PySide6)

```
asrpro/ui/
├── components/          # Reusable UI components with glassmorphism design
│   ├── button.py        # Custom styled buttons
│   ├── overlay.py       # Recording status overlay
│   ├── tray.py          # System tray with dark mode icon support
│   └── ...
├── layouts/             # Window and layout management
│   ├── main_window.py   # Main application window with frameless design
│   ├── sidebar.py       # Navigation sidebar
│   └── ...
├── pages/               # Application pages/views
│   ├── models_page.py   # Model selection and loading
│   ├── transcribe_file_page.py  # SRT generation interface
│   └── ...
└── styles/
    └── dark_theme.py    # Glassmorphism styling with transparency
```

#### UI Design Principles
- **Glassmorphism**: Semi-transparent elements with blur effects
- **Dark mode**: Automatic system theme detection with icon inversion
- **Frameless windows**: Custom title bar implementation
- **System integration**: Tray icon, context menus, global hotkeys

## Important Technical Details

### Device and Performance Management
- **Automatic device selection**: CUDA preferred, falls back through Vulkan to CPU
- **Model lifecycle**: Single resident model with automatic unloading after 30 minutes
- **Memory optimization**: Mixed precision for GPU, int8 quantization for CPU
- **Thread safety**: All model operations are thread-safe

### Audio Processing Pipeline
- **Recording**: 16kHz mono WAV format via sounddevice
- **Processing**: librosa for analysis, ffmpeg for format conversion
- **Output**: Direct transcription paste or SRT file generation

### Configuration Management
- **Hierarchical config**: Device → server → UI → audio → models → hotkey sections
- **Runtime updates**: Configuration changes applied immediately
- **Migration**: Automatic migration from legacy `~/.asrpro/config.json`
- **Dot notation access**: `config.get("ui.theme")` style access patterns

### Process Management
- **Single instance**: PID-based locking with user confirmation for existing instances
- **Process cleanup**: Automatic termination of zombie processes
- **Lock file**: `config/asrpro.lock` (safe to delete if app crashes)

## Model Integration Patterns

### Adding New Models
1. Create loader class in `models.py` following existing patterns
2. Implement required methods: `load_model()`, `transcribe()`, `unload()`
3. Add device-specific optimization (CUDA/Vulkan/CPU)
4. Register in `model_manager.py` model registry
5. Add UI controls in `models_page.py`

### Device Support Implementation
- **CUDA detection**: Check torch.cuda.is_available()
- **Vulkan fallback**: Use onnxruntime VulkanExecutionProvider
- **CPU optimization**: Enable int8 quantization and threading

## Security and Configuration Notes

### Server Security
- **Bind address**: Defaults to 127.0.0.1:7341 (localhost only)
- **Public exposure**: Requires reverse proxy and authentication for public access
- **API compatibility**: OpenAI-compatible endpoints for easy integration

### Configuration Security
- **Local storage**: All config stored in project directory
- **No sensitive data**: No API keys or tokens stored in configuration
- **Process isolation**: Single instance enforcement prevents conflicts

## Coding Patterns and Conventions

### Python Standards
- **Version**: Python 3.11+ required
- **Style**: PEP 8 with 4-space indentation, snake_case for functions, CapWords for classes
- **Type hints**: Preferred throughout codebase
- **Imports**: `from __future__ import annotations` for forward references

### UI Patterns
- **Component isolation**: Reusable components in `ui/components/`
- **Page structure**: Each page inherits from `base_page.py`
- **Theme consistency**: All styling through `dark_theme.py`
- **Event handling**: Qt signals/slots pattern throughout

### Error Handling
- **Graceful degradation**: Fallback device selection, model unloading
- **User feedback**: Status updates through UI and tray notifications
- **Process recovery**: Automatic cleanup of crashed instances

## Build and Deployment

### Nuitka Build Process
- **Target**: Single executable with all dependencies bundled
- **Platform**: Windows primary (cross-platform compatible)
- **Plugins**: PySide6 plugin enabled for Qt integration
- **Assets**: Automatic inclusion of assets/ directory
- **Size optimization**: Excludes tkinter, matplotlib, and other unused modules

### Dependencies Management
- **Core requirements**: `requirements.txt` with pinned versions for reproducibility
- **Optional models**: `requirements-nemo.txt` for Parakeet models
- **System dependencies**: FFmpeg required for audio processing
- **GPU acceleration**: CUDA toolkit for optimal performance (optional)