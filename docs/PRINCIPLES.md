# Principes — AmigaHorse

Ontwerpprincipes die voor alle 6 repo's gelden. In Dragon1-structuur: **statement → rationale → mechanisme → implicaties**.

P-AMH-01 t/m 08 vastgesteld v0.0.1-Lemmings (2026-05-31). P-AMH-09 toegevoegd v0.1.0-Worms (2026-05-31, BASIC-modus-besluit op AmigaHorse_Web v0.0.2).

---

## P-AMH-01 — Upstream first

**Statement:** Alle wijzigingen aan emulatie-logica gaan eerst naar **upstream WinUAE en/of FS-UAE** als PR. Pas als upstream afwijst of niet reageert binnen 30 dagen, houden we een private patch in `AmigaHorse_Core/patches/`.

**Rationale:** WinUAE (Toni Wilen) en FS-UAE (Frode Solheim) zijn volwassen projecten met 25+ jaar geschiedenis. Forks die uit elkaar groeien sterven binnen 2 jaar. Door upstream-first blijft onze platform-laag dun.

**Mechanisme:** Per commit aan `external/winuae/` of `external/fs-uae/` evalueren: "kan dit naar upstream?". Zo ja → PR + wachten. Zo nee → patch + comment waarom upstream het niet wil (link naar PR-discussie).

**Implicaties:**
- Platform-specifieke UI / WASM-bindings / Steam-Input zaken komen in **onze laag**, niet in upstream
- Geen breaking-changes aan upstream Public API; alles backwards-compatible

---

## P-AMH-02 — Gamepad-first + touch waar relevant

**Statement:** De hele UI moet bedienbaar zijn met **alleen een gamepad** (SteamDeck/Android-met-controller) en met **alleen touch** (Web-mobile/Android). Toetsenbord/muis = nice-to-have op desktop, nooit vereist op handhelds.

**Rationale:** Amiga-games gebruikten 99% joystick. Een moderne emulator-UI die rond muis-menus is gebouwd, faalt op touchscreens en gamepads.

**Mechanisme:** Elk scherm krijgt focus-navigatie (D-pad + A/B). Touch-targets ≥44pt. Tekstinvoer via on-screen-keyboard of speech-to-text. Cartridge-search via filter-letters + scroll.

**Implicaties:**
- Geen modale dialogen die alleen muis ondersteunen
- Geen menus met meer dan ~7 items per niveau (D-pad-vermoeidheid)
- Touch-overlay voor in-game joystick op Web-mobile en Android

---

## P-AMH-03 — Cycle-accurate emulatie

**Statement:** We accepteren **geen** "fast-mode" shortcuts die game-timing breken. WinUAE is cycle-accurate (custom-chip-niveau); we breken die garantie niet.

**Rationale:** Veel Amiga-software (demos, copper-tricks, raster-effects, scrollers) is extreem timing-gevoelig. Demoscene-software valt om bij elk percentage drift. Een spelersbase die specifiek Amiga wil emuleren accepteert geen "ongeveer goed".

**Mechanisme:** Performance-bottlenecks lossen we op via betere CPU-utilisatie (multi-threading per subsysteem) of JIT (waar toegestaan per target), niet via timing-shortcuts.

**Implicaties:**
- Geen `--turbo`-flag in de UI (wel `--accuracy=low` voor zwakke targets, met duidelijke compat-waarschuwing)
- Web-WASM-target moet 50Hz halen op midrange laptops; als dat niet lukt, verlagen we visuele complexiteit (geen scanlines/CRT-shader by default), niet timing

---

## P-AMH-04 — Save-state portability als API-contract

**Statement:** Save-states gemaakt op één target (Web) MOETEN laadbaar zijn op een andere target (X86/Android/SteamDeck) met dezelfde versie van AmigaHorse_Core. Het save-state-formaat is een **API-contract** tussen alle varianten.

**Rationale:** Gebruikers willen kunnen beginnen in de browser tijdens lunch en doorspelen op de Deck thuis. Geen lock-in per platform.

**Mechanisme:** AmigaHorse_Core definieert het wire-formaat (versioned binary blob + JSON-metadata). Elke variant gebruikt dezelfde serializer. Breaking change in formaat → major version bump (+1.0.0).

**Implicaties:**
- Cloud-sync (later v0.x) is mogelijk zonder per-platform-translatie
- Forks die het formaat aanpassen → niet meer compat met mainline → afgeraden

---

## P-AMH-05 — Géén Kickstart-ROM in repo

**Statement:** De repo's bevatten **uitsluitend AROS-Kickstart-replacement** (open source) als fallback. Originele Amiga Kickstart-files importeert de gebruiker zelf.

**Rationale:** Originele Kickstart 1.x/2.x/3.x is auteursrechtelijk van Cloanto (huidige rechthebbende via Cloanto Amiga Forever). Distributie = illegaal in NL, US, EU, JP. AGPL-licentie zou hierdoor besmet raken (license-conflict, analoog aan SteamDeckMSX/P-SDM-05).

**Mechanisme:**
- AmigaHorse_Web: AROS via static include in WASM-bundle; user-Kickstart via File-API → IndexedDB
- AmigaHorse_X86/Android/SteamDeck: AROS bundled; user-Kickstart via OS-file-picker → app-data-dir
- Géén automatische Kickstart-downloads (anti-piraterij). Wel link naar legale bron (Cloanto Amiga Forever).

**Implicaties:**
- Compatibility-matrix in README per variant: welke games werken op AROS, welke vereisen originele Kickstart 1.3/2.05/3.1.
- Eerste-boot-flow per variant moet duidelijk maken: "AROS-mode actief" / "User-Kickstart geladen".

---

## P-AMH-06 — Steam Input passthrough (SteamDeck)

**Statement:** Op SteamDeck is Steam Input de **enige** input-pipeline. Geen directe `/dev/input/eventX` reads, geen SDL2 raw joystick voor Deck-gebruikers.

**Rationale:** Op Steam Deck rebindt Steam Input gamepad-events. Apps die rond Steam Input gaan, krijgen onverwacht gedrag bij user-binding-wissels.

**Mechanisme:** AmigaHorse_Core joystick → SDL2 → Steam Input wrapper. Steam Input preset `amiga-gamepad.vdf` bundled in `AmigaHorse_SteamDeck/`.

**Implicaties:**
- Custom-binding-UI in onze app is geen vervanger voor Steam Input-config — alleen aanvulling (preset-import).
- Op gewone Linux desktop (X86-variant) gebruiken we wél raw SDL2 gamepad (geen Steam-laag).

---

## P-AMH-07 — AGPL-vriendelijke afhankelijkheden + per-file-license-audit

**Statement:** Geen GPL-incompatibele dependencies. Geen closed-source libs in proces-space. Bij elke upstream-merge: per-file-license-review.

**Rationale:** AGPL-3.0 is strikt over linkbare libs. WinUAE/FS-UAE bevatten code uit meerdere bronnen (ARAnyM GPL-2.0+, coreutils GPL-3.0+, originele UAE GPL-2.0+, …); per-file-headers zijn de bron van waarheid.

**Mechanisme:**
- Audit-spoor in `Meta_AmigaHorse/docs/UPSTREAM_AUDIT.md` (aanmaken bij eerste upstream-merge)
- Bij ontbrekende per-file-header: aannemen GPL-2.0 (origineel UAE) → upgrade naar AGPL-3.0 alleen via "or later"-clausule
- Bij conflicting license zonder upgrade-pad: isoleren in `vendor/` of vervangen

**Implicaties:**
- Mogelijk geen Steam Achievement-integratie (vereist Steam SDK closed-source)
- Geen Cloanto Amiga Forever ROM-bundling (closed-source binaries)
- Web-variant: geen closed-source CDN libraries; alle JS-deps AGPL-/MIT-/BSD-compatible

---

## P-AMH-09 — BASIC as first-class use-case

**Statement:** AmigaBASIC (en in de toekomst AMOS/HiSoft/Blitz) is **geen feature** van de emulator maar een **eerste-klas use-case** met eigen UI-route, eigen flow en eigen architectuur. "Mijn `.bas` draaien" staat naast "Turrican spelen" als gelijkwaardig doel.

**Rationale:** AmigaBASIC werd letterlijk met elke Amiga 1000/500 meegeleverd op de Workbench 1.x-disk (Microsoft 1985-1989). Veel Amiga-gebruikers schreven hun eerste programma's in AmigaBASIC. Een Amiga-emulator zonder gestroomlijnde BASIC-flow negeert een hele gebruikersgroep. Bovendien onderscheidt deze flow AmigaHorse van upstream vAmigaWeb (dat alleen disk-based gaming-focus heeft).

**Mechanisme:**
- Twee parallelle routes in elke variant met UI (Web zeker, eventueel later X86/Android):
  - **Quick BASIC mode** (`/` op Web) — drop een `.bas`, ~3-5 sec tot RUN
  - **Full configurable** (`/full` op Web) — volledige emulator-ervaring
- BASIC-mode vereist eenmalige **Asset-Setup-wizard** (user-Kickstart 1.3 + WB 1.3 + AmigaBASIC binary → IndexedDB / app-data-dir)
- Per-target implementatie: warm-snapshot van Amiga-in-BASIC-prompt + hostfs-injection van `.bas` + simulated `LOAD/RUN` keyboard-input
- Auto-RUN default; toggle "stop in BASIC-prompt" voor LIST/edit-workflow

**Implicaties:**
- Save-state-formaat (P-AMH-04) wordt uitgebreid met `BasicEnvSnapshot`-subtype (warm-start)
- ROM-beleid (P-AMH-05) blijft strikt: AmigaBASIC binary = user-supplied, niet in git
- Per-variant ARCHITECTURE.md MOET de BASIC-route documenteren als die ondersteund wordt
- v0.0.x AmigaBASIC; AMOS in v0.0.3+; HiSoft/Blitz v0.x+
- AmigaBASIC werkt alleen op Kickstart 1.x (Commodore-bug op KS 2.0+); BASIC-mode lockt UI op A500/A1000 + KS 1.3

---

## P-AMH-08 — Multi-platform by design (in tegenstelling tot P-SDM-08)

**Statement:** AmigaHorse is **expliciet multi-platform** — Web + Linux + Windows + macOS + Android + SteamOS. iOS, Switch, Xbox/PlayStation = uit scope.

**Rationale:** Amiga-software wordt door een wereldwijde gemeenschap gespeeld op alle moderne form-factors. Single-platform (zoals SteamDeckMSX) zou de doelgroep onnodig beperken. WinUAE is Windows-only → wij vullen het gat voor Web/Linux/macOS/Android/Deck.

**Mechanisme:** AmigaHorse_Core heeft een platform-onafhankelijke Public API. Platform-laag (per repo) wrapt deze met platform-native UI/storage/input/audio. CMakePresets en build-targets per platform.

**Implicaties:**
- iOS-variant kan later toegevoegd (Apple App Store accepteert sinds 2024 emulators — beoordelen in v1.x)
- Console-ports (Switch homebrew/PS3-homebrew) door community welkom maar niet door ons onderhouden
- Per-platform-build-CI noodzakelijk (GitHub Actions matrix in v0.x)
