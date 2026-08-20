# Unify Kids Control

An Android app for managing internet access for your kids' devices through a **Unifi network controller** (UDM, UDM Pro, USG, CloudKey, etc.).

Group devices by child or category, then block or unblock their internet access with a single tap — no need to log into the Unifi web interface.

---

## Features

- **Device groups** — organise devices by child, room, or any label you choose
- **UniFi group sync** — groups are loaded directly from your UniFi controller's Client Groups; create, rename, and delete groups from the app
- **Local & UniFi groups** — create groups stored only on the device or backed by the UniFi controller (shown with a "UniFi" / "Local" badge)
- **One-tap block / unblock** — cut or restore internet access for an entire group at once
- **Per-device control** — block or unblock individual devices within a group independently
- **Live status** — real-time connection status synced from the controller
- **WLAN MAC filter** — manage wireless MAC allow/block lists directly from the app
- **WLAN blackout schedule** — define per-WLAN time windows when Wi-Fi is automatically switched off, with 30-minute granularity and a day × hour grid
- **Group images** — assign a photo from your gallery to each group for quick identification
- **Persistent storage** — groups and credentials are saved locally using Hive; no cloud account needed
- **Self-hosted controller** — connects directly to your own Unifi controller over your local network
- **SSL flexibility** — optional "Ignore SSL errors" toggle for self-signed certificates
- **Auto-update** — checks for new APK releases on startup and guides you through installation

---

## Requirements

- Android 5.0 (API 21) or higher
- A self-hosted Unifi controller reachable from your Android device (same network or VPN)
  - Supported: UniFi OS (UDM, UDM Pro, UDM SE), UCK Gen2, CloudKey, self-hosted UniFi Network Application
- A controller admin account with sufficient privileges to block/unblock clients

---

## Setup

1. Install the APK from the [Releases](../../releases) page.
2. Open the app and tap the **Settings** icon (top-right).
3. Enter your controller details:

   | Field | Example |
   |---|---|
   | Controller URL | `https://192.168.1.1` |
   | Port | `443` (UDM) or `8443` (UCK/self-hosted) |
   | Site ID | `default` |
   | Username | your Unifi admin username |
   | Password | your Unifi admin password |

4. Enable **Ignore SSL errors** if your controller uses a self-signed certificate.
5. Tap **Test Connection** to verify, then **Save**.
6. Back on the dashboard, tap **New Group** to create your first device group.
7. Add devices to the group by MAC address (or pick from the list of active clients).

---

## Usage

| Action | How |
|---|---|
| Block a group | Toggle the block switch on the group card |
| Unblock a group | Toggle the same switch |
| Block a single device | Open the group → tap the block icon next to the device |
| Add a device to a group | Open the group → tap the + button |
| Remove a device | Open the group → swipe or use the remove option on the device tile |
| Rename / edit a group | Open the group → tap the edit icon |
| Delete a group | Open the group → tap the delete icon |
| Manage WLAN filter | Tap the Wi-Fi icon in the dashboard toolbar |
| Manage WLAN schedule | Tap the clock icon in the dashboard toolbar |
| Set blackout hours | Select a WLAN → enable schedule → tap cells (red = off, light = on) |
| Toggle a full day/hour | Tap a day label (left) or an hour header (top) in the schedule grid |

---

## Releases

### v1.3.10 — 2026-08-16

**Fixed: Cannot create device groups on the UniFi network and black screen on local group creation**

- **Fixed v2 API path selection** — the app now correctly probes only valid v2 endpoints (`/groups` and `/profiles/client`) instead of incorrectly selecting `/network-members-groups`, which is not a valid group creation endpoint. This ensures group sync works on both newer and older UniFi controllers
- **Fixed black screen on local group creation** — when creating a local group (without the "Save to UniFi" checkbox), the dialog now properly awaits the async group creation before closing. This prevents a double-pop scenario with a stale context that could cause a crash or black screen
- **Improved error handling** — all async operations in the group creation dialog are now properly awaited and exceptions are caught, ensuring the UI remains responsive and errors are displayed to the user

### v1.3.9 — 2026-08-16

**Fixed: Build error in release mode (`kDebugMode` not defined)**

- **Removed `kDebugMode` check** — the conditional debug logging that caused release build failures has been replaced with unconditional `print()` statements, which Flutter automatically strips in release builds anyway

### v1.3.7 — 2026-08-16

**Fixed: Group sync not working when "Save to UniFi Client Group" is enabled**

- **Replaced silent error swallowing with user feedback** — when creating a group, errors from the UniFi controller are now displayed to the user instead of being silently ignored; this helps diagnose connection, authentication, or permission issues
- **Improved v2 API detection** — empty group lists (no groups exist yet) are now properly handled so v2 endpoints are used even on fresh installs
- **Automatic v2-to-v1 fallback** — if v2 API group creation fails, the app automatically falls back to the v1 API for compatibility with older controller versions
- **Clear error messages** — users now see exactly why group sync failed (network error, auth error, permission denied, etc.)

### v1.3.6 — 2026-06-08

- **Fixed CI build failure (JVM target mismatch + toolchain upgrades)** — upgraded AGP from 8.9.1 to 8.11.1, Gradle wrapper from 8.11.1 to 8.14.1, and aligned Java compiler target to VERSION_21 to match Flutter's Built-in Kotlin JVM target when running on JDK 21

### v1.3.5 — 2026-06-08

- **Fixed CI build failure** — upgraded Gradle wrapper from 8.9 to 8.11.1, which is the minimum required by AGP 8.9.1

### v1.3.4 — 2026-06-08

- **Fixed CI build failure (AGP 8.9.1 + Built-in Kotlin)** — upgraded AGP from 8.7.3 to 8.9.1 to satisfy transitive AndroidX dependencies (`activity:1.12.4`, `core:1.17.0`). Migrated to Flutter's Built-in Kotlin by removing the explicit `kotlin-android` plugin and `kotlinOptions` block; Flutter now manages Kotlin compilation internally, eliminating the KGP version warnings

### v1.3.3 — 2026-06-08

- **Fixed CI build failure (AGP / Gradle toolchain upgrade)** — upgraded Android Gradle Plugin from 8.3.2 to 8.7.3, Gradle wrapper from 8.4 to 8.9, Kotlin Gradle plugin from 1.9.24 to 2.0.21, and JDK in CI from 17 to 21. The previous pinning was too old for the Flutter composite build which brings AGP 9+ onto the classpath, causing the Flutter Gradle plugin to refuse the build. Removed the `resolutionStrategy.force()` workarounds that were only needed to stay on AGP 8.3.2

### v1.3.2 — 2026-06-08

- **Fixed CI build failure** — pinned Flutter to 3.44.1 in the release workflow and added `android.newDsl=false` to opt out of the AGP 9+ DSL mode that broke the Flutter Gradle plugin on newer stable releases

### v1.3.1 — 2026-06-08

Bug fixes and stability improvements identified by a full code review:

- **Fixed partial block status** — when some devices in a group were blocked and others were not, the group card showed "unknown" instead of "partial"; the correct "Partial" status chip is now displayed
- **Fixed description cannot be cleared** — editing a group and removing the description text had no effect; the description is now correctly cleared when the field is left empty
- **Fixed UniFi v2 group API errors** — if a v2 endpoint returned a 401/403 during the controller probe, an unhandled exception caused group sync to fail entirely; the probe now falls through gracefully to the next candidate path and to the v1 fallback
- **Fixed wrong REST paths for v2 group create/delete** — the path used for create and delete operations had a trailing character stripped, producing invalid API URLs; the correct collection path is now used for both operations
- **Fixed group edits failing when offline** — renaming or editing a group that is linked to UniFi would throw and skip the local save when the controller was unreachable; local changes now always persist regardless of the UniFi sync result
- **Fixed `wlan_filter_sheet` compile error** — `addGroupToWlanWhitelist` was called but never existed; the sheet now correctly merges group MACs into the WLAN filter using the repository directly
- **Fixed image picker `mounted` check** — `setState` after picking a group image had no `mounted` guard; this could crash if the dialog was closed while the image picker was open
- **Removed blocking I/O from `build()`** — `File.existsSync()` was called synchronously in every `GroupCard` rebuild; replaced with `Image.file` + `errorBuilder` which handles missing files without blocking the UI thread
- **Fixed MAC normalisation in WLAN filter sheet** — manually added MACs used `toLowerCase()` instead of `MacAddressUtils.normalise()`, which could store dash-separated MACs inconsistently
- **Fixed cross-midnight schedule overflow** — schedule entries spanning midnight would map slots past 23:59 back to 00:xx on the same day; iteration is now capped at the end of the day
- **Cached site detection** — the site-listing API call made on every group refresh is now cached per session, reducing redundant network requests
- **Replaced deprecated `withOpacity`** — all colour opacity uses updated to `withValues(alpha:)` to avoid precision loss
- **Removed unused `_slotIndex` function** from `wlan_schedule_page.dart`

### v1.3.0 — 2026-03-25

- **WLAN blackout schedule** — new clock icon in the dashboard toolbar opens a dedicated schedule page. Select any WLAN and draw blackout windows on a 7-day × 24-hour grid with 30-minute granularity. Red cells mark hours when the WLAN is automatically disabled. Tap a day label to toggle an entire day; tap an hour header to toggle that hour across all days. The schedule is saved to the UniFi controller via the `schedule_with_duration` API and is fully independent from the MAC filter page
- **Separate save flows** — MAC filter and schedule are now managed on separate pages, each with its own Save button, so changes to one cannot accidentally affect the other

### v1.2.2 — 2026-03-25

- **Source-aware group icons** — the icon on each group card now reflects the group's source: a router icon for UniFi-controlled groups and a computer icon for locally managed groups, replacing the generic Wi-Fi icon

### v1.2.1 — 2026-03-25

- **Fixed auto-update permission dialog** — the "Install unknown apps" permission window was never shown because the `REQUEST_INSTALL_PACKAGES` permission was missing from the Android manifest; this is now declared correctly so Android 8.0+ presents the system permission screen as expected
- **Install error feedback** — when the system rejects the install (e.g. permission not yet granted), the update dialog now shows a clear message with step-by-step instructions to grant the permission in Settings and retry
- **Visible install failures** — the result of the APK install intent is now checked and surfaced in the UI instead of being silently ignored

### v1.2.0 — 2026-03-25

- **UniFi Client Group sync** — groups are now loaded directly from the UniFi controller's modern Client Groups API (`network-members-groups`). Groups created in the UniFi UI (e.g. "Julian", "Tobias") appear automatically in the app after connecting
- **Create groups on the controller** — when creating a new group, choose "Save to UniFi" to create a matching Client Group on the controller; or leave it off for a local-only group
- **Local / UniFi badge** — each group card shows a "UniFi" or "Local" badge so you can tell at a glance whether it is synced to the controller
- **Add devices to groups** — adding a device to a UniFi-backed group now updates the device's group assignment on the controller via the user record API (the same approach used by the UniFi web UI)
- **Auto-login on credential change** — the app now automatically connects to the controller whenever credentials are saved in Settings, without requiring a manual pull-to-refresh
- **Sync error feedback** — pull-to-refresh now shows a red snackbar with the exact error message if the sync with the controller fails

### v1.1.0 — 2026-03-25

- Added **auto-update**: the app now checks for new releases on startup and guides you through downloading and installing the update
- Added **per-device block/unblock**: individual devices within a group can be toggled independently
- Improved live status sync from the Unifi controller

### v1.0.0 — Initial release

- Device groups with block/unblock support
- WLAN MAC filter management
- Persistent local storage with Hive
- SSL error bypass for self-signed certificates

---

## Installation from APK

Because this app is distributed outside the Play Store, Android requires you to allow installation from unknown sources:

1. Download the `.apk` file from the [Releases](../../releases) page.
2. Open the file — Android may ask you to grant **"Install unknown apps"** permission for your browser or file manager.
3. Grant the permission, then tap **Install**.

If you use the in-app auto-update, the system permission screen will appear automatically — grant it and tap **INSTALL** again in the app dialog. If the permission is denied, the dialog shows step-by-step guidance to enable it in Settings.

---

## Privacy

All data (credentials, device groups) is stored **only on your device**. The app communicates exclusively with your own Unifi controller. No data is sent to any external server.
