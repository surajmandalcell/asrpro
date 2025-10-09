# ASRPro Linux Frontend Implementation Plan

## Overview

This document outlines a comprehensive implementation plan for the GTK4-based Linux frontend of ASRPro. The plan covers the phased approach, detailed project structure, required dependencies, technical considerations, and integration strategy with the Python sidecar backend.

## Phased Implementation Approach

### Phase 1: Foundation Enhancement (Weeks 1-2)
**Goal**: Establish robust core functionality and backend integration

#### Tasks:
1. **Enhance Backend Communication**
   - Implement proper HTTP client with error handling
   - Add retry mechanisms with exponential backoff
   - Create request/response models with serde
   - Implement authentication flow

2. **Basic File Operations**
   - Add file selection dialog
   - Implement basic file validation
   - Create file upload functionality
   - Add progress indication for uploads

3. **Improved Error Handling**
   - Custom error types with proper error propagation
   - User-friendly error messages
   - Graceful degradation when backend is unavailable

#### Deliverables:
- Functional file upload and transcription
- Robust error handling system
- Basic settings persistence

### Phase 2: User Interface Expansion (Weeks 3-4)
**Goal**: Create a comprehensive user interface for all core features

#### Tasks:
1. **Main Window Redesign**
   - Implement MVVM pattern
   - Create responsive layout
   - Add menu bar and toolbar
   - Implement status bar

2. **File Management UI**
   - Drag-and-drop support for audio files
   - Recent files list
   - File preview with metadata
   - Batch processing interface

3. **Model Selection Interface**
   - Dynamic model list from backend
   - Model configuration options
   - Model download management
   - Performance indicators

#### Deliverables:
- Complete user interface
- File management system
- Model selection and configuration

### Phase 3: Advanced Features (Weeks 5-6)
**Goal**: Implement advanced features and improve user experience

#### Tasks:
1. **Real-time Updates**
   - WebSocket client implementation
   - Live transcription progress
   - Real-time status updates
   - Notification system

2. **Audio Processing**
   - Audio preview functionality
   - Waveform visualization
   - Audio format conversion
   - Quality settings

3. **Configuration Management**
   - Settings dialog
   - Configuration persistence
   - Import/export settings
   - Advanced options

#### Deliverables:
- Real-time transcription updates
- Audio preview and visualization
- Comprehensive settings management

### Phase 4: Polish and Integration (Weeks 7-8)
**Goal**: Finalize implementation and ensure seamless integration

#### Tasks:
1. **Performance Optimization**
   - Memory usage optimization
   - UI responsiveness improvements
   - Async operation optimization
   - Resource management

2. **Platform Integration**
   - Desktop entry file
   - MIME type registration
   - System notifications
   - File association

3. **Testing and Documentation**
   - Unit tests for core functionality
   - Integration tests with backend
   - User documentation
   - Developer documentation

#### Deliverables:
- Production-ready application
- Complete test suite
- Comprehensive documentation

## Detailed Project Structure

```
frontends/linux/
├── Cargo.toml                 # Rust dependencies and metadata
├── meson.build                # Build configuration
├── README.md                  # Installation and usage instructions
├── src/                       # Source code
│   ├── main.rs                # Application entry point
│   ├── app.rs                 # Application struct and setup
│   ├── ui/                    # UI components
│   │   ├── mod.rs
│   │   ├── main_window.rs     # Main application window
│   │   ├── menu_bar.rs        # Application menu
│   │   ├── toolbar.rs         # Main toolbar
│   │   ├── status_bar.rs      # Status information
│   │   ├── file_panel.rs      # File management panel
│   │   ├── model_panel.rs     # Model selection panel
│   │   ├── transcription_panel.rs # Transcription results
│   │   ├── settings_dialog.rs # Settings configuration
│   │   ├── progress_dialog.rs # Progress indication
│   │   └── widgets/           # Reusable UI widgets
│   │       ├── mod.rs
│   │       ├── audio_preview.rs
│   │       ├── waveform.rs
│   │       ├── file_drop.rs
│   │       └── model_selector.rs
│   ├── models/                # Data models
│   │   ├── mod.rs
│   │   ├── app_state.rs       # Application state
│   │   ├── audio_file.rs      # Audio file metadata
│   │   ├── transcription.rs   # Transcription data
│   │   ├── model_info.rs      # Model information
│   │   └── settings.rs        # Application settings
│   ├── services/              # Business logic
│   │   ├── mod.rs
│   │   ├── backend_client.rs  # HTTP API client
│   │   ├── websocket_client.rs # WebSocket client
│   │   ├── file_manager.rs    # File operations
│   │   ├── model_manager.rs   # Model management
│   │   └── config_manager.rs  # Configuration management
│   ├── utils/                 # Utility functions
│   │   ├── mod.rs
│   │   ├── error.rs           # Error types
│   │   ├── audio.rs           # Audio utilities
│   │   └── async_utils.rs     # Async utilities
│   └── resources/             # Resource management
│       ├── mod.rs
│       └── resources.rs       # Resource loading
├── data/                      # Application resources
│   ├── resources.xml          # GResource definition
│   ├── style.css              # Application styling
│   ├── icons/                 # Application icons
│   │   ├── scalable/
│   │   └── symbolic/
│   └── ui/                    # UI definition files
│       ├── main_window.ui
│       ├── settings_dialog.ui
│       └── menus.ui
├── tests/                     # Test files
│   ├── integration/
│   │   ├── backend_tests.rs
│   │   └── file_tests.rs
│   └── unit/
│       ├── models_tests.rs
│       └── services_tests.rs
└── scripts/                   # Build and utility scripts
    ├── build.sh
    ├── test.sh
    └── package.sh
```

## Required Dependencies

### System Dependencies
```bash
# Ubuntu/Debian
sudo apt install \
    build-essential \
    meson \
    libgtk-4-dev \
    libjson-glib-dev \
    libsoup-3.0-dev \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev

# Fedora
sudo dnf install \
    meson \
    gtk4-devel \
    json-glib-devel \
    libsoup3-devel \
    gstreamer1-devel \
    gstreamer1-plugins-base-devel

# Arch Linux
sudo pacman -S \
    meson \
    gtk4 \
    json-glib \
    libsoup \
    gstreamer
```

### Rust Dependencies (Cargo.toml)
```toml
[package]
name = "asrpro-gtk4"
version = "1.0.0"
edition = "2021"

[dependencies]
# GTK4 and GLib
gtk4 = "0.9"
gio = "0.19"
glib = "0.19"

# Async runtime
tokio = { version = "1.0", features = ["full"] }
tokio-tungstenite = "0.20"

# HTTP client
reqwest = { version = "0.11", features = ["json", "multipart", "stream"] }

# Serialization
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"

# Error handling
thiserror = "1.0"
anyhow = "1.0"

# Utilities
uuid = { version = "1.0", features = ["v4", "serde"] }
url = "2.0"
mime = "0.3"
mime_guess = "2.0"

# Audio processing
gstreamer = "0.21"
gstreamer-app = "0.21"

# Configuration
config = "0.14"
dirs = "5.0"

# Logging
log = "0.4"
env_logger = "0.10"

# File operations
tempfile = "3.0"
walkdir = "2.0"

[dev-dependencies]
tempfile = "3.0"
mockito = "1.0"
tokio-test = "0.4"
```

## Technical Considerations

### 1. Threading Model

GTK4 requires all UI operations to be performed on the main thread. The implementation will use:

```rust
use glib::{MainContext, clone};

// For async operations that need to update UI
let context = MainContext::default();
context.spawn_local(async move {
    // Perform async operation
    let result = some_async_function().await;
    
    // Update UI on main thread
    main_context.invoke(move || {
        // Update UI components
    });
});
```

### 2. State Management

Centralized state management using Arc<Mutex<T>>:

```rust
use std::sync::{Arc, Mutex};

pub struct AppState {
    pub current_files: Arc<Mutex<Vec<AudioFile>>>,
    pub transcriptions: Arc<Mutex<HashMap<Uuid, Transcription>>>,
    pub settings: Arc<Mutex<Settings>>,
    pub backend_status: Arc<Mutex<BackendStatus>>,
}

impl AppState {
    pub fn new() -> Self {
        Self {
            current_files: Arc::new(Mutex::new(Vec::new())),
            transcriptions: Arc::new(Mutex::new(HashMap::new())),
            settings: Arc::new(Mutex::new(Settings::load())),
            backend_status: Arc::new(Mutex::new(BackendStatus::Disconnected)),
        }
    }
}
```

### 3. Error Handling Strategy

Comprehensive error handling with custom error types:

```rust
use thiserror::Error;

#[derive(Debug, Error)]
pub enum AsrProError {
    #[error("Backend connection failed: {source}")]
    BackendConnection { source: reqwest::Error },
    
    #[error("File operation failed: {path} - {source}")]
    FileOperation { path: PathBuf, source: std::io::Error },
    
    #[error("Transcription failed: {message}")]
    Transcription { message: String },
    
    #[error("Configuration error: {message}")]
    Configuration { message: String },
    
    #[error("WebSocket error: {source}")]
    WebSocket { source: tokio_tungstenite::tungstenite::Error },
}

pub type Result<T> = std::result::Result<T, AsrProError>;
```

### 4. Configuration Management

Platform-agnostic configuration management:

```rust
use config::{Config, ConfigError, File};
use dirs::config_dir;

pub struct Settings {
    config: Config,
}

impl Settings {
    pub fn load() -> Result<Self, ConfigError> {
        let config_path = config_dir()
            .unwrap_or_else(|| PathBuf::from("."))
            .join("asrpro")
            .join("config.toml");
        
        let config = Config::builder()
            .add_source(File::with_name(config_path.to_str().unwrap()))
            .build()?;
        
        Ok(Settings { config })
    }
    
    pub fn get_backend_url(&self) -> String {
        self.config.get_string("backend.url")
            .unwrap_or_else(|_| "http://localhost:8000".to_string())
    }
}
```

## Integration Strategy

### 1. Backend API Integration

The frontend will integrate with the Python backend through a well-defined API:

```rust
pub struct BackendClient {
    client: reqwest::Client,
    base_url: String,
    auth_token: Option<String>,
}

impl BackendClient {
    pub fn new(base_url: String) -> Self {
        Self {
            client: reqwest::Client::new(),
            base_url,
            auth_token: None,
        }
    }
    
    pub async fn health_check(&self) -> Result<HealthStatus> {
        let response = self.client
            .get(&format!("{}/health", self.base_url))
            .send()
            .await?;
        
        if response.status().is_success() {
            let health: HealthStatus = response.json().await?;
            Ok(health)
        } else {
            Err(AsrProError::BackendConnection {
                source: reqwest::Error::from(response.error_for_status().unwrap_err()),
            })
        }
    }
    
    pub async fn upload_file(&self, file_path: &Path) -> Result<TranscriptionResponse> {
        let file_content = tokio::fs::read(file_path).await?;
        let file_name = file_path.file_name()
            .and_then(|n| n.to_str())
            .unwrap_or("audio.file");
        
        let form = reqwest::multipart::Form::new()
            .part("file", reqwest::multipart::Part::bytes(file_content)
                .file_name(file_name.to_string()));
        
        let response = self.client
            .post(&format!("{}/v1/audio/transcriptions", self.base_url))
            .multipart(form)
            .send()
            .await?;
        
        if response.status().is_success() {
            let result: TranscriptionResponse = response.json().await?;
            Ok(result)
        } else {
            Err(AsrProError::Transcription {
                message: format!("Upload failed: {}", response.status()),
            })
        }
    }
}
```

### 2. WebSocket Integration

Real-time updates through WebSocket:

```rust
use tokio_tungstenite::{connect_async, tungstenite::Message};
use futures_util::{SinkExt, StreamExt};

pub struct WebSocketClient {
    sender: mpsc::UnboundedSender<Message>,
}

impl WebSocketClient {
    pub async fn connect(url: &str) -> Result<Self> {
        let (ws_stream, _) = connect_async(url).await?;
        let (mut write, mut read) = ws_stream.split();
        
        let (tx, mut rx) = mpsc::unbounded_channel();
        
        // Spawn task to handle outgoing messages
        tokio::spawn(async move {
            while let Some(msg) = rx.recv().await {
                if let Err(e) = write.send(msg).await {
                    eprintln!("Failed to send WebSocket message: {}", e);
                    break;
                }
            }
        });
        
        // Spawn task to handle incoming messages
        tokio::spawn(async move {
            while let Some(msg) = read.next().await {
                match msg {
                    Ok(Message::Text(text)) => {
                        // Handle incoming message
                        if let Ok(event) = serde_json::from_str::<WebSocketEvent>(&text) {
                            // Process event
                        }
                    }
                    Err(e) => eprintln!("WebSocket error: {}", e),
                    _ => {}
                }
            }
        });
        
        Ok(WebSocketClient { sender: tx })
    }
    
    pub fn send_message(&self, message: &str) -> Result<()> {
        self.sender.send(Message::Text(message.to_string()))
            .map_err(|_| AsrProError::WebSocket {
                source: tokio_tungstenite::tungstenite::Error::AlreadyClosed,
            })
    }
}
```

### 3. File Management Integration

Integration with system file managers:

```rust
use gtk4::{DragSource, DropTarget, FileChooserNative};

pub struct FileDropTarget {
    drop_target: DropTarget,
}

impl FileDropTarget {
    pub fn new() -> Self {
        let drop_target = DropTarget::new(gio::File::static_type());
        drop_target.set_actions(gdk::DragAction::COPY);
        
        let target = Self { drop_target };
        
        target.drop_target.connect_drop(|_target, value, _x, _y| {
            if let Some(file) = value.get::<gio::File>() {
                if let Some(path) = file.path() {
                    // Handle dropped file
                    return true;
                }
            }
            false
        });
        
        target
    }
}

pub struct FileChooser {
    native: FileChooserNative,
}

impl FileChooser {
    pub fn new_audio_chooser() -> Self {
        let filter = gtk4::FileFilter::new();
        filter.add_mime_type("audio/*");
        filter.add_mime_type("video/*");
        filter.set_name(Some("Audio and Video Files"));
        
        let native = FileChooserNative::builder()
            .title("Select Audio File")
            .accept_label("Transcribe")
            .cancel_label("Cancel")
            .modal(true)
            .filter(&filter)
            .build();
        
        Self { native }
    }
    
    pub async fn get_selected_file(&self) -> Option<PathBuf> {
        // Implementation for async file selection
    }
}
```

## Build and Deployment

### 1. Build Configuration (meson.build)

```meson
project('asrpro-gtk4', 'rust',
  version: '1.0.0',
  default_options: ['warning_level=2',
                    'c_std=c99',
                    'rust_std=2021'])

# Dependencies
gtk4_dep = dependency('gtk4')
json_glib_dep = dependency('json-glib-1.0')
libsoup_dep = dependency('libsoup-3.0')
gstreamer_dep = dependency('gstreamer-1.0')

# Resource compilation
glib_compile_resources = find_program('glib-compile-resources')
resources = custom_target('resources',
  input: 'data/resources.xml',
  output: 'resources.gresource',
  command: [glib_compile_resources,
            '--sourcedir', join_paths(meson.current_source_dir(), 'data'),
            '--target', '@OUTPUT@',
            '--generate-source',
            '@INPUT@'],
  install: true,
  install_dir: get_option('bindir')
)

# Rust executable
executable('asrpro-gtk4',
  'src/main.rs',
  dependencies: [gtk4_dep, json_glib_dep, libsoup_dep, gstreamer_dep],
  rust_args: ['--extern', 'resources=@0@'.format(resources.full_path())],
  install: true,
  install_dir: get_option('bindir')
)

# Install desktop file
install_data('data/com.asrpro.gtk4.desktop',
  install_dir: join_paths(get_option('datadir'), 'applications')
)

# Install icons
install_subdir('data/icons',
  install_dir: join_paths(get_option('datadir'), 'icons')
)
```

### 2. Desktop Integration

**Desktop Entry File** (`data/com.asrpro.gtk4.desktop`):
```ini
[Desktop Entry]
Type=Application
Name=ASRPro
Comment=Advanced Speech Recognition
Exec=asrpro-gtk4
Icon=com.asrpro.gtk4
Terminal=false
Categories=AudioVideo;Audio;
MimeType=audio/*;video/*;
StartupNotify=true
```

### 3. Packaging

```bash
#!/bin/bash
# scripts/package.sh

set -e

# Build application
meson setup build
ninja -C build

# Create package directory
PKG_DIR="asrpro-linux-x64"
mkdir -p "$PKG_DIR"

# Copy executable
cp build/asrpro-gtk4 "$PKG_DIR/"

# Copy resources
cp -r data/icons "$PKG_DIR/"
cp data/com.asrpro.gtk4.desktop "$PKG_DIR/"

# Create archive
tar czf "asrpro-linux-x64.tar.gz" "$PKG_DIR"

echo "Package created: asrpro-linux-x64.tar.gz"
```

## Testing Strategy

### 1. Unit Tests
```rust
#[cfg(test)]
mod tests {
    use super::*;
    
    #[tokio::test]
    async fn test_backend_client_health_check() {
        let client = BackendClient::new("http://localhost:8000".to_string());
        // Mock server for testing
        let result = client.health_check().await;
        assert!(result.is_ok());
    }
    
    #[test]
    fn test_audio_file_validation() {
        let valid_file = PathBuf::from("test.mp3");
        assert!(AudioFile::is_valid_format(&valid_file));
        
        let invalid_file = PathBuf::from("test.txt");
        assert!(!AudioFile::is_valid_format(&invalid_file));
    }
}
```

### 2. Integration Tests
```rust
#[tokio::test]
async fn test_full_transcription_flow() {
    // Start mock backend
    let mock_server = start_mock_backend().await;
    
    // Create application instance
    let app = ASRProApp::new();
    
    // Simulate file upload
    let test_file = create_test_audio_file().await;
    let result = app.transcribe_file(&test_file).await;
    
    assert!(result.is_ok());
    assert!(!result.unwrap().text.is_empty());
}
```

## Conclusion

This implementation plan provides a comprehensive roadmap for developing a feature-rich GTK4-based Linux frontend for ASRPro. The phased approach ensures steady progress while maintaining code quality and user experience. The modular architecture allows for easy maintenance and future enhancements, while the integration strategy ensures seamless communication with the Python backend.

The plan emphasizes native Linux integration, robust error handling, and a user-friendly interface that leverages the strengths of both GTK4 and Rust. With proper execution, this implementation will provide ASRPro users with an excellent native Linux experience.