# ASR Pro - TODO List

Tasks and improvements for the ASR Pro application.

## ✅ Completed Tasks

### Project Organization
- [x] Fixed misspelled 'srcipts' folder → 'scripts'
- [x] Moved utility scripts to scripts/ folder (run.py, build_nuitka.py, asrpro_run.py)
- [x] Created comprehensive Makefile with all common commands
- [x] Added test directory with basic test suite
- [x] Created macOS-specific requirements file
- [x] Added standard configuration files (LICENSE, pyproject.toml, setup.py, .editorconfig)
- [x] Updated config.py to use platform-specific paths (~/Library/Application Support/ on macOS)

### macOS Fixes
- [x] **tray.py** - Fixed Windows-specific `winreg` import with platform detection
- [x] **build_nuitka.py** - Fixed `.exe` extension and Windows flags for macOS
- [x] **hotkey.py** - Added accessibility permission checks for macOS
- [x] **System tray** - Fixed icon size (22x22px for macOS menu bar)
- [x] **Dark mode** - Implemented proper dark mode detection for macOS
- [x] **Model loading** - Added MPS (Metal) support for Apple Silicon

## 🚀 Priority Tasks

### 1. Core Functionality
- [ ] **Audio recording** - Test sounddevice on macOS with different device selection
- [ ] **Microphone permissions** - Add proper microphone permission handling for macOS
- [ ] **File dialogs** - Ensure QFileDialog shows native macOS picker properly

### 2. macOS Integration
- [ ] **App bundling** - Test py2app build with `make build-py2app`
- [ ] **Frameless window** - Fix window controls (traffic lights) on macOS
- [ ] **Icon creation** - Create proper .icns icon file for macOS app

### 3. Model Support
- [ ] **Vulkan optimization** - Skip Vulkan on macOS (use MPS or CPU only)
- [ ] **Model caching** - Implement proper model cache management
- [ ] **Auto-unload** - Implement model auto-unload after inactivity

### 4. Server & Network
- [ ] **Port conflict detection** - Check if port 7341 is available before starting
- [ ] **Firewall handling** - Add proper handling for macOS firewall warnings
- [ ] **Unix socket option** - Consider Unix socket for local-only communication

## 📝 Documentation & Testing

### 1. Testing
- [ ] **Expand test coverage** - Add more comprehensive tests
- [ ] **Integration tests** - Add tests for model loading and transcription
- [ ] **Performance tests** - Add benchmarks for different models

### 2. Documentation 
- [ ] **Usage guide** - Create comprehensive user documentation
- [ ] **API documentation** - Document the FastAPI server endpoints
- [ ] **Troubleshooting guide** - Common issues and solutions for macOS

## 🎯 Quick Start

```bash
# Setup development environment
make setup

# Run the application
make run

# Run in development mode with hot-reload  
make dev

# Run tests
make test

# Format code
make format

# Clean build artifacts
make clean

# See all available commands
make help
```

## 📦 Build & Distribution

```bash
# Build with Nuitka (cross-platform)
make build

# Build macOS .app bundle
make build-py2app

# Install macOS-specific dependencies
make install-macos
```

## Project Structure

```
asrpro/
├── asrpro/           # Main application package
├── assets/           # Icons, fonts, and resources
├── config/           # Legacy config location (migrated)
├── scripts/          # Utility and build scripts
├── tests/            # Test suite
├── Makefile          # Build and development commands
├── pyproject.toml    # Modern Python packaging config
├── setup.py          # Backward compatibility & py2app
├── requirements.txt  # Core dependencies
└── requirements-macos.txt  # macOS-specific deps
```
