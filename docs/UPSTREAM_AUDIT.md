# Upstream Audit — AmigaHorse

> Verplicht per **P-AMH-07** (AGPL-vriendelijke deps + per-file-license-audit). Bijgehouden bij elke upstream-submodule-add of -update. Doel: bewijs dat elke vendored regel code AGPL-3.0-compat is.

## Audit-baseline 1 — vAmigaWeb (`external/vamigaweb`)

**Bron:** [vAmigaWeb/vAmigaWeb](https://github.com/vAmigaWeb/vAmigaWeb)
**Pinned commit:** `c3c50d9` (2026-05-28, "Merge pull request #332 from vAmigaWeb/v4")
**Add-datum:** 2026-05-31
**Repo waar gebruikt:** AmigaHorse_Web
**Project-licentie (LICENSE):** GPL-3.0 (geverifieerd, header `GNU GENERAL PUBLIC LICENSE Version 3`)
**Upgrade-pad:** GPL-3.0 → AGPL-3.0 toegestaan via "or later"-clausule (geverifieerd: GPL-3.0 sectie 14)

### Sub-licenties binnen vAmigaWeb

vAmigaWeb vendort vAmiga in `Core/`. vAmiga's eigen LICENSE-file beschrijft 3 lagen:

| Subcomponent | Pad in vAmigaWeb | Licentie | Audit-status |
|---|---|---|---|
| vAmiga application-laag | n.v.t. (alleen Mac-app, niet vendored) | GPL-3.0-or-later | n.v.t. |
| vAmiga Core Emulator | `external/vamigaweb/Core/` (CPU, Custom, Floppy, FileSystems, etc.) | MPL-2.0 | AGPL-3.0-compat via combined-work-clause |
| Moira (68k CPU emulator) | `external/vamigaweb/Core/Components/CPU/Moira/` (te verifieren bij eerste merge) | MIT | AGPL-3.0-compat (permissive) |
| vAmigaWeb glue-code | `external/vamigaweb/main.cpp`, `shell.html`, `js/`, etc. | GPL-3.0 (project-LICENSE) | AGPL-3.0-compat via "or later" |

**Conclusie:** Alle 4 lagen zijn AGPL-3.0-compatible. Strictest licentie is GPL-3.0 (vAmiga-app + glue), wat na "or later"-upgrade naar AGPL-3.0 mag.

### Te volgen bij volgende vAmigaWeb-bump

1. Diff `git log <old>..<new> --stat` op `external/vamigaweb` lopen na bump
2. Nieuwe files met onbekend header → per-file-check (zoek `License`, `Copyright`, `SPDX-License-Identifier`)
3. Bij gevonden conflict (GPL-2.0-only, closed-source, …) → blokkeer merge, raadpleeg `Meta_AmigaHorse/CLAUDE.md` per-file-review-clausule
4. Update deze tabel met `audit-datum / commit-range / bevindingen`

### Bekende risico's

- **vAmigaWeb gebruikt `localhost-key.pem` + `localhost.pem`** (self-signed certs voor lokale HTTPS-dev). Géén security-impact (alleen dev), wel licentie-vraag voor de cert-content (waarschijnlijk just dev-cert-data, geen IP). → Niet-blokkerend.
- **vAmigaWeb gebruikt `https.py`** (Python dev-server). Bij hergebruik in productie: check header.
- **Geen submodules in vAmigaWeb** (vAmiga vendored in `Core/`). Géén transitive submodule-risico's.

### Nog niet uitgevoerd (v0.0.2.x roadmap)

- Per-file-licentie-spider voor de eerste 50 source-files in `external/vamigaweb/Core/`
- Spot-check op SPDX-headers in `Components/` en `FileSystems/`
- Verifieer Moira-pad (`Core/Components/CPU/Moira/`) en MIT-LICENSE-file

---

## Audit-baseline 2..N — toekomstige upstreams

Plek voor:
- WinUAE / FS-UAE (bij Core-submodule-add in AmigaHorse_Core)
- uae4droid / uae4arm-rpi (bij Android-fork)
- AROS-Kickstart-replacement (bij eerste integratie)
- AMOS-runtime (bij v0.0.3+ BASIC-uitbreiding)

Format hierboven hergebruiken per add.
