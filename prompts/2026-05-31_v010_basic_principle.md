---
date: 2026-05-31
repo: Meta_AmigaHorse
status: open
resume: "verder met Meta_AmigaHorse v0.0.3 — wireframes screens/ voor Quick BASIC route + Asset-Setup-wizard (4-step) + Full mode library, tool-keuze Excalidraw/Figma/Penpot"
---

# Sessie 2026-05-31 — Meta_AmigaHorse v0.1.0-Worms (P-AMH-09 + Web-route-arch)

**Versie-bump:** 0.0.1-Lemmings → 0.1.0-Worms (Oranje per CLAUDE.md — architectuurwijziging: nieuw principe + ARCH-update).
**Trigger:** AmigaHorse_Web v0.0.2 sessie heeft BASIC-modus geïntroduceerd → cross-cutting impact op Meta.

## Wijzigingen

- `docs/PRINCIPLES.md`: **P-AMH-09 toegevoegd** (BASIC as first-class use-case). Statement / Rationale / Mechanisme / Implicaties conform Dragon1-structuur.
- `ARCHITECTURE.md`: Variant 1 Web volledig herzien — twee parallelle routes + warm-snapshot data-flow + vAmigaWeb besluit + COOP+COEP-hosting-vereiste.
- `CLAUDE.md`: codenaam-pool-tabellen bijgewerkt (Meta v0.1.0-Worms toegewezen, Web v0.0.2-CannonFodder toegewezen, beide verwijderd uit "vrije pool"). Header van PRINCIPLES.md ook bijgewerkt met "P-AMH-09 toegevoegd v0.1.0-Worms".
- `VERSION` + `CHANGELOG.md` met Decided-blok.

## Open v0.0.3 (Meta)

1. Wireframes `docs/screens/` per UI-flow:
   - `web_01_route_picker.png` — landing met twee modi
   - `web_02_quick_basic_dropzone.png` — drop-state idle/active/error
   - `web_03_setup_wizard.png` — 4-step progress + asset-status
   - `web_04_full_library.png` — library + player layout
2. Tool-keuze: Excalidraw / Figma / Penpot (consistent met SteamDeckMSX-keuze)
3. UPSTREAM_AUDIT.md aanmaken zodra Web vAmigaWeb-submodule toevoegt (eerste per-file-license-audit)

## Trigger voor /checkresume

"verder met Meta_AmigaHorse v0.0.3 — wireframes screens/ Quick BASIC + Setup-wizard + Full library + tool-keuze"
