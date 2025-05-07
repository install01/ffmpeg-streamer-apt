# ffmpeg-streamer-apt
**FFmpeg Streamer GUI**

A simple, cross-platform GUI application for streaming local video files to YouTube, Twitch, or a custom RTMP server using FFmpeg and Tkinter.

---

## 📖 Table of Contents

* [Features](#features)
* [Requirements](#requirements)
* [Installation](#installation)

  * [Clone Repository](#clone-repository)
  * [Install Dependencies](#install-dependencies)
* [Usage](#usage)

  * [Running from Source](#running-from-source)
  * [Standalone Build](#standalone-build)
* [🏗 Building the App](#-building-the-app)

  * [Windows (Standalone .exe)](#windows-standalone-exe)
  * [macOS (.app)](#macos-app)
  * [Linux (Standalone & .desktop)](#linux-standalone--desktop)
  * [Raspberry Pi (ARM)](#raspberry-pi-arm)
* [Configuration File](#configuration-file)
* [Packaging & Distribution](#packaging--distribution)
* [Troubleshooting](#troubleshooting)
* [License](#license)

---

## 🚀 Features

* Select service: YouTube, Twitch, or custom RTMP URL
* Enter stream key or full RTMP URL
* Browse and optionally loop local video files
* Configure resolution (720p/1080p), FPS, and bitrate
* Schedule start time (YYYY-MM-DD HH\:MM\:SS)
* Live progress display and logging
* Cross-platform standalone builds with embedded Python & FFmpeg

---

## 🛠 Requirements

**⚠️ Warning:** On some systems, the `python` command may still invoke Python 2.x. To ensure you’re using Python 3, always run scripts and commands with `python3` (e.g., `python3 ffmpeg_streamer_tk.py`).

* Python 3.7 or higher (for running from source)

* Tkinter (included in standard Python installers)

* FFmpeg (bundled in standalone builds or install via package manager)

* Python 3.7 or higher (for running from source)

* Tkinter (included in standard Python installers)

* FFmpeg (bundled in standalone builds or install via package manager)

---

## 📥 Installation

### Clone Repository

```bash
git clone https://github.com/yourusername/ffmpeg-streamer-gui.git
cd ffmpeg-streamer-gui
```

### Install Dependencies

You can edit configuration or script files directly using the `nano` text editor before installation or running. For example:

```bash
nano config.json
nano ffmpeg_streamer_tk.py
```

```bash
pip install -r requirements.txt
```

> *`requirements.txt` includes `pyinstaller` and any other Python dependencies.*

```bash
pip install -r requirements.txt
```

> *`requirements.txt` includes `pyinstaller` and any other Python dependencies.*

---

## ▶️ Usage

### Running from Source

```bash
python ffmpeg_streamer_tk.py
```

* A `config.json` file will be created on first run to store your settings.
* Ensure `ffmpeg` is available in your system PATH, or provide it via configuration.

### Standalone Build

Download a prebuilt binary from the [Releases](https://github.com/yourusername/ffmpeg-streamer-gui/releases):

* **Windows:** `ffmpeg_streamer_tk.exe`
* **macOS:** `ffmpeg_streamer.app.zip`
* **Linux:** `ffmpeg_streamer_linux.tar.gz`

Unpack and launch via double-click. On macOS, you may need to bypass Gatekeeper:

```bash
xattr -rd com.apple.quarantine dist/ffmpeg_streamer.app
```

---

## 🏗 Building the App

### Windows (Standalone .exe)

1. **Prepare environment**:

   * Install [Python 3.7+ for Windows](https://www.python.org/downloads/windows/).
   * Install PyInstaller:

     ```bat
     pip install pyinstaller
     ```
2. **Bundle FFmpeg**:

   * Download a static Windows build of FFmpeg (e.g., from Gyan Dev).
   * Place `ffmpeg.exe` in the project root.
3. **Build with PyInstaller**:

   ```bat
   pyinstaller --windowed --onefile \
     --icon=icon.ico \
     --add-data "ffmpeg.exe;." \
     --add-data "config.json;." \
     ffmpeg_streamer_tk.py
   ```

   * Output: `dist\ffmpeg_streamer_tk.exe`
4. **(Optional) Create Windows Installer via Inno Setup**:

   * Install [Inno Setup](https://jrsoftware.org/isinfo.php).
   * Use the provided `installer.iss` script to build an `.exe` installer.

### macOS (.app)

1. **Prepare icon**:

   * Convert your PNG to `.icns` using `iconutil` (see `scripts/iconutil.sh`).
2. **Bundle FFmpeg**:

   ```bash
   cp $(which ffmpeg) ./ffmpeg && chmod +x ffmpeg
   ```
3. **Build with PyInstaller**:

   ```bash
   pyinstaller --windowed --onedir \
     --icon=icon.icns \
     --add-data "ffmpeg:." \
     ffmpeg_streamer_tk.py
   ```

   * Output: `dist/ffmpeg_streamer.app`

### Linux (Standalone & .desktop)

1. **Install build tools**:

   ```bash
   sudo apt update
   sudo apt install -y python3-tk python3-pip patchelf
   pip3 install pyinstaller
   ```
2. **Bundle FFmpeg**:

   ```bash
   cp $(which ffmpeg) ./ffmpeg && chmod +x ffmpeg
   ```
3. **Build standalone binary**:

   ```bash
   pyinstaller --onefile --add-data "ffmpeg:." ffmpeg_streamer_tk.py
   ```

   * Output: `dist/ffmpeg_streamer_tk`
4. **Create Desktop Entry**:

   * Copy binary to `~/.local/bin/ffmpeg-streamer`
   * Copy `icon.png` to `~/.local/share/icons/hicolor/256x256/apps/ffmpeg-streamer.png`
   * Place the provided `ffmpeg-streamer.desktop` in `~/.local/share/applications/`.
   * Update DB: `update-desktop-database ~/.local/share/applications`

### Raspberry Pi (ARM)

On the Pi itself:

```bash
sudo apt update
sudo apt install -y python3-tk python3-pip
pip3 install pyinstaller
# Download or build a static ARM ffmpeg binary
cp /path/to/arm-ffmpeg ./ffmpeg && chmod +x ffmpeg
pyinstaller --onefile --add-data "ffmpeg:." ffmpeg_streamer_tk.py
```

* Output: `dist/ffmpeg_streamer_tk` (ARM ELF binary)
* Use the same Linux `.desktop` steps above for launcher setup.

---

## ⚙️ Configuration File

**`config.json`** (auto-created) stores your last-used settings:

```json
{
  "service": "YouTube",
  "custom_url": "",
  "key": "YOUR_STREAM_KEY",
  "video": "/path/to/video.mp4",
  "loop": false,
  "resolution": "720p",
  "fps": 30,
  "bitrate": "2500k",
  "schedule": ""
}
```

---

## 📦 Packaging & Distribution

1. Commit the `dist/` artifacts to GitHub Releases.
2. Tag a version:

   ```bash
   git tag v1.0.0
   git push origin --tags
   ```
3. Create a Release and upload:

   * `ffmpeg_streamer_tk.exe` (Windows)
   * `ffmpeg_streamer.app.zip` (macOS)
   * `ffmpeg_streamer_linux.tar.gz` (Linux)

---

## 🐞 Troubleshooting

* **`ffmpeg not found` warning**: ensure bundling of `ffmpeg` or system path is correct.
* **Permission issues on macOS**: run `xattr -rd com.apple.quarantine dist/ffmpeg_streamer.app`.
* **Architecture errors**: build on the target platform (x86\_64 vs ARM).

---
