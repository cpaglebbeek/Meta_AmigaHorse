---
date: 2026-05-31
repo: Meta_AmigaHorse
status: done
resume: ""
---

> Afgesloten 2026-05-31 bij v0.1.0-Worms bump (P-AMH-09 toegevoegd, ARCH herzien). Wireframes screens/ doorgeschoven naar v0.0.3 — zie `2026-05-31_v010_basic_principle.md`.

# Sessie 2026-05-31 — newp Meta_AmigaHorse (ecosysteem-master)

**Agent:** Claude Opus 4.7 (1M context)
**Repo:** Meta_AmigaHorse (`cpaglebbeek/Meta_AmigaHorse`)
**Branche:** main
**Cross-repo werk:** AmigaHorse_Core, AmigaHorse_Web, AmigaHorse_X86, AmigaHorse_Android, AmigaHorse_SteamDeck, Meta_Master
**Eindstand commits:** initial commit (hash volgt)

---

## Opdracht (samengevat)

Gebruiker startte `newp "AmigaHorse"` — open-source Amiga-emulator voor **Web + x86 + Android + Steamdeck SteamOS**, basis [WinUAE](https://github.com/tonioni/WinUAE). Na 1 WhatIf-ronde (6 vragen) gaf gebruiker akkoord op alle voorstellen MET correctie: **"webversie als 1e"**. Deze sessie zet skeleton op voor het ecosysteem (6 repos).

---

## Prompts en acties — chronologisch

### Prompt 1 — `newp "AmigaHorse"`
**Actie:** /verifyrules (compact) + Meta_Master pull (already up to date) + WhatIf met 6 open vragen (licentie/structuur/forks/codenamen/ROM/targets) + voorstellen per vraag + 6 skeleton-repos voorgesteld.
**Keuze:** Alle WhatIf-besluiten gebundeld in één antwoord.

### Prompt 2 — "jouw voorstellen maar webversie als 1e"
**Actie:** Alle 6 voorstellen overgenomen + Web krijgt prio 1 + Web krijgt topcodenaam (Turrican). LICENSE-verificatie WinUAE via shallow-clone → GPL-2.0-or-later geverifieerd in source-headers (cpummu.cpp ARAnyM-clausule) → AGPL-3.0 upgrade toegestaan. SteamDeckMSX-templates gelezen voor consistent skeleton-patroon. 6 directories aangemaakt. AGPL-3.0 LICENSE-tekst gekopieerd naar alle 6 repos.
**Keuze:** Skeleton-bouw gestart per repo.

---

## Beslissingen (alle definitief in v0.0.1)

| Vraag | Beslissing | Rationale |
|---|---|---|
| Licentie | AGPL-3.0 | Upstream GPL-2.0-or-later → AGPL-3.0 mag via "or later"-clausule; consistent met SteamDeckMSX |
| Repo-structuur | Hybride Y-pattern, 6 repos | Meta + Core (submodule) + 4 platform-varianten — beste balans tussen onderhoud en zichtbaarheid |
| Basis-fork | WinUAE referentie + FS-UAE praktische basis (definitief in v0.0.2) | WinUAE = master-source, FS-UAE = cross-platform-derivaat |
| Codenamen | Amiga-game-legendes (Turrican, Lemmings, …) | Past bij het platform; pool van 40+ titels |
| ROM-beleid | AROS-fallback + bring-your-own + géén binaries in git | Cloanto-IP-conflict (analoog C-BIOS-only bij MSX) |
| Targets | x86 = Linux+Win+macOS; SteamDeck apart; Web = A500+A1200 v1 | Geen iOS, geen Switch (P-AMH-08) |
| **Ontwikkelprioriteit** | **Web → X86 → Android → SteamDeck** | Door gebruiker bepaald in prompt 2 |

## Skeleton-inhoud Meta_AmigaHorse

- `README.md` — ecosysteem-overzicht + prio + status
- `CLAUDE.md` — codenaam-pool + per-file-license-review-clausule + Color-Coded protocol
- `ARCHITECTURE.md` — Core → 4 variant diagram + data-flow per target + cross-variant tabel
- `docs/PRINCIPLES.md` — P-AMH-01..08 (Upstream first, Gamepad+touch UI, Cycle-accurate, Save-state portability, No-Kickstart-in-repo, Steam Input passthrough, AGPL-vriendelijke deps + per-file-audit, Multi-platform by design)
- `docs/DEPENDENCIES.md` — cross-repo submodule-model + upstream + per-target + impactmatrix
- `docs/screens/README.md` — wireframe-plan placeholder
- `LICENSE` — AGPL-3.0
- `VERSION` — `0.0.1-Lemmings`
- `CHANGELOG.md` — v0.0.1-Lemmings entry
- `.gitignore`

## Open punten v0.0.2 (Meta)

- Wireframes screens/ (tool-keuze: Excalidraw/Figma/Penpot)
- 8 principes finetunen na review per variant
- UPSTREAM_AUDIT.md aanmaken bij eerste merge

## Trigger-zinnen voor /checkresume

- "verder met Meta_AmigaHorse v0.0.2 — wireframes screens/"
- "verder met AmigaHorse_Web v0.0.2 — Emscripten-port vs vAmigaWeb-integratie"
- "verder met AmigaHorse_Core v0.0.2 — WinUAE-direct vs FS-UAE fork-basis"
- "verder met AmigaHorse_X86 v0.0.2 — Qt6 vs GTK4 vs ImGui"
- "verder met AmigaHorse_Android v0.0.2 — uae4droid forken vs nieuwe NDK-port"
- "verder met AmigaHorse_SteamDeck v0.0.2 — Flatpak-strategie A/B/C"
