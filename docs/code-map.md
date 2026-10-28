# ratchet-clank code map

> **Superseded by `ratchet-clank-pc`.** This repo is kept as it is (nothing deleted or archived); new reading and work go to ratchet-clank-pc, and its code map is the fuller one.

- **Repo:** iannitram/ratchet-clank (private, not a GitHub fork: a copy of btsouth/ratchet-clank-pc). **Kind:** C loader and runtime plus a C++ GPU library (CMake and Ninja, driven by `build.sh`), Python offline tools and a GTK4 launcher: a native Linux port of *Ratchet & Clank* (2016, PS4), built on the Bloodborne port (bbport).
- **Upstream:** btsouth/ratchet-clank-pc (`upstream/main`; the two share history up to commit `72bd988` and then diverged). **Integration branch:** `main`.
- **Describes commit:** `15472ca` (2026-10-27).

The game's original x86-64 executable runs directly on the CPU, a small runtime stands in for the PS4 system libraries, and a Vulkan renderer derived from shadPS4 draws the frame, with FSR and DLSS upscaling. No game files are in the repo.

## Layout

The source tree is the same as upstream's at its current tip, apart from the five files listed under "Where our changes live". Directory by directory:

| Path | What it owns |
|---|---|
| `src/` | Loader (`probe.c`) and HLE runtime, one `runtime_*.c` per area (kernel, memory, thread, fiber, file, audio, pad, savedata, net, and others); `import_names*.inc` |
| `scripts/` | Offline preparation (`prepare.py`, `link_libc.py`, `link_modules.py`, `patches.py`, `mods.py`, `game_check.py`, `content_profile.py`, `online.py`, `respell.py`) and `ratchet_save.py` (save viewer and unlocked-copy writer) |
| `gpu/` | Renderer library `libbbgpu.so`: `shadps4/` (vendored shadPS4 subset, `VENDOR.txt`), `shim/` (glue, menu, hooks), `cp/` (PM4 decoder), `layer/` (memory layer), `games/` (`ratchet.cpp`, `bloodborne.cpp` profiles), `bbnet/` (online module), `dlss_bridge/`, `tools/`, `patches/` (FSR-Vulkan patch), `third_party/` (`sirit`, `gcn`, `half`, `httplib`, `imgui`, `fonts`, `fsr-vulkan` submodule) |
| `launcher/` | GTK4 launchers: Ratchet (`ratchet-launcher.sh`, `ratchet_launcher.py`, `ratchet_settings.py`) and the inherited Bloodborne one (`bb-launcher.sh`, `bbport_*.py`) |
| `packaging/` | `appimage.sh`, `AppRun`, `relocate.py`, `bbport-entry.c`, `default.nix` |
| `patches/` | `Bloodborne.xml` (inherited community patches) |
| `tools/` | Developer tools: scripted runs, FSR 4 shader rewrites, `fsr4cap/`, `bridge_helper/`, `re/` (Python analysis helpers) |
| `tests/` | About 45 C, C++ and Python tests (runtime, pad, online, CP decoder, flip watchdog, shader metadata, upscaler pieces, Ratchet launcher and save, AppImage relocation) |
| `docs/` | Design notes and release `CHANGES_*.md`, mostly inherited from the Bloodborne port, partly in Russian |
| `third_party/LibAtrac9` | Submodule: ATRAC9 decoder |
| `build.sh`, `run.sh`, `shell.nix` | Build driver, run wrapper, Nix dev shell |
| `README.md`, `LICENSE` | Upstream's readme and license |
| `ROADMAP.md`, `.gitignore` | Ours: a placeholder roadmap with one note (port the PS4 FPU-state fix for guest threads, since done upstream); ignore rules for build output, game files, local logs and analysis leftovers |
| `AGENTS.fork.md`, `CHANGELOG.fork.md`, `PIPELINE.md` | Our rules for this copy, change history, stage record |

## Where our changes live

Against upstream's `main` tip the only differences are five files: `.gitignore`, `ROADMAP.md`, `AGENTS.fork.md`, `CHANGELOG.fork.md`, `PIPELINE.md`. Compared with `ratchet-clank-pc` the only differences are `.gitignore`, `ROADMAP.md` and the fork files. By commit count this copy is 12 ahead of and 18 behind upstream's `main`, but the file contents match: the same work landed here as different commits.

## Build and test

```sh
bash build.sh
bash build.sh --test
python3 -m unittest discover -s tests
BB_GAME_DIR=/path/to/CUSA01073 bash run.sh
```

Needs GCC, CMake, Ninja, Python 3, glslang, SDL3, Vulkan headers and the libraries in `shell.nix`; submodules are `gpu/third_party/fsr-vulkan`, `gpu/third_party/imgui`, `third_party/LibAtrac9`. No CI workflow.

## Mac / PC / Linux

Linux x86-64 with a Vulkan 1.3 GPU only (PIPELINE.md: platforms linux). It does not build on macOS or Windows; a Mac or Windows session can read and edit files but cannot build or run anything here (R04).
