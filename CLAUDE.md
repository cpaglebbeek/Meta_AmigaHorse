# CLAUDE.md — Meta_AmigaHorse

> Master/orchestratie-repo voor het AmigaHorse-ecosysteem (6 repo's). Verwijst naar `Meta_Master/CLAUDE.md` voor alle globale regels (WhatIf, ZSH, sessie-protocol, over-en-uit).

## Rol van deze repo

Deze repo bevat **geen runnable code**. Alleen:
- Ecosysteem-overstijgende architectuur (`ARCHITECTURE.md`)
- Principes (`docs/PRINCIPLES.md`) — 8 stuks P-AMH-01..08
- Cross-repo afhankelijkheden (`docs/DEPENDENCIES.md`)
- Wireframes / schermontwerpen (`docs/screens/`)
- Sessie-MD's (`prompts/`)

Wijzigingen aan code-componenten gaan ALTIJD in de sub-repo (`AmigaHorse_Core`, `AmigaHorse_Web`, `AmigaHorse_X86`, `AmigaHorse_Android`, `AmigaHorse_SteamDeck`).

## Ontwikkelprioriteit (door gebruiker bepaald 2026-05-31)

1. **AmigaHorse_Web** — prio 1 (eerste runnable variant na skeleton-fase)
2. AmigaHorse_Core — geactiveerd zodra Web een gedeelde core nodig heeft
3. AmigaHorse_X86 — volgt na Web
4. AmigaHorse_Android — volgt na X86
5. AmigaHorse_SteamDeck — laatste (kan FS-UAE Flatpak inpakken)

## Codenaam-thema: Amiga-game-legendes

> **Single source of truth voor de hele ecosysteem.** Sub-repos verwijzen hierheen — niet zelf de pool dupliceren (drift-risico).

Elke release krijgt een unieke Amiga-titel als codenaam.

### Toegewezen (gebruikte titels)

| Repo | Versie | Codenaam |
|------|--------|----------|
| Meta_AmigaHorse | v0.0.1 | Lemmings |
| Meta_AmigaHorse | v0.1.0 | Worms |
| AmigaHorse_Core | v0.0.1 | Another World |
| AmigaHorse_Web | v0.0.1 | Turrican |
| AmigaHorse_Web | v0.0.2 | Cannon Fodder |
| AmigaHorse_X86 | v0.0.1 | Shadow of the Beast |
| AmigaHorse_Android | v0.0.1 | Pinball Dreams |
| AmigaHorse_SteamDeck | v0.0.1 | Sensible Soccer |

### Vrije pool (Amiga-game-legendes, willekeurige volgorde)

Speedball 2, Flashback, Project-X, Alien Breed, Super Frog, IK+, R-Type, Defender of the Crown, Populous, Syndicate, Frontier Elite II, Stunt Car Racer, Lotus Esprit Turbo, Xenon 2 Megablast, Gods, Magic Pockets, Chaos Engine, Apidya, Lionheart, Banshee, Body Blows, Brutal Sports Football, F/A-18 Interceptor, Hunter, Killing Game Show, Mega-lo-Mania, Microprose Soccer, Monkey Island, Beneath a Steel Sky, Settlers, Theme Park, Theme Hospital, UFO Enemy Unknown, Wing Commander, Zool, Walker, Agony, Disposable Hero.

### Rotatie-regels

1. Bij toewijzen van een nieuwe codenaam: schuif naar boven uit "Vrije pool", plaats in "Toegewezen"-tabel.
2. Geen codenaam mag in meer dan één repo voorkomen.
3. Sub-repo CLAUDE.md's verwijzen ALLEEN naar dit bestand voor de pool — geen dubbele administratie.

## Per-file-license-review (verplicht bij upstream-merge)

WinUAE en FS-UAE bevatten code uit meerdere bronnen (Bernd Schmidt, Ed Hanway, ARAnyM, coreutils, 65c02core, …). Sommige files zijn expliciet "GPL-2.0-or-later", andere hebben permissieve copyrights. Vóór elke merge van upstream-code:

1. Check per-file header voor licentie-clausule
2. Bij ontbreken: aannemen GPL-2.0 (origineel UAE), upgrade-pad naar AGPL-3.0 alleen via "or later"-clausule
3. Bij conflicting license (closed/permissive zonder upgrade-pad): isoleren in `vendor/` of vervangen
4. Auditspoor bijhouden in `docs/UPSTREAM_AUDIT.md` (aanmaken bij eerste upstream-merge)

## Feature & Bugfix Protocol (Color-Coded)

**Nieuwe Feature:**
- **Groen** — Documentatie-update zonder architectuurimpact → +0.0.1
- **Oranje** — Architectuurwijziging (nieuwe component, gewijzigde relatie) → +0.1.0
- **Rood** — Conceptuele heroriëntatie (bv. extra target, doctrinewissel) → +1.0.0

**Bugfix:**
- **Groen** — Tekstcorrectie
- **Geel** — Architectuurdiagram fout (impact: sub-repo's krijgen verkeerde aanname)
- **Rood** — Conceptueel principe blijkt onjuist → re-design

**Root Cause Analysis (verplicht bij elke bugfix):** Functioneel / Technisch / Architectonisch.

## Versioning Mandate

Elke functionele/documentaire wijziging → versie bumpen in `VERSION` (semver + codenaam: `0.0.1-Lemmings`).

## WhatIf Protocol

Zie `Meta_Master/CLAUDE.md` — verplicht vóór elke actie. Hier specifiek: wijziging in `ARCHITECTURE.md` raakt 5 sub-repo's; benoem impact per repo expliciet vóór commit.

## Sub-repo synchronisatie

Bij architectuurwijziging in deze repo: open issue/PR in elk geraakt sub-repo zodat hun `ARCHITECTURE.md` bijgewerkt wordt. Drift tussen master en sub is een **rode bug**.

## Build / Release

Deze repo bouwt niets. "Release" = tag op git met codenaam-versie. Geen artefacten.

## Sessie-MD's

Alle prompt-sessies in `prompts/YYYY-MM-DD_<slug>.md` met frontmatter (`date`, `repo`, `status`, `resume`). Zie `Meta_Master/templates/PROMPT_SESSION_TEMPLATE.md`.
