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
