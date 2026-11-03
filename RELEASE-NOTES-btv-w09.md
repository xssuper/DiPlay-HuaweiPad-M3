# DiPlay for Huawei MediaPad M3 (BTV-W09) — hi3650 debug build

Debug build of this fork for **Huawei MediaPad M3 8.4" (BTV-W09, WiFi edition)**:
HiSilicon hi3650, Android 7.0 (API 24), 2560×1600.

Base: upstream legacy project at `eb208e9` (v0.3.15) + the hi3650 MSVDX
software-decoder fix (this fork's purpose) + label fix.

## What's in this build

- **Green-screen fix**: `isKnownBrokenHardwareDecoder` scans the full AVC
  decoder list and routes `OMX.IMG.MSVDX.Decoder.AVC` (decodes at 0.5–1 s/frame
  on hi3650, watchdog resets it forever) to the Google software H.264 decoder.
- **App name fixed**: installs as `com.shihab.diplay`, displays as **"DiPlay"**
  (upstream debug builds show "DiPlay HUD Test" and install as `…hudtest`).
- MFi offline auth assets bundled (extracted from an official release APK),
  so CarPlay connects without the "cannot load CarPlay authentication" error.

## Install

1. If DiPlay is already installed, **uninstall it first** (debug signature
   differs from the official release signature).
2. Install `DiPlay-BTV-W09-hi3650-debug.apk` (package `com.shihab.diplay`).
3. Fresh install = empty app data: set resolution / frame rate in settings
   again (software decode caps at ≤50% resolution / 1280×800 @ 30fps on this
   device).
4. iPhone: Settings → General → CarPlay → forget the old head unit, re-pair.

## Verify

```
adb logcat | findstr "decoder"
# expect: Video: decoder=OMX.google.h264.decoder
adb shell dumpsys package com.shihab.diplay | findstr lastUpdateTime
```

## Notes

- Debug build (unsigned release key, debuggable, larger APK).
- SHA-256: `DiPlay-BTV-W09-hi3650-debug.apk.sha256` (same directory).
- To rebuild: `DIPLAY_AUTH_ASSETS_DIR=<dir with offline-mfi/>` +
  `./gradlew :mobile:assembleDebug`. See README "This fork" section.
