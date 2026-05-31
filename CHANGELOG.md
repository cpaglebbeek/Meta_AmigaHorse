# Changelog — Meta_AmigaHorse

Alle relevante wijzigingen worden hier vastgelegd. Format: [Keep a Changelog](https://keepachangelog.com/), versies volgen semver + codenaam (Amiga-game-legendes).

## [0.0.1-Lemmings] — 2026-05-31

### Added
- Eerste skeleton via `newp` ("AmigaHorse" — opensource Amiga-emulator voor Web/x86/Android/SteamOS, basis WinUAE)
- README.md met ecosysteem-overzicht + ontwikkelprioriteit (Web = prio 1)
- CLAUDE.md met codenaam-pool (Amiga-game-legendes) + per-file-license-review-clausule + werkprotocol
- ARCHITECTURE.md met Core → 4 platform-varianten + data-flow per target
- docs/PRINCIPLES.md met 8 principes P-AMH-01..08 (Upstream first, Gamepad+touch UI, Cycle-accurate, Save-state portability, No-Kickstart-in-repo, Steam Input passthrough, AGPL-vriendelijke deps, Multi-platform by design)
- docs/DEPENDENCIES.md met cross-repo matrix + externe deps
- docs/screens/ wireframe-placeholder
- LICENSE AGPL-3.0

### Decided (in newp WhatIf-rondes)
- **Licentie:** GPL-2.0-or-later van WinUAE/FS-UAE → AGPL-3.0 upgrade (consistent met SteamDeckMSX-precedent)
- **Repo-structuur:** hybride Y-pattern, 6 repos (1 Meta + 1 Core + 4 platform-varianten)
- **Basis:** WinUAE als referentie, FS-UAE als praktische cross-platform basis (definitieve keuze in v0.0.2)
- **Codenaam-thema:** Amiga-game-legendes
- **ROM-beleid:** AROS-fallback + bring-your-own + géén Kickstart-binaries in git
- **Targets:** x86 = Linux+Windows+macOS, SteamDeck = aparte repo (geen "Linux-subset"), Web = A500+A1200 v1 (CD32 later)
- **Ontwikkelprioriteit:** Web → X86 → Android → SteamDeck (per user 2026-05-31)
