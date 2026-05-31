# ARCHITECTURE — AmigaHorse ecosysteem

> Eén functionele kern (Amiga-emulatie), vier deployment-targets, één gedeelde Core met dunne platform-laag erbovenop.

## Functionele kern (alle varianten)

**Doel:** Klassieke Amiga-software (OCS/ECS/AGA, ADF/HDF/WHDLoad, optioneel CD32/CDTV) speelbaar maken op moderne hardware met **gamepad-first** input (en touch waar relevant).

**Functionele componenten:**
- **Amiga-machine** — MC68000/68010/68020/68040/68060 CPU, Custom chips (Agnus/Denise/Paula/Lisa/Alice), Chip+Fast+Slow RAM, Kickstart-ROM
- **Software-laden** — ADF (floppy), HDF (harddisk), WHDLoad-bundels, IPF (SPS), LHA-archieven
- **Kickstart** — AROS open-source fallback (niet 100% compat), originele Kickstart via user-import (Cloanto Amiga Forever, eigen rip)
- **Save-states** — quicksave/quickload, slot-based, **portable formaat** als API-contract tussen alle 4 varianten (zie P-AMH-04)
- **Input-mapping** — gamepad → joystick-port 1/2 / CD32-pad, on-screen-keyboard, touch-overlay (Web/Android)
- **OSD** — pauze, menu, slot-selectie, disk-wisseling tijdens spel

## Componenten-overzicht

```
          ┌───────────────────────────────────────────────┐
          │                  AmigaHorse_Core              │
          │  (WinUAE/FS-UAE fork als git-submodule        │
          │   + abstractie-laag in C/C++)                 │
          │                                               │
          │  ┌─────────────────┐  ┌─────────────────────┐ │
          │  │  Emulator-core  │  │  Public API         │ │
          │  │  CPU, Custom,   │──┤  (C/C++ headers)    │ │
          │  │  Floppy, HDD    │  │  + WASM-binding     │ │
          │  └─────────────────┘  │  + JNI-binding      │ │
          │                       │  + SDL2-binding     │ │
          │                       └─────────────────────┘ │
          └─────────────┬─────────────────────────────────┘
                        │
       ┌────────────────┼────────────────┬────────────────┐
       │                │                │                │
       ▼                ▼                ▼                ▼
┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐
│ Web        │   │ X86        │   │ Android    │   │ SteamDeck  │
│ (WASM)     │   │ (Linux/    │   │ (NDK +     │   │ (Flatpak + │
│ + JS UI    │   │  Win/Mac)  │   │  Compose)  │   │  Steam     │
│ + canvas/  │   │ + Qt6 of   │   │ + gamepad+ │   │  Input)    │
│  WebGL     │   │  GTK4 UI   │   │  touch     │   │            │
│            │   │            │   │            │   │            │
│ prio 1     │   │ prio 2     │   │ prio 3     │   │ prio 4     │
└────────────┘   └────────────┘   └────────────┘   └────────────┘
```

## Variant 1 — Web (prio 1)

**Twee parallelle routes (v0.0.2-CannonFodder, conform P-AMH-09):**

```
┌────────────────────────────────────────────────────────────┐
│                       Browser                              │
│                                                            │
│   ┌────────────────────┐         ┌────────────────────┐    │
│   │  /  Quick BASIC    │         │  /full  Full mode  │    │
│   │  - Drag .bas       │         │  - Library         │    │
│   │  - Auto-RUN        │         │  - Settings        │    │
│   │  - Warm-snapshot   │         │  - Compat-set      │    │
│   └──────────┬─────────┘         └─────────┬──────────┘    │
│              │                              │              │
│              └──────────────┬───────────────┘              │
│                             │ wasm-bridge.js               │
│                             ▼                              │
│   ┌──────────────────────────────────────────────────┐     │
│   │  vAmiga-WASM (via vAmigaWeb-fork, v0.0.2-basis)  │     │
│   │  + AudioWorklet (audio)                          │     │
│   │  + WebGL/Canvas2D (framebuffer)                  │     │
│   │  + hostfs (Emscripten MEMFS → vAmiga DH1: mount) │     │
│   └──────────────────────────────────────────────────┘     │
│                             │                              │
│                             ▼                              │
│   ┌──────────────────────────────────────────────────┐     │
│   │  IndexedDB                                       │     │
│   │  - amigahorse-kickstart (user KS 1.3/2.05/3.1)   │     │
│   │  - amigahorse-disks (ADF/HDF)                    │     │
│   │  - amigahorse-states (save-states + warm-basic)  │     │
│   │  - amigahorse-config (per-route settings)        │     │
│   └──────────────────────────────────────────────────┘     │
└────────────────────────────────────────────────────────────┘
```

**Quick BASIC data-flow (P-AMH-09):**

1. Eenmalig: Asset-Setup-wizard (`/basic/setup`) — user uploadt KS 1.3 + WB 1.3 ADF + AmigaBASIC binary → IndexedDB
2. Eenmalig: warm-snapshot bouwen — emulator boot A500 + KS 1.3 + WB 1.3, AmigaBASIC start, save-state freeze → IndexedDB als `basic-env-snapshot`
3. Per `.bas`: drag-drop → schrijf naar MEMFS:`/dh1/launch.bas` → restore `basic-env-snapshot` → inject keyboard `LOAD "DH1:launch.bas"<CR>RUN<CR>` → klaar (~1-2 sec na cold-cache, ~500 ms warm)

**Core-keuze v0.0.2 (besloten):** [vAmigaWeb](https://github.com/dirkwhoffmann/vAmigaWeb) als basis. GPL-3.0 → AGPL-3.0 upgrade legaal. A500 + OCS sweet-spot past op KS 1.3 BASIC-doelen. Coherentie met AmigaHorse_Core (FS-UAE/WinUAE-derivaat) heroverwegen in v0.0.3.

**Distributie:** Static hosting met COOP+COEP-headers (SharedArrayBuffer-vereiste). Voorlopig icthorse.nl/AmigaHorse/ of HC55.

## Variant 2 — X86 (prio 2)

Native desktop voor Linux + Windows + macOS. Eigen UI-laag bovenop Core. **Open vraag v0.0.2:** Qt6 vs GTK4 vs Dear ImGui.

**Distributie:** AppImage (Linux), portable .exe (Windows), .app bundle (macOS). Geen installer in v0.x.

## Variant 3 — Android (prio 3)

Android 10+ (API 29+), arm64-v8a + x86_64. Kotlin/Compose UI + NDK-build van Core (JNI-bridge).
**Open vraag v0.0.2:** Bestaande uae4arm-rpi/Uae4droid forken vs nieuwe NDK-port van AmigaHorse_Core.

## Variant 4 — SteamDeck (prio 4)

SteamOS Flatpak. Kan in principe FS-UAE upstream-Flatpak inpakken met Steam Input preset + Deck-specifieke launcher. Of native AmigaHorse_X86-build met Deck-launcher.
**Open vraag v0.0.2:** Eigen Flatpak (full stack) vs upstream FS-UAE inpakken (snel) vs AmigaHorse_X86 binary herverpakken.

## Cross-variant gedeelde elementen

| Element | Web | X86 | Android | SteamDeck |
|---------|-----|-----|---------|-----------|
| Core (Emulator-engine) | WASM-build | Native | NDK-build | Native (X86-base) |
| Kickstart-handling | User upload (File-API → IndexedDB) | User-import | User-import (SAF) | User-import (Flatpak data-dir) |
| Cartridge/ADF-library | IndexedDB | `~/.local/share/AmigaHorse/` | App-private dir + SAF | `~/.var/app/.../data/` |
| Save-states | IndexedDB | Lokaal filesysteem | App-private dir | Lokaal Flatpak-dir |
| Input | Touch + USB-gamepad (Gamepad API) | SDL2 gamepad/keyboard/muis | Compose touch + Android Gamepad | Steam Input passthrough |
| Codenaam-thema | Amiga-game-legendes | idem | idem | idem |
| Licentie | AGPL-3.0 | AGPL-3.0 | AGPL-3.0 | AGPL-3.0 |

## Data-flow (generic)

1. User selecteert ADF/HDF → AmigaHorse_Core boot met Kickstart (user-supplied of AROS-fallback)
2. Input-driver (touch/gamepad/keyboard) → SDL2-equivalent of platform-native → Amiga joystick/keyboard
3. 50/60Hz render → display-target (canvas/WebGL/native window/Compose canvas/Flatpak window)
4. Audio → AudioWorklet (Web) of SDL2-audio (native) of AudioTrack (Android)
5. Quicksave → portable save-state-formaat naar platform-specifieke storage (zie tabel hierboven)

## Relatie met andere ecosystemen

- **Gaming / SteamDeckMSX** — Zusterproject. Geen code-overlap; mogelijke shared UI-patronen voor Steam-Deck-laag in v0.x.
- **CloudInfra** — Web-variant kan static-served worden vanaf HC55 of icthorse.nl. Géén poorten geclaimd in v0.0.1.
- **iCt_Horse** — Mogelijke publieke demo van AmigaHorse_Web onder icthorse.nl/AmigaHorse/ (uit scope v0.0.1).
- **ClaudeBug** — Standaard bug-rapportage-flow.

## Niet-scope (expliciet v0.0.1)

- iOS-variant (Apple-stack juridisch ingewikkeld, geen App Store voor emulators tot recent — beoordeel later)
- Switch/PS-port (closed platforms)
- Multiplayer/netplay (parallel-port-sync van originele Amiga's — out of scope tenzij user vraagt)
- Originele Kickstart-binaries in git (juridisch verboden, zie P-AMH-05)
- Distributie via Cloanto Amiga Forever ROMs (closed source, alleen user-upload-pad)
- WinUAE Windows-specifieke driver-stacks (DirectInput, WASAPI ultra-low-latency) — abstractie via SDL2 of platform-natief

## Open vraag — eerste v0.0.2-keuzes per variant

| Variant | Eerste v0.0.2-vraag |
|---|---|
| Core | WinUAE-direct-fork vs FS-UAE-fork als basis + per-file-license-audit-set opstellen |
| **Web (prio 1)** | Eigen Emscripten-port vs vAmigaWeb-integratie; A500-only of A500+A1200 v1 |
| X86 | FS-UAE-fork vs zelf bouwen + GUI-keuze (Qt6/GTK4/ImGui) |
| Android | uae4arm-rpi/Uae4droid forken vs nieuwe NDK-port + UI (Compose) |
| SteamDeck | Eigen Flatpak vs upstream FS-UAE inpakken vs AmigaHorse_X86 herverpakken |
| Meta | 8 principes P-AMH-01..08 finetune + wireframes screens/ |
