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

---

## Releases

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

If you use the in-app auto-update, the same permission prompt may appear — grant it and tap **Install** again in the dialog.

---

## Privacy

All data (credentials, device groups) is stored **only on your device**. The app communicates exclusively with your own Unifi controller. No data is sent to any external server.
