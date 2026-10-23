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
- Clock/date may remain in the left-side block; top-right placement is not mandatory. Keep them compact and visually quiet, with the small calendar close to the clock/date. The clock/date must not dominate the wallpaper-led layout.
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
- **2026-10-23 — Clock/date layout refinement (user-approved):** left-side placement is acceptable; clock/date should be small/compact and visually quiet, with the small calendar nearby. Top-right placement is no longer mandatory.
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


### Follow-up audit — exact pinned launcher revision and locked prayer source (2026-10-23)
- Inspected the exact launcher submodule gitlink commit `9071ec8d2f41826778f6b4c7f5f42dadc22f5b22`, not just upstream `main`. The commit is real and includes `MainActivity.java`, `activity_main.xml`, `Config.java`, `config.xml`, settings, wallpaper code, and JVM unit tests.
- MainActivity is a large Java activity (~120 KB at the pinned revision) that programmatically wires app-grid/dock focus, wallpaper, greeting/clock, Wi-Fi/status and optional feature panels. It is a high-regression-risk integration point; keep changes narrow and add tests for logic extracted to small helpers.
- `activity_main.xml` is a FrameLayout root with multiple existing layers/regions; this is not a clean empty template. Need map all view IDs/constraints and programmatically added views before finalizing XML edits.
- Existing dock configuration has four default app package names, but `fill_dock=true` deliberately fills free dock slots with grid apps if a pinned app is absent or fewer than the columns are configured. Thus the four-slot visual requirement needs explicit behavior and tests, not just changing the list.
- Existing grid navigation and dock-slot tests are present (`GridNavigationTest.java`, `DockSlotsTest.java`); preserve their invariants and extend tests around any changed focus/slot behavior.
- MainActivity already has device clock validity checks and updates clock/greeting using system time. Avoid coupling prayer location to system time zone. Existing network-state permissions do not establish that the app has `INTERNET` permission; confirm manifest declarations before adding API calls.
- Existing settings activity has remote-friendly controls for columns, rows below dock, wallpaper, animation/effects, but no location selector. Wallpaper settings already exist; reuse the existing wallpaper selection and `WallpaperPlayer` pipeline.
- The source tree includes a dedicated `addons/prayer-times` module, but it is a separate full-screen app with Morocco defaults (method 21), not the requested integrated Indonesian widget. Do not transplant its default coordinates/method.
- Existing manifest has `ACCESS_FINE_LOCATION` but that alone does not guarantee location can be detected on a TV or that runtime permission is granted. The location flow must gracefully fall back to manual selection.
- **User-approved source choice overrides the earlier open research question:** use Aladhan API with `method=20` (KEMENAG) and `school=0` (Shafi/standard) for prayer calculations. Aladhan documentation confirms method 20's label and warns timings may differ from local mosque/government schedules. Do not describe these results as guaranteed exact copies of official Kemenag tables. Keep location, timezone, cache, timeout, and stale/error behavior explicit.
- Official-table alternatives are not being substituted behind the user's back. If future testing shows unacceptable discrepancies, report them and ask before changing the approved source.
- Research for the Quran verse/translation remains open; do not use invented text, translation, or references.

### Implementation remains blocked on finishing the audit, not on another user decision
1. Complete a view-ID and lifecycle map for the pinned MainActivity and full XML, including exact code paths that populate dock/grid and clock/date/status.
2. Inspect theme JSON overlay parsing and settings persistence interactions, plus existing Gradle/test setup.
3. Verify Quran source, Indonesian translation license/attribution, API availability, offline cache and daily rotation semantics.
4. Prepare a concrete file-by-file diff plan with test cases before source edits.
5. No source files have been edited. No build, tests, or APK packaging have been run.


### Quran data-source research update — 2026-10-23
- Kemenag's official Qur'an API provides Qur'anic text, Indonesian translation (2019 edition), and tafsir, but its published access process requires registration, a formal request, activation/token, and a follow-up survey before endpoint access. This is the preferred authority if access can be obtained, but do not assume credentials are available.
- The MyQuran API v3 documentation advertises Arabic text, Indonesian translation, and Kemenag short/long tafsir in its response schema and says its prayer schedule source is Kemenag. It is a potential integration candidate, but this audit has not yet verified production availability, terms/attribution, rate limits, or whether its translation content is appropriate for the requested daily verse widget.
- QuranEnc provides an Indonesian translation identified as the Ministry of Religious Affairs translation and documents a public JSON endpoint; its reuse terms require no modification and attribution/version details. This is a fallback candidate only after validating the precise translation version, Arabic text source, and attribution fit.
- No Quran API/provider is locked yet. Next step is to validate the candidate endpoint and response shape, confirm usage terms and cache behavior, then choose the lightest reliable source. Do not embed private API credentials in the launcher APK.

### Decision update — 2026-10-23 (user-approved data sources)
- Prayer times: use Aladhan Prayer Times API with `method=20` (KEMENAG) and `school=0` (standard/Shafi), while clearly describing it as the Kemenag calculation method rather than guaranteeing exact equality with every official local timetable. Use detected coordinates only when Android TV supports location and permission is granted; otherwise use a persistent manual city/location selector. Do not alter device time/time zone. Cache last valid schedule and show stale/unavailable state on failure; never invent values.
- Quran verse: user approved either QuranEnc or MyQuran. Prefer QuranEnc Indonesian Affairs/Kemenag translation if its endpoint, Arabic source, version, reuse terms, and attribution can be verified; otherwise use MyQuran API v3 after checking schema, availability, terms, and attribution. Show Arabic, Indonesian translation, surah/ayah reference, and required attribution/version. Never alter text or fabricate a verse. Deterministic daily selection; cache last valid result and visibly indicate stale/unavailable data.
- Data-source verification references: https://aladhan.com/prayer-times-api ; https://aladhan.com/calculation-methods ; https://quranenc.com/id/browse/indonesian_affairs ; https://quranenc.com/sw/home/api ; https://api.myquran.com/doc
- User confirmed there are no additional design decisions to discuss. Continue with implementation and verification without asking for non-blocking preferences. Any genuinely unavailable external credential, unsupported TV capability, or failed verification must be reported accurately and must not be worked around with fabricated data.
- Release gate unchanged: do not build/package an APK until the user explicitly says “FIX JADI”.

### Execution log — 2026-10-23
- Created launcher working branch `Demikians/cleanyourtv:feature/forest-green-home-widgets` from the user's `demikians-overdrive` branch.
- Implemented the first visual/configuration slice: `activity_main.xml` shows the CleanYourTV Forest Edition tagline and restrained emerald accents; default background is deep forest; cinematic backdrop and ambient glow default off; dock auto-fill defaults off.
- Theme preset `themes/overdrive/config.json` now uses fixed emerald `#8FBF9F`, 70% dock opacity, no extra network/input pills, and no app-library home entry. Four pinned favorite slots remain configured.
- Added hidden compatibility IDs to `layout-iw/activity_main.xml` for `brandTagline`, `miniCalendarTitle`, and `miniCalendarGrid`.
- Draft review: https://github.com/Demikians/cleanyourtv/pull/1
- The launcher fork already contains native `DemiPanel` code for Aladhan prayer times (method 20, school 0), Maghrib countdown, MyQuran Arabic/Indonesian verse, local cache/stale states, and a TV-friendly location/settings dialog. Presence in source is not proof of correct runtime behavior; location fallback, response parsing, cache correctness, and layout still require review and target-TV validation.
- Latest checks for commit `19a4abe8f4234f90eeabe22412d09476db6a379e`: workflow run `39988204398` (lists/tools/docs and launcher unit tests/lint) passed; workflow run `39988204414` (unit tests and lint) passed. Both workflows skipped APK assembly.
- Historical legacy CI run `39984424318` failed a 480 KB APK-size gate (482 KB) and documentation/resource checks. The current implementation workflow intentionally does not assemble an APK.
- Shell clone was unavailable due network/DNS constraints; GitHub connector is being used to inspect and update source files.
- Remaining work: review prayer/Quran API parsing and location behavior, verify visual layout and right-side panel constraints, reconcile any stale status notes, and validate on the actual Android TV. Theme remains draft until a genuine TV screenshot is captured; no screenshot will be fabricated.
- Release gate unchanged: no APK until explicit “FIX JADI”.

### Execution correction and verification — 2026-10-23 late
- Corrected earlier status wording: the selected launcher fork already contains a native `DemiPanel` implementation for prayer times, Maghrib countdown, MyQuran Arabic/Indonesian verse, local caching, stale/error handling, and a TV-friendly settings dialog. These features are present in source; runtime correctness and API behavior still require code-path review and validation on the target TV.
- Fixed the Hebrew resource contract by restoring the required `brandTagline`, `miniCalendarTitle`, and `miniCalendarGrid` view IDs in `layout-iw/activity_main.xml` as hidden compatibility views.
- Tightened `themes/overdrive/config.json`: `fill_dock=false`, fixed emerald accent, dock opacity reduced to 70, and extra network/input pills plus app-library home entry disabled for a quieter four-favorite home.
- Verified GitHub Actions for commit `db55bd6cb8b8fc73ae6756571f7b12d358983ad4`: source/docs tests passed; Android `testReleaseUnitTest` and `lintRelease` passed. Release APK assembly was skipped.
- A newer workflow is running for commit `19a4abe8f4234f90eeabe22412d09476db6a379e`; check its final result before treating the current config changes as fully verified.
- Previous legacy CI run produced a 482 KB release APK and failed its 480 KB size gate. That run used an older workflow; the current PR workflow does not assemble APKs. Do not build an APK before explicit `FIX JADI`.
- Remaining verification: review API parsing/location fallbacks and visual layout; test on target Android TV; capture a genuine TV screenshot before promoting the theme from draft. No fake screenshot will be created.


### Execution update — 2026-10-23 (continued)
- Added remote-friendly Indonesian city presets to the native DemiPanel location settings on launcher branch `feature/forest-green-home-widgets` (commit `4496d17d19fd3acf2f75bc4b2d50a76feb9369e7`). Presets include Jakarta, Bandung, Surabaya, Yogyakarta, Semarang, Surakarta, Medan, Makassar, Denpasar, Palembang, Balikpapan, Banjarmasin, Pontianak, Manado, Mataram, and Banda Aceh. Choosing a preset fills editable location name and coordinates; saving continues to persist a manual location without changing Android system time or time zone.
- Important limitation: automatic TV location still depends on the Android fine-location permission being granted and a TV location provider having a last-known fix. This code path does not request runtime permission. The city preset is a practical manual fallback, not proof that auto-location works on the target TV.
- The new commit has been saved to the working branch. No workflow run was returned for that commit at the time of inspection; therefore its CI status is **not yet verified**. Do not infer passing tests from earlier commits.
- No APK assembled, no PR merged. Release gate remains: build/package only after the user explicitly says `FIX JADI`.


### Execution update — 2026-10-23 (CI completion and compact clock)
- Confirmed CI for launcher commit `aff334593c232d1b87f88a59cd2070de283c720e`: both `Overdrive source validation` and `CI` completed successfully. Jobs passed: `Unit tests and lint (no APK)`, `Launcher unit tests and lint (no APK)`, and `Lists, tools and docs`. This is source-level automated validation only; it does not prove target-TV visual/runtime behavior.
- Verified the MyQuran v3 documentation describes `GET /quran/{surah}/{ayah}` with `status`, `data.surah_number`, `data.ayah_number`, `data.arab`, and `data.translation`, matching the current parser assumptions. This verifies documented schema, not live endpoint availability or translation licensing/attribution.
- User-approved clock/date refinement: left-side placement is acceptable; compactness and minimalism matter more than moving the clock to the right.
- Implemented a narrow layout change on launcher branch `feature/forest-green-home-widgets`, commit `d814a876fede775dd730b39a0088f4013f53b77a`: clock font 52sp→38sp, AM/PM 15sp→12sp, date 14sp→12sp, calendar title 11sp→10sp, and reduced related spacing. This has not yet had CI verification; check the new commit's workflow before treating it as validated.
- No APK built; no PR merge. Actual Android TV visual testing remains outstanding.
