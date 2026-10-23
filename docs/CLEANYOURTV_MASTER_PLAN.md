# CleanYourTV — Master Plan & Locked Decisions

> **PRIORITY: VERY HIGH — source of truth for planning and implementation.**
>
> Read this document before any CleanYourTV work. Do not silently reinterpret, omit, or replace approved decisions. If implementation conflicts with this plan, stop and report the conflict before proceeding.
>
> **HARD RELEASE GATE: Do not build, package, or release an APK until the user explicitly says “FIX JADI”.** “FIX JADI” means all agreed implementation, discussion, documentation, review, and checks are complete. A request to plan, audit, design, or implement is not permission to build an APK.

## 1. Current scope

The current design discussion is focused on the **TV launcher/display**. Do not redesign the phone companion app UI or alter unrelated cleaning, ADB, restore, watchdog, or recovery behavior unless the launcher change demonstrably requires it and the user approves the scope change.

This file is a living specification. Update it when the user makes a new decision; retain earlier decisions unless the user explicitly changes them. Keep an audit trail in the decision log.

## 2. Approved visual direction

- Use the user's supplied TV-screen reference image as the primary design reference.
- Theme: **Forest Green — Natural & Fresh**, with a dark green base and restrained emerald accents.
- Premium, calm, minimal, clean, lightweight; avoid excessive glow, noisy decoration, or visually loud elements.
- The wallpaper must remain a visible focal point. Preserve a large, deliberately empty central area so the wallpaper can breathe.
- Previous project context also specifies a 16:9 Android TV layout, original DK logo at top-left, title **“DEMI KIANZ × NERO”**, subtitle **“CleanYourTV · Forest Edition”**, and selectable wallpaper from TV storage. Verify these against the latest source and existing agreed design during audit; do not casually discard them.
- Target device context previously discussed: 32-inch Android TV 11 with 1 GB RAM. Treat lightweight performance and low memory use as important constraints; confirm current target assumptions during audit if necessary.

## 3. Home-screen layout decisions

### Main / center
- No movie posters and no movie recommendation row. Remove the poster section entirely from the planned design.
- Leave the central area spacious for the wallpaper; do not fill it with substitute cards or widgets.
- Four favorite-app slots only.
- Place the four favorite apps **lower on the main screen**, leaving more visible wallpaper and negative space above them.
- Keep icons and spacing consistent and readable from a TV viewing distance.
- Preserve reliable Android TV remote / D-pad focus and app launching.

### Right-side widgets — fixed layout
The right-side widget column is fixed in position and must not drift, swap positions, overlap, or cover one another when content changes or refreshes. Reserve layout space for each widget and handle long text / missing data safely. On different supported screen sizes, adapt dimensions in a controlled way without widget collisions or changing the agreed order.

Widgets, in this order:
1. Prayer times.
2. Countdown to Maghrib.
3. Daily Quran verse (Ayat Harian), including translation and a clear reference when data is available.

Do not invent prayer times, Quran text, translations, or references. If data is unavailable, show a clear status instead of fabricated content.

### Clock and date
- Show clock and date in the agreed screen area (reference: top-right).
- Clock updates automatically from the TV/device system time.
- Date updates automatically.
- A prayer-location selection must **not silently change the device clock or system time zone**. Keep display time tied to system time unless a separate, explicitly approved time-setting feature is defined.

## 4. Location and prayer-time decision — locked

- Preferred location source: **automatic location detection from the device, if supported and permission is available**.
- If automatic detection is unsupported, permission is denied, or detection fails, provide a **manual city/location selector** as fallback.
- Prayer-time reference: **Kementerian Agama Republik Indonesia (Kemenag RI)**.
- Use verifiable Kemenag RI schedule data or a properly verified Kemenag-aligned source/method; do not present guesses as official data. During audit, verify the actual available data/API/source, usage constraints, and offline/failure behavior before implementation.
- Prayer schedule must match the selected/detected location.
- Maghrib countdown is calculated from the applicable local Maghrib schedule and refreshed appropriately.
- Persist the selected location so the user does not need to configure it repeatedly.
- Clearly communicate permission, connectivity, or data failures. Do not make unsupported location/API assumptions.

## 5. Information and performance

- Audit existing RAM, storage, CPU, Wi-Fi, and other system information before deciding exactly what to show.
- Display only metrics that can be retrieved reliably on the target TV. Never fake or imply precision for unavailable metrics.
- Keep performance information compact and subordinate to the wallpaper and right-side widgets.
- Avoid heavy animations, excessive background polling, unnecessary network work, and memory-heavy visual effects.
- Ensure remote navigation, app launching, launcher recovery, and existing compatibility safeguards continue to work.

## 6. Explicit non-goals / protected behavior

Unless the user approves a change after the audit, do not alter:
- Phone companion app UI or unrelated phone-app logic.
- ADB connection behavior.
- Cleaning / app-disable behavior and restore-point logic.
- Watchdog / launcher recovery behavior.
- Existing remote navigation and ability to open apps.
- Supported-device compatibility safeguards.

## 7. Required workflow — follow in order

1. **Read this master plan first** and compare it with the latest conversation decisions.
2. **Audit the actual TV launcher source**, including the upstream/submodule relationship. Identify the real UI entry points, theme, layout, wallpaper handling, widgets, settings, data sources, and navigation.
3. Write an audit report: current behavior, files involved, dependencies, risks, unknowns, and which decisions are already supported by code.
4. Produce a file-by-file implementation plan before editing code. Include purpose, changes, dependencies, risk, and verification for each file.
5. Reconcile any mismatch with this document. Ask the user only about genuinely blocking decisions; never silently choose against an approved decision.
6. Implement in small, reviewable steps limited to the agreed TV-display scope. Update this document when decisions change.
7. Test and document: 16:9 layout; large central wallpaper space; exactly four lower favorite apps; zero poster/recommendation rows; right-side widgets fixed with no overlap; clock/date; location permission and manual fallback; Kemenag-based prayer times; Maghrib countdown; verse and reference; remote/D-pad focus; app launching; performance on low-memory target; and protected recovery/restore behavior.
8. Review the diff and report exactly what changed, what passed, what remains, and any limitations. Do not claim tests that were not run.
9. Wait for the user's explicit **“FIX JADI”** approval.
10. **Only after “FIX JADI”**, and only when all agreed work and checks are complete, proceed to APK build. If any agreed item is unfinished, report it and do not build.

## 8. Definition of done before asking for “FIX JADI”

- Every agreed item is implemented or explicitly identified as blocked with user agreement.
- All changed files and design decisions are documented.
- Tests/checks have results recorded honestly.
- No unintended changes to protected phone-app, ADB, cleaning, restore, watchdog, or recovery behavior.
- TV screen remains minimal and wallpaper-led, with four lower favorites, no posters, and fixed non-overlapping right-side widgets.
- Location and prayer-time behavior uses verified Kemenag RI-aligned data with graceful fallback.
- Final diff reviewed; known issues disclosed.
- APK has **not** been built before explicit approval.

## 9. Decision log

- **2026-10-23 — Main visual reference:** user's supplied TV launcher screenshot is the primary design reference.
- **2026-10-23 — Posters:** remove movie posters and recommendations entirely.
- **2026-10-23 — Favorites:** exactly four favorite apps, positioned lower to preserve open wallpaper space in the center.
- **2026-10-23 — Right-side widgets:** fixed column; no movement, position swapping, or overlap. Contents: prayer times, countdown to Maghrib, daily Quran verse.
- **2026-10-23 — Location:** automatic device location when supported; manual city/location fallback when unavailable or permission/detection fails.
- **2026-10-23 — Prayer-time reference:** Kemenag RI; verify the actual data source/method before implementation.
- **2026-10-23 — Clock/date:** automatic from device system time; selecting prayer location must not silently alter system time/time zone.
- **2026-10-23 — Release gate:** no APK build until explicit user phrase “FIX JADI” after all agreed work is complete.

## 10. Change-control rule

Every new request is recorded here before implementation. Keep a short checklist of newly requested work, mark decisions as proposed or locked, and do not mark an item complete without evidence. When starting a later session, read this file and summarize the current state before making changes. This file is the persistent project reference; conversational memory alone is not sufficient.


## 11. Source audit — 2026-10-23 (read-only; implementation not started)

### Repository relationship
- App repository: `Demikians/cleanyourtv-app`.
- `.gitmodules` links `third_party/cleanyourtv` to upstream `https://github.com/Gathin23/cleanyourtv.git`.
- Audited upstream launcher paths directly on its `main` branch. The app repository's submodule checkout/revision must be verified before implementation so edits target the intended pinned source and are not accidentally lost on submodule update.
- The launcher source is a native Android Java app (not Jetpack Compose): `MainActivity.java`, XML layout/resources, `Config.java`, and a theme overlay system.

### Confirmed existing components
- `launcher/app/src/main/java/com/gathin/tvhome/MainActivity.java`: Home-screen orchestration, app grid/dock, focus behavior, clock-related state, network/status elements, and integration with wallpaper. Primary UI integration point; large file and therefore high regression risk.
- `launcher/app/src/main/res/layout/activity_main.xml`: root `FrameLayout`, sky/ambient layers, and pre-existing optional race panel. Must inspect the entire layout and MainActivity's programmatic view creation before choosing exact insertion points.
- `launcher/app/src/main/java/com/gathin/tvhome/Config.java` and `launcher/app/src/main/res/values/config.xml`: existing built-in settings and optional JSON theme overlay. Existing config has a default four-entry dock list, but `fill_dock=true` and the normal grid mean this does NOT yet guarantee exactly four favorites on screen.
- `launcher/app/src/main/java/com/gathin/tvhome/WallpaperPlayer.java`: local theme wallpaper collection, rotation, background decode executor, RGB_565 by default, and wallpaper dimming. This supports reusing the current wallpaper pipeline rather than introducing a second full-screen bitmap path.
- `launcher/app/src/main/java/com/gathin/tvhome/LauncherSettingsActivity.java` and `launcher/app/src/main/res/layout/activity_launcher_settings.xml`: remote-friendly launcher settings, including columns, rows below dock, dock opacity, animations/effects and wallpaper choice. Any new manual location selection needs an accessible, remote-friendly settings surface, but should not overload unrelated existing controls.
- `launcher/app/src/main/AndroidManifest.xml`: existing `ACCESS_FINE_LOCATION` permission is documented as used for Wi-Fi network-name display and granted through a setup script; this does NOT prove runtime location permission flow is correctly available for automatic location detection on all target TVs.
- `launcher/app/build.gradle`: Android application, compile/target SDK 34, min SDK 26, Java 17; currently lean dependency set (RecyclerView, dynamic animation, JUnit). New data/network code should avoid unnecessary heavy libraries.
- Existing system clock/screensaver code: `ClockActivity.java` and `ClockScene.java`; verify MainActivity's clock implementation separately before changing it.

### Prayer-time and Quran source findings
- Upstream has a separate `addons/prayer-times` app. Its README and code explicitly use the Aladhan API, with coordinates and method hard-coded in `addons/prayer-times/app/src/main/res/values/config.xml`; current default is Casablanca, Morocco, calculation method 21.
- That add-on is a full-screen standalone prayer-time app, not the fixed right-side launcher widget requested here. It reads today's times from Aladhan when opened and after midnight, refreshes its countdown each minute, and retains prior display when a fetch fails.
- The add-on does NOT currently implement the approved Indonesian Kemenag RI source requirement, automatic device location, manual persistent city selector, or the integrated fixed right-side widget. Do not reuse its output as if it were Kemenag-verified. It is reference material for basic fetch/countdown/failure patterns only.
- A recursive upstream tree search found no Quran/verse add-on by name. The daily Quran verse source, license/attribution, translation, and offline/failure behavior remain to be researched before implementation.
- No Kemenag API/source has been verified in this read-only audit. The source/method remains a blocking technical research item; do not implement prayer-time values from guesses.

### File-by-file implementation plan (proposed, pending completion of source/data audit)
1. `launcher/app/src/main/res/layout/activity_main.xml` — reserve explicit layout regions for top-left brand, top-right system clock/date, wallpaper-led center, lower four-slot favorite row, and fixed right widget column. Remove/hide any poster/recommendation UI if present after full-layout inspection. Verification: 16:9 bounds, no overlap at supported sizes, central negative space remains large.
2. `launcher/app/src/main/java/com/gathin/tvhome/MainActivity.java` — wire the approved view structure into existing home lifecycle; keep app launching, D-pad focus, settings/rebuild, wallpaper lifecycle, and recovery behavior. Ensure fixed widgets do not affect focus traversal unless intentionally selectable. High risk: test remote navigation and app launching on each change.
3. `launcher/app/src/main/java/com/gathin/tvhome/Config.java` and `launcher/app/src/main/res/values/config.xml` — only add configuration keys genuinely required for the agreed layout/theme/location behavior. Ensure defaults are safe and theme overlay parsing remains backward compatible.
4. `launcher/app/src/main/java/com/gathin/tvhome/WallpaperPlayer.java` — initially reuse without modifying; change only if audit proves wallpaper sizing/lifecycle needs an explicit adaptation. Verify no duplicate full-screen bitmap allocations and correct cleanup.
5. `launcher/app/src/main/java/com/gathin/tvhome/LauncherSettingsActivity.java` plus `launcher/app/src/main/res/layout/activity_launcher_settings.xml` — add a remote-friendly manual city/location selector only after the authoritative schedule source and city model are chosen; persist selection and provide an explicit auto-detect/manual fallback status. Avoid silently changing system time/time zone.
6. New small Java helper(s), names to be finalized after data-source verification — isolate location permission/fallback, Kemenag-aligned schedule retrieval/cache, Maghrib countdown, and Quran verse retrieval/cache. Keep network operations off the main thread, set timeouts, cache safely, and expose clear unavailable states. Avoid embedding invented endpoints or claiming an unverified API is official.
7. `launcher/app/src/main/AndroidManifest.xml` — add only the permissions actually needed after checking target Android TV runtime behavior. Verify installation and operation without granted location; manual selector must remain functional.
8. `launcher/app/src/test/java/...` — add unit tests for countdown boundary cases (including after Maghrib/next day), parsing/validation of schedule and verse responses, fallback/cache handling, and layout/config invariants where practical.
9. Theme/config sample and screenshots/docs — document Forest Green values and agreed layout; provide before/after reference and actual TV screenshots when hardware verification is available. Existing upstream theme screenshots are not evidence for the new design.

### Audit gaps before code changes
- Finish reading all of MainActivity and the full activity_main.xml, identify exact current clock/date and app-dock view construction, and inspect existing app sorting/dock fallback.
- Verify the app repository's exact submodule gitlink SHA and branch strategy; do not edit upstream `main` blindly if the project expects a fork/feature branch.
- Research and verify a usable Kemenag RI schedule source/method and the daily Quran verse source; establish attribution, network/cache policy, and city resolution before choosing APIs.
- Check existing tests and build scripts, then prepare an exact diff plan and dependency graph.
- No source code has been edited, no build/test has been run, and no APK has been built during this audit.


### Data-source research update — 2026-10-23
- SIHAT's developer page describes a Kemenag prayer-schedule API, but access requires an application, an approved token, and registered/allowlisted IP address; every displayed schedule must cite Kementerian Agama RI. Therefore direct SIHAT API integration is not plug-and-play and cannot be assumed available without credentials and approval. Reference: https://sihat.kemenag.dev/pengembang
- SIHAT's public schedule page offers location selection and states its calculation uses Kemenag's standard method. This is a useful official-facing reference for validating schedule semantics, but the current audit has not established a stable, permitted direct machine-readable endpoint suitable for this TV app. Reference: https://sihat.kemenag.dev/jadwal-salat
- A third-party API Indonesia product documents Kemenag-sourced prayer schedules for 514 regencies/cities, but it requires a user-owned API key and paid credits after a one-time signup bonus. Do not embed a private key in the public TV APK. It is not yet approved as the project data source. Reference: https://docs.apiindonesia.id/
- Quran Edge API documents Arabic text, Indonesian translations, and Tafsir Kemenag RI with no API key and an MIT-licensed API service. It appears a plausible candidate for verse content, but the endpoint/schema, source dataset provenance, usage stability, and attribution need a small verification test before adoption. Reference: https://quran.dyzulk.com/
- Decision: no endpoint or API provider is locked yet. Prioritize an approach that does not require shipping private credentials inside the APK; use a persistent cache and clear stale/unavailable status. The widget must not falsely label a third-party schedule as direct official data unless the data provenance and method are verified.

### Current audit status
- Confirmed source/UI mapping is documented; data source and location flow remain unresolved.
- No implementation changes have been made. No build or tests have been run.


### Locked source decision — 2026-10-23 (user approved)
- **Prayer-time calculation provider:** Aladhan Prayer Times API.
- **Calculation method:** `method=20` (Aladhan labels this KEMENAG — Kementerian Agama Republik Indonesia).
- **Asr school:** `school=0` (Shafi/standard), unless later validation demonstrates the target schedule requires a different setting and the user approves the change.
- Use the selected/detected location's coordinates for the request where available; do not rely on a hard-coded city or the upstream add-on's Casablanca defaults.
- Do not change the TV system clock/time zone when a prayer location is selected. Use the device's local time context consistently for countdown and date boundaries.
- Cache the last valid response and show a clear stale/unavailable state if refresh fails; never fabricate a schedule.
- Important limitation: method 20 identifies Aladhan's calculation method, not a guarantee that every returned time exactly matches the published local Kemenag table. Validate representative dates/locations during testing and disclose any discrepancies; do not claim exact official-table equivalence without evidence.
- Reference: Aladhan API docs list method 20 as KEMENAG and warn that computed timings may not always match local mosque/government authority schedules: https://aladhan.com/prayer-times-api and https://aladhan.com/calculation-methods


### Submodule pin verification — 2026-10-23
- The app repository's `third_party/cleanyourtv` gitlink currently pins upstream commit `9071ec8d2f41826778f6b4c7f5f42dadc22f5b22`.
- `.gitmodules` confirms the source URL is `https://github.com/Gathin23/cleanyourtv.git`.
- The prior UI inspection read upstream `main`; this is not yet proof that the pinned commit has identical contents. Before editing, compare the pinned commit against upstream `main` and target the intended project-owned fork/branch rather than pushing directly to upstream `main`.
- The app repository currently exposes only the `main` branch. No launcher source files have been edited.
