# KastKard Desktop — Releases

This repository hosts installers and auto-update assets for **KastKard Desktop**, the internal back-office app for inventory, sales, and hardware-driven order fulfilment.

> **Internal use only.** Do not share installers or the `.env` configuration file outside of the KastKard team.

---

## What KastKard Desktop does

KastKard Desktop is an all-in-one back-office tool that runs on Mac and Windows. It connects directly to hardware on the production floor and to KastKard's cloud services.

**Core features:**

- **Order management** — view and manage orders from a central dashboard
- **Inventory control** — add, update, and track stock levels in real time
- **8-step fulfilment pipeline** — a guided workflow that takes an order from received through to shipped:
  1. Order confirmation
  2. NFC tag programming (USB reader/writer)
  3. Niimbot USB thermal label printing
  4. XTool laser engraving (via Wi-Fi)
  5. Production asset generation (QR codes, PDF files)
  6. Network/thermal printing
  7. Packaging verification
  8. Shipping label creation
- **Hardware integration** — controls USB NFC readers, Niimbot thermal printers, XTool laser engravers, and network printers directly from the app
- **Role-based access** — `admin`, `user`, and `fulfillment_operator` roles with different permissions enforced across the UI and hardware controls
- **Auto-updates** — the app checks for new releases on every launch and every 4 hours, downloads updates silently in the background, and prompts you to restart when ready

---

## Installing

### First-time install

1. Go to the [Releases](../../releases) page of this repository
2. Download the installer for your operating system:
   - **macOS** — download the `.dmg` file matching your chip:
     - `KastKard Desktop-x.y.z-arm64.dmg` → Apple Silicon (M1/M2/M3/M4)
     - `KastKard Desktop-x.y.z-x64.dmg` → Intel Mac
   - **Windows** — download `KastKard Desktop Setup x.y.z.exe`
3. Run the installer and follow the prompts
4. **Before launching**, place the `.env` configuration file in the correct location (see below)

### macOS note
The app is signed and notarized with an Apple Developer ID. If macOS still shows a security prompt on first launch, go to **System Settings → Privacy & Security** and click **Open Anyway**.

### Windows note
Windows SmartScreen may show a one-time warning on first install ("Windows protected your PC"). Click **More info → Run anyway**. This only appears on the first install.

---

## Configuration — the `.env` file

The app needs a `.env` file to connect to KastKard's cloud services (Supabase auth, the API Worker, and the shipping platform). This file is **not included in the installer** — an admin must place it on each machine once.

### Where to put it

| Operating system | Path |
|---|---|
| macOS | `~/Library/Application Support/KastKard Desktop/.env` |
| Windows | `%APPDATA%\KastKard Desktop\.env` |

On macOS, `~/Library` is hidden by default. In Finder press **Cmd + Shift + G** and paste the path to navigate there.

On Windows, press **Win + R**, type `%APPDATA%\KastKard Desktop\` and press Enter.

### What goes in the `.env` file

Create a file named `.env` (no extension) with the following contents, filling in the real values provided by your admin:

```
# Supabase
VITE_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key

# KastKard API Worker
VITE_D1_API_BASE=https://kastkard-api.YOUR_SUBDOMAIN.workers.dev

# Main-process copies (required — do not remove)
SUPABASE_URL=https://YOUR_PROJECT.supabase.co
SUPABASE_ANON_KEY=your-anon-key

# Shipping platform
SHIPPING_API_BASE=https://api.your-shipping-platform.com
SHIPPING_API_KEY=your-shipping-key

# Local production asset output directory
PRODUCTION_ASSETS_DIR=~/KastKard/production-assets
```

The app will not connect to any KastKard services without this file. If you see a login error or a blank screen after launching, double-check the file is in the right place and the values are correct.

---

## Auto-updates

Once installed, **you never need to reinstall manually.** The app handles updates automatically:

- On every launch and every 4 hours, the app checks this repository for a newer release
- If a new version is available, it downloads silently in the background while you work
- When the download is complete, you'll see a prompt to restart the app — the update applies on restart
- If you dismiss the prompt, the update will apply the next time you quit and reopen the app

**macOS:** Auto-update requires the app to be signed. If you installed an older unsigned build, you will need to reinstall once from the latest release — auto-update will work from that point forward.

**Windows:** Auto-update works on all builds, including unsigned ones.

---

## Uninstalling

**macOS:** Drag `KastKard Desktop.app` from your Applications folder to the Trash.

**Windows:** Go to **Settings → Apps** and uninstall `KastKard Desktop`. Your `.env` file in `%APPDATA%\KastKard Desktop\` is preserved by default and does not need to be recreated on reinstall.

---

## Getting help

Contact your KastKard admin for credential issues, hardware setup, or role access questions.
