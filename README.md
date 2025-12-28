# win-mediakey-lolbin [![GitHub license](https://img.shields.io/badge/license-Public%20Domain-blue.svg)](https://unlicense.org/) [![Executable size](https://img.shields.io/badge/size-~4KB-brightgreen)]() [![.NET Framework](https://img.shields.io/badge/.NET-4.x-orange)]()

An ultra-lightweight Windows native utility (~4KB) to control media playback (Play/Pause, Next, Prev) from the command line.

---

## Context: LOLBin and MITRE ATT&CK

This project is also an educational proof of concept about **LOLBins** (Living Off the Land Binaries).

It is documented in MITRE ATT&CK as [T1027.004 "Compile After Delivery"](https://attack.mitre.org/techniques/T1027/004/), an evasion technique where code is delivered as plain text and compiled in-situ with `csc.exe` (the .NET Framework compiler pre-installed on Windows).

In this case, the usage is completely legitimate: a 4KB utility to control the music player.

---

## Why this exists

- No installation required
- Tiny executable (~4KB)
- No background process or tray icon
- Perfect for binding to mouse gestures, stream deck, or shortcuts
- Pure Windows native (no third-party dependencies)

---

## What it does

Compiles a single executable: `MediaKey.exe`

Supports these commands:

- `MediaKey.exe playpause` → Play/Pause
- `MediaKey.exe next` → Next track
- `MediaKey.exe prev` → Previous track

Works on players that respect media keys (Spotify, YouTube in browser, VLC, etc.).

---

## Requirements

- Windows 10/11
- .NET Framework 4.x (to have `csc.exe` available)
  - Typical path (x64):
    - `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe`
  - Typical path (x86):
    - `C:\Windows\Microsoft.NET\Framework\v4.0.30319\csc.exe`

---

## Compile

1) Clone/download the repo.
2) Run:

```bat
.\compile.bat
````

The script:

* detects `csc.exe` (Framework64 → Framework → PATH),
* creates `.\bin\`,
* compiles as **winexe** (without console window),
* and shows the size and usage.

Example output:

```text
Compiling bin\MediaKey.exe...

OK: Compilation successful.
Generated: ".\bin\MediaKey.exe" (4096 bytes)

Usage:
   MediaKey.exe playpause
   MediaKey.exe next
   MediaKey.exe prev
```

---

## Usage

```bat
.\bin\MediaKey.exe playpause
.\bin\MediaKey.exe next
.\bin\MediaKey.exe prev
```

Example for Logitech Options+/G Hub (Ring Actions):

* Action 1: `MediaKey.exe playpause`
* Action 2: `MediaKey.exe next`
* Action 3: `MediaKey.exe prev`

---

## How it works

The program uses P/Invoke to call `keybd_event` from `user32.dll`, simulating media key presses with these virtual key codes:

| Command     | Virtual Key Code | Constant               |
|-------------|------------------|------------------------|
| `playpause` | `0xB3`           | VK_MEDIA_PLAY_PAUSE    |
| `next`      | `0xB0`           | VK_MEDIA_NEXT_TRACK    |
| `prev`      | `0xB1`           | VK_MEDIA_PREV_TRACK    |

Small `Sleep()` calls ensure Windows processes the key event before the process exits.

---

## License

This project is dedicated to the public domain - [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).
No rights reserved.
