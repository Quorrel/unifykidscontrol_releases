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
