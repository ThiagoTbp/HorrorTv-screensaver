```markdown
# HorrorTV 📺👻

A Windows screensaver applying retro **xanalogtv** analog TV glitch and distortion effects over custom videos, music, and images. 

> **Note:** This is a streamlined repository containing only the source code and dependencies required to build `horrortv.scr` (stripped of all other XScreenSaver hacks).

---

## 📁 Repository Structure

| Path / File | Description |
| :--- | :--- |
| `CMakeLists.txt`, `build_msys2.sh` | Build scripts and configuration |
| `horrortv/` | Core screensaver source (`horrortv.c`, video, music, CAB handling, embedded images) |
| `screenhack/`, `scrnsave/`, `common/`, `include/`, `images/` | XScreenSaverWin base framework (includes multi-monitor support) |
| `cab_tools/` | Utilities to generate `horrortv.cab` |
| `screenhack_res.rc`, `XScreenSaver.ico` | Windows `.scr` resource files |

---

## 🛠️ Build Instructions

### Prerequisites
* MSYS2 installed at `C:\msys64` with `gcc`, `cmake`, and `make` under the **MinGW64** environment.

### Quick Build
Open an MSYS2 terminal (**UCRT64** or **MINGW64**) inside the project root folder and run:

```bash
bash build_msys2.sh

```

### Manual Step-by-Step Build

In your MSYS2 terminal:

```bash
export PATH=/c/msys64/mingw64/bin:$PATH
cmake -DCMAKE_BUILD_TYPE=Release -G "MinGW Makefiles" .
mingw32-make horrortv

```

Output binary: `build\horrortv.scr`

> ⚠️ **Important:** Always compile **inside** the root project folder directly rather than using a separate out-of-source build directory. The icon resource loader relies on a relative path to locate assets during build.

---

## 📦 Asset Package (`.CAB`)

The asset bundle (`horrortv.cab`) contains:

* **Videos:** `.a` files
* **Audio:** `.b` files (stored with **NO compression**)

If you need to rebuild the pre-packaged `.cab` bundle:

* **Option 1: Using Windows `makecab**`
Copy `cab_tools\horrortv.ddf` and `cab_tools\build_cab.bat` into the root directory containing your `Converted` and `Music` folders, then double-click `build_cab.bat`.
*(The `.ddf` explicitly sets `Compress=off`; compressed CAB files will not be read properly).*
* **Option 2: Using Python**
```bash
python cab_tools\make_cab.py <path_to_folder_with_Converted_and_Music> horrortv.cab

```



---

## 🚀 Usage & Installation

1. **Asset Placement:** Place `horrortv.cab` in the **same directory** as `horrortv.scr` (e.g., `build\`).
* If installing to `C:\Windows\System32\horrortv.scr`, place the CAB at:
`C:\ProgramData\HorrorTV\horrortv.cab`


2. **Testing:** Run `build\horrortv.scr /s` *(move the mouse or press any key to exit)*.
3. **Installation:** Right-click `horrortv.scr` in File Explorer ➔ Select **Install**.

---

## ⚡ First-Run Behavior

* On its initial run, the screensaver extracts `.cab` contents once to:
`%LOCALAPPDATA%\HorrorTV\c_xxxxxxxx_xxxxxxxx\` *(requires approx. 265 MB)*.
* Expect a brief black screen for a few seconds during the initial unpacking process. Subsequent launches will load instantly.
* If you replace `horrortv.cab` with an updated version, it will unpack into a new folder. You may safely delete old cached folders to free up disk space.
* If `horrortv.cab` is missing, the screensaver will fallback to running with embedded static images only (no audio).
* The small Windows Screen Saver Settings preview window will neither extract assets nor play media.

---

## ⚙️ How It Works

* **Visuals:** Each virtual "TV channel" in the effect displays a video (**80% chance**) or one of 11 embedded static images. Every time a channel changes, off-screen channels receive new content feeds.
* **Audio:** Music tracks play continuously in a seamless random loop.
* **Multi-Monitor Support:** Displays an independent instance per screen; audio playback is constrained exclusively to the primary monitor.

---

## 🔧 Configuration Options

Custom parameters can be tweaked in `horrortv/horrortv.c`:

```c
#define HORRORTV_VIDEO_PERCENT  80  // Probability percentage of a channel showing a video
#define HORRORTV_FILL           0   // Set to 1 to fill 4:3 screen (crops excess image edges)

```

---

## 📋 System Requirements

* **OS:** Windows 10 / Windows 11 with native H.264 video decoding support.
*(Note: Windows **"N"** editions require the installation of the official Microsoft **Media Feature Pack**).*
* Video files can carry any extension; the internal player explicitly registers and decodes them as standard MP4 containers.

```

<ElicitationsGroup message="To take this repository documentation further:">
  <Elicitation label="Add a visual architecture badge section and license text" query="Add standard GitHub badges (license, build, platform) and a MIT license section to the HorrorTV README."/>
  <Elicitation label="Draft a GitHub Actions workflow for automatic builds" query="Write a GitHub Actions CI workflow file (.github/workflows/build.yml) to automatically build horrortv.scr with MSYS2 on every push."/>
</ElicitationsGroup>

```
