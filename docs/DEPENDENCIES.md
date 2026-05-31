# Dependencies — AmigaHorse ecosysteem

## Cross-repo (intern)

```
Meta_AmigaHorse  ──► (geen runtime-dep; docs only)
                     │
                     │ verwijst naar
                     ▼
AmigaHorse_Core  ◄───┬──── AmigaHorse_Web      (WASM-binding)
                     ├──── AmigaHorse_X86      (SDL2-binding)
                     ├──── AmigaHorse_Android  (JNI-binding)
                     └──── AmigaHorse_SteamDeck (X86-binary herverpakt OF eigen build)
```

**Submodule-strategie:** AmigaHorse_Core is git-submodule in elke platform-repo. Tag-gebaseerd (geen floating main).

## Upstream (extern)

| Upstream | Licentie | Rol | Repo |
|---|---|---|---|
| WinUAE | GPL-2.0-or-later (per-file mix) | Referentie-codebase + master-source van Amiga-emulatie | [tonioni/WinUAE](https://github.com/tonioni/WinUAE) |
| FS-UAE | GPL-2.0+ | Cross-platform WinUAE-derivaat (Linux/Mac/Win); voor X86/Android-basis | [FrodeSolheim/fs-uae](https://github.com/FrodeSolheim/fs-uae) |
| **vAmigaWeb** | **GPL-3.0** (geverifieerd 31-05) | **AmigaHorse_Web Core-basis (v0.0.2)** — submodule `external/vamigaweb`, pinned `c3c50d9` | [vAmigaWeb/vAmigaWeb](https://github.com/vAmigaWeb/vAmigaWeb) |
| vAmiga (in vAmigaWeb/Core/) | 3-tier: GPL-3.0+ (app) / MPL-2.0 (Core Emulator) / MIT (Moira CPU) | Vendored upstream van vAmigaWeb. Alle 3 lagen AGPL-3.0-compat. | [dirkwhoffmann/vAmiga](https://github.com/dirkwhoffmann/vAmiga) |
| AROS | APL (Apache-stijl) / GPL | Open-source Kickstart-replacement | [aros-development-team](https://github.com/aros-development-team) |
| Uae4droid / uae4arm-rpi | GPL-2.0+ | Bestaande Android-port (overweging) | midwan/uae4arm-rpi |
| SDL2 | zlib | Cross-platform input/audio/video abstractie | libsdl-org/SDL |
| Emscripten | MIT | C/C++ → WASM compiler (Web) | emscripten-core |

## Per-target deps (verwacht v0.0.2+)

### AmigaHorse_Web
- Emscripten ≥3.1.x
- IndexedDB (browser-native)
- Web Audio API + AudioWorklet (browser-native)
- WebGL 1.0 + Canvas2D fallback (browser-native)
- Gamepad API (browser-native)
- Vanilla JS of lichte framework (geen React/Vue tot bewezen need)

### AmigaHorse_X86
- C++17 + CMake ≥3.20
- SDL2 ≥2.28
- Qt6 of GTK4 of Dear ImGui (open keuze v0.0.2)
- libcurl (optional: WHDLoad-package-download)

### AmigaHorse_Android
- Android NDK r26+
- Kotlin 1.9 + Jetpack Compose
- AndroidX Gamepad
- Storage Access Framework
- AudioTrack

### AmigaHorse_SteamDeck
- Flatpak runtime (Freedesktop 23.08 of KDE/GNOME)
- Steam Input preset .vdf
- (Optie A) Eigen Flatpak met embedded Core
- (Optie B) Upstream FS-UAE Flatpak + Steam-Input-launcher
- (Optie C) AmigaHorse_X86 binary in Flatpak-wrapper

## Impactmatrix — wijzigingen in Core

Bij **breaking** wijziging in AmigaHorse_Core's Public API:

| Wijziging | Web | X86 | Android | SteamDeck |
|---|---|---|---|---|
| Save-state-formaat | ✗ ALL break (P-AMH-04 zegt: major bump) | ✗ | ✗ | ✗ |
| Kickstart-API | ✗ | ✗ | ✗ | ✗ |
| Input-API | ✗ | ✗ | ✗ | ✗ |
| Audio-sample-rate-API | ✗ Web | ✗ X86 | ✗ Android | ✗ SteamDeck |
| WASM-export-list | ✗ ONLY Web | ✓ | ✓ | ✓ |
| JNI-binding-signaturen | ✓ | ✓ | ✗ ONLY Android | ✓ |

## Niet-scope deps

- Closed-source Cloanto Kickstart-binaries (P-AMH-05)
- Steam SDK (P-AMH-06: gebruiken Steam Input via OS-niveau)
- Microsoft DirectInput/WASAPI (Windows-specifiek; we gebruiken SDL2-abstractie)
- iOS/iPadOS toolchain (P-AMH-08)
