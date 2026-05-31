# Meta_AmigaHorse

Master/orchestratie-repo voor het **AmigaHorse** ecosysteem: een open-source Amiga-emulator op vier targets, gebaseerd op de WinUAE-codebase (referentie) en FS-UAE/PUAE-derivaten (praktische cross-platform basis).

## Targets

In volgorde van ontwikkelprioriteit (door gebruiker bepaald 2026-05-31):

1. **Web** — WebAssembly, browser-based, geen install
2. **x86** — Native Linux + Windows + macOS desktop
3. **Android** — NDK-build, Compose-UI, gamepad+touch
4. **Steam Deck** — SteamOS Flatpak, Steam Input preset

## Ecosysteem (6 repos, allemaal PUBLIC, AGPL-3.0, branch `main`)

| Repo | Rol | Codenaam v0.0.1 |
|------|-----|------------------|
| **Meta_AmigaHorse** | Master/docs (deze repo) | Lemmings |
| [AmigaHorse_Core](https://github.com/cpaglebbeek/AmigaHorse_Core) | Gedeelde emulator-core (WinUAE/FS-UAE fork + abstractie-laag) | Another World |
| [AmigaHorse_Web](https://github.com/cpaglebbeek/AmigaHorse_Web) | WASM-variant (Emscripten + web-UI) — **prio 1** | Turrican |
| [AmigaHorse_X86](https://github.com/cpaglebbeek/AmigaHorse_X86) | Native desktop Linux/Win/macOS | Shadow of the Beast |
| [AmigaHorse_Android](https://github.com/cpaglebbeek/AmigaHorse_Android) | Android NDK + Compose | Pinball Dreams |
| [AmigaHorse_SteamDeck](https://github.com/cpaglebbeek/AmigaHorse_SteamDeck) | SteamOS Flatpak | Sensible Soccer |

## Codenaam-thema

**Amiga-game-legendes** — klassieke Amiga-titels die het platform definieerden (Psygnosis, Bitmap Brothers, Team17, Factor 5, Sensible Software, DMA Design, Delphine, Reflections). Single source of truth voor de pool: zie `CLAUDE.md` in deze repo.

## Status

- **Fase:** v0.0.1 skeleton (newp 2026-05-31)
- **Upstream:** [WinUAE](https://github.com/tonioni/WinUAE) (GPL-2.0+, referentie) + waarschijnlijk [FS-UAE](https://github.com/FrodeSolheim/fs-uae) (GPL-2.0+, cross-platform basis) — keuze in v0.0.2
- **ROM-beleid:** Géén Kickstart-binaries in git. AROS-fallback + bring-your-own (Cloanto Amiga Forever, eigen rip).

## Licentie

AGPL-3.0 — zie [LICENSE](LICENSE). Upstream WinUAE/FS-UAE = GPL-2.0-or-later → upgrade naar AGPL-3.0 toegestaan onder de "or later"-clausule. Per-file-license-review verplicht bij iedere upstream-merge (zie [CLAUDE.md](CLAUDE.md)).

## Documentatie

- [ARCHITECTURE.md](ARCHITECTURE.md) — componenten + relaties + data-flow (Core → 4 platform-varianten)
- [docs/PRINCIPLES.md](docs/PRINCIPLES.md) — 8 ontwerpprincipes (P-AMH-01..08)
- [docs/DEPENDENCIES.md](docs/DEPENDENCIES.md) — externe afhankelijkheden + cross-repo matrix
- [docs/screens/](docs/screens/) — wireframes per scherm (per variant)
- [CLAUDE.md](CLAUDE.md) — werkprotocol voor AI-agenten + codenaam-pool

## Relatie met andere ecosystemen

- **Gaming** (zusterproject [SteamDeckMSX](https://github.com/cpaglebbeek/Meta_SteamDeckMSX)) — Beide retro-emulatie op Deck. Geen code-overlap; mogelijke shared UI-patronen.
- **CloudInfra** — Géén HC55-poorten in v0.0.1. Web-variant kan later static-served worden vanaf HC55 of icthorse.nl.
- **ClaudeBug** — Standaard bug-rapportage-flow.
