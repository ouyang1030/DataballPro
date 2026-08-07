# DataballPro

**Sport science video analysis for football — event annotation, broadcast-style telestration, and tracking data, in one desktop app.**

[![Latest release](https://img.shields.io/github/v/release/ouyang1030/DataballPro)](https://github.com/ouyang1030/DataballPro/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/ouyang1030/DataballPro/total)](https://github.com/ouyang1030/DataballPro/releases)
[![Documentation](https://img.shields.io/badge/docs-Help%20Center-blue)](https://ouyang1030.github.io/DataballPro/)

**English** · [简体中文](README.zh-CN.md) · [Deutsch](README.de.md)

> This repository distributes the released builds. The application source is developed in a private repository.

![DataballPro main dashboard](pics/main_dashboard.png)

---

## Download

Get the latest build from the [**Releases**](https://github.com/ouyang1030/DataballPro/releases/latest) page.

| Platform                  | File                                                                     | Requirements                                                                |
| ------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| **macOS** (Apple Silicon) | `DataballPro_<version>_aarch64.dmg`                                      | macOS 13 Ventura or later. M1/M2/M3/M4 only — Intel Macs are not supported. |
| **Windows** (x64)         | `DataballPro_<version>_x64-setup.exe` (installer) or `..._x64_en-US.msi` | Windows 10 or 11                                                            |
| **Linux** (x64)           | `DataballPro_<version>_amd64.AppImage`                                   | glibc 2.38 or newer (Ubuntu 24.04+, Debian 13+, Fedora 39+)                 |

### FFmpeg is required

DataballPro uses FFmpeg for video decoding, proxy generation, recording, and export. Install it before first use:

```bash
# macOS
brew install ffmpeg

# Windows (PowerShell)
winget install Gyan.FFmpeg

# Debian / Ubuntu
sudo apt install ffmpeg
```

The app searches the usual install locations and your `PATH`. If FFmpeg lives somewhere unusual, point the app at it with the `DATABALLPRO_FFMPEG` environment variable (set it to the full path of the binary).

If videos fail to load with a message about video dimensions, your FFmpeg install is usually the cause — reinstall it (`brew reinstall ffmpeg` on macOS) and restart the app.

---

## First launch

**macOS** — builds are ad-hoc signed and not notarized, so Gatekeeper will refuse to open the app on the first try. Either right-click the app and choose **Open**, or clear the quarantine flag:

```bash
xattr -dr com.apple.quarantine /Applications/DataballPro.app
```

**Windows** — SmartScreen may show "Windows protected your PC". Click **More info → Run anyway**.

**Linux** — make the AppImage executable, and install FUSE 2 if your distribution ships only FUSE 3:

```bash
chmod +x DataballPro_*_amd64.AppImage
sudo apt install libfuse2t64      # Ubuntu 24.04+; older releases use libfuse2
./DataballPro_*_amd64.AppImage
```

---

## What it does

### Video annotation

Frame-accurate seeking and stepping, playback from 0.25× to 2.0×, and a Code Window driven by JSON coding schemes (phases → subphases → events/formations). Tag events with one-key shortcuts while the match plays, then refine them on a drag-and-drop timeline.

### Telestration

Broadcast-style graphics drawn straight onto the clip: arrows, halos, linked rings, zones, formation shapes, man-marking links, vision cones, measurements, text, and timers. Effects can follow tracked players, freeze-frame holds insert a held segment into the timeline, and everything can be burned into an exported video.

### Live match capture

Connect a capture card, webcam, or network stream (RTSP/HTTP). The native FFmpeg backend records to `.mp4` while serving a low-latency preview, so you can tag events live instead of waiting for the finished file.

### Tracking data

Import processed CSV (Metrica, databallpy), Opta F24/F7 + 25 Hz TRACAB, and Opta Match XML + 10/25 Hz TGV bundles, with preflight and quality validation. Sync tracking time to video time using kickoff and other visual cues, then read the synchronized 2D tactical view with velocity trails, Voronoi space control, convex hulls, team centers, phase-aware formation detection, and a marking network. Velocity, acceleration, and distance covered are computed natively.

### Analysis

Player heatmaps (2D KDE with configurable bandwidth), sunburst and Sankey views of label distribution and event flow, event distribution on a schematic pitch, and inter-rater reliability (Cohen's κ, Fleiss' κ with bootstrap CIs, confusion matrix, temporal IoU). Fused datasets — video events, derived annotations, and tracking data — export to CSV/JSON for downstream work in Python or R.

### AI assistance

Player detection and tracking, and automatic pitch calibration. Everything happens on your own machine — no account, no upload, no network call. Telestration effects can then be bound to the tracked players.

### Interface

Available in English, 简体中文, Deutsch, Español, Français, 日本語, 한국어, and العربية (with RTL layout).

---

## Documentation

The [**Help Center**](https://ouyang1030.github.io/DataballPro/) covers the whole workflow:

- [Getting started](https://ouyang1030.github.io/DataballPro/fundamentals/getting-started/) — create a project from a video file or a live source
- [The workspace](https://ouyang1030.github.io/DataballPro/fundamentals/workspace/) and [video playback](https://ouyang1030.github.io/DataballPro/fundamentals/video-playback/)
- [Code Window](https://ouyang1030.github.io/DataballPro/annotation/code-window/), [annotating](https://ouyang1030.github.io/DataballPro/annotation/annotating/), [timeline editing](https://ouyang1030.github.io/DataballPro/annotation/timeline/)
- [Effect library](https://ouyang1030.github.io/DataballPro/telestration/effects/), [player tracking](https://ouyang1030.github.io/DataballPro/telestration/player-tracking/), [freeze frames](https://ouyang1030.github.io/DataballPro/telestration/freeze-frame/)
- [Importing tracking data](https://ouyang1030.github.io/DataballPro/tracking/importing/) and the [2D pitch panel](https://ouyang1030.github.io/DataballPro/tracking/pitch-panel/)
- [Analysis tools](https://ouyang1030.github.io/DataballPro/analysis/tools/) and [export](https://ouyang1030.github.io/DataballPro/sharing/export/)
- Reference: [keyboard shortcuts](https://ouyang1030.github.io/DataballPro/reference/keyboard-shortcuts/), [CSV format](https://ouyang1030.github.io/DataballPro/reference/csv-format/), [preferences](https://ouyang1030.github.io/DataballPro/reference/preferences/)

---

## Updates

DataballPro checks this repository's releases for updates and can install them in place, so you only need to download manually once.

## Your data

Projects are ordinary folders on your disk — a SQLite database of annotations plus configuration alongside your video. Nothing is uploaded: the app reaches the network only to check for updates and to read any stream URL you configure yourself.

## Reporting problems

Please open an [issue](https://github.com/ouyang1030/DataballPro/issues) with your OS and app version, what you did, and what happened. Attaching the log file helps a lot:

- macOS: `~/Library/Logs/com.databallpro.app/`
- Windows: `%APPDATA%\com.databallpro.app\logs\`
- Linux: `~/.local/share/com.databallpro.app/logs/`

## License

DataballPro is distributed under the [PolyForm Noncommercial License 1.0.0](LICENSE): free to use for research, teaching, personal projects, and any other noncommercial purpose, including by universities and public research organizations. Commercial use requires a separate licence — please open an issue to get in touch.

Bundled components, including the ONNX model weights, carry their own terms — see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
