# ASR Pro - Practical TODO List for macOS

Real fixes needed for the Python/PySide6 codebase to work properly on macOS.

## ✅ Completed macOS Fixes

### 1. Fixed Windows-Specific Code Breaking on macOS
- [x] **tray.py line 22** - `import winreg` fails on macOS
  - Wrap in try/except or use platform detection
  - Use `defaults read -g AppleInterfaceStyle` for macOS dark mode detection
  ```python
  import subprocess
  result = subprocess.run(['defaults', 'read', '-g', 'AppleInterfaceStyle'], 
                         capture_output=True, text=True)
  is_dark = 'Dark' in result.stdout
  ```

- [x] **build_nuitka.py line 40** - Uses `.exe` extension on macOS
  - Change to `.app` or no extension for macOS
  - Remove `--windows-console-mode` flag
  - Remove `--windows-icon-from-ico` for macOS

### 2. Fixed Global Hotkey Permissions
- [x] **hotkey.py** - pynput requires Accessibility permissions on macOS
  - Add check for accessibility permissions
  - Show dialog explaining how to enable in System Settings
  - Gracefully handle when permissions denied
  ```python
  import subprocess
  # Check if Terminal has accessibility access
  result = subprocess.run(['osascript', '-e', 
    'tell application "System Events" to return (name of processes whose frontmost is true)'],
    capture_output=True)
  ```

### 3. Fixed Path Issues
- [x] **Missing asrpro_run.py** - build_nuitka.py references non-existent file
  - Either create asrpro_run.py or change to actual entry point
  - Should probably be `asrpro/__main__.py`

## ✅ Fixed Features on macOS

### 1. System Tray Fixed
- [x] **Menu bar icon wrong size** - macOS needs 22x22px, not 32x32
- [x] **Tray menu styling broken** - Remove Windows-specific CSS that doesn't work
- [x] **Icon not showing in dark mode** - Icon inversion logic might be backwards

### 2. Audio Recording Issues  
- [ ] **Test sounddevice on macOS** - Might need different device selection
- [ ] **Microphone permissions** - Need to request microphone access
  ```python
  # macOS requires Info.plist entries for microphone access
  # Or handle permission denied errors gracefully
  ```

### 3. File Dialog Issues
- [ ] **QFileDialog might not show native macOS picker**
  - Add `QFileDialog.DontUseNativeDialog` flag if broken
  - Or ensure native dialog works properly

## 🟢 Actual Improvements (Within Python/PySide6)

### 1. Better macOS Integration
- [ ] **Proper app bundling**
  - Create simple .app structure with Info.plist
  - Use py2app instead of Nuitka (actually works on macOS)
  ```bash
  pip install py2app
  python setup.py py2app
  ```

- [ ] **Fix frameless window on macOS**
  - Frameless windows have issues with window controls on macOS
  - Either add custom traffic light buttons or use native title bar

- [ ] **Better device detection**
  - Use `platform.system() == 'Darwin'` for macOS detection
  - Handle M1/M2 chips properly for model selection

### 2. Model Loading Fixed
- [x] **Fix CUDA detection on macOS** - No CUDA on macOS
  - Now uses Metal/MPS instead of CUDA on macOS
  ```python
  if platform.system() == 'Darwin':
      device = 'mps' if torch.backends.mps.is_available() else 'cpu'
  ```

- [ ] **Fix Vulkan on macOS** - Vulkan via MoltenVK is sketchy
  - Just use CPU or Metal, don't bother with Vulkan

### 3. Server Improvements
- [ ] **Port conflicts** - Check if port 7341 is available
- [ ] **Firewall warnings** - macOS will warn about network connections
  - Consider Unix socket instead of TCP for local-only

### 4. Configuration Fixes
- [ ] **Use proper macOS paths**
  - Config should go in `~/Library/Application Support/asrpro/`
  - Not in project directory (that's weird)
  ```python
  from pathlib import Path
  config_dir = Path.home() / "Library" / "Application Support" / "asrpro"
  ```

## 🔧 Development Experience

### 1. Testing
- [ ] **Add basic smoke tests**
  ```python
  # test_macos.py
  def test_imports():
      import asrpro
      assert asrpro
  
  def test_no_winreg():
      # Make sure no Windows-specific imports
      from asrpro.ui.components import tray
      assert True
  ```

### 2. Dev Setup
- [ ] **Add macOS-specific requirements**
  ```bash
  # requirements-macos.txt
  pyobjc-framework-Cocoa  # For native macOS APIs if needed
  py2app  # For building .app bundles
  ```

- [ ] **Fix hot-reload script**
  - `run.py` uses msvcrt which doesn't exist on macOS
  - The fallback already exists but test it works

### 3. Documentation
- [ ] **Add macOS setup instructions**
  - How to grant permissions
  - How to install FFmpeg: `brew install ffmpeg`
  - How to handle security warnings

## Priority Order

1. **Fix import errors** - App won't even start without this
2. **Fix permissions** - Core features won't work without accessibility/microphone
3. **Fix device selection** - No CUDA on macOS, use MPS or CPU
4. **Fix paths and config** - Use proper macOS directories
5. **Fix UI issues** - Tray icon, window controls
6. **Improve build** - Use py2app instead of Nuitka

## Quick Wins

```bash
# Test if it even runs
python -m asrpro

# If it crashes, fix imports first
# Then test each component:
python -c "from asrpro.ui.components.tray import build_tray"
python -c "from asrpro.hotkey import ToggleHotkey"
python -c "import sounddevice; print(sounddevice.query_devices())"
```

## Notes

- Don't add Swift/Objective-C code - stick to Python
- Don't add complex native integrations - use what PySide6 provides
- Focus on making existing code work, not adding features
- Test on actual macOS before assuming things work