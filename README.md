# Project Icewolf

A private, fast and debloated browser with its own frost-wolf look, built on the **LibreWolf** engine and packaged as one standalone `setup.exe`.

## Download
Get the newest version from the **Releases** page: the installer (`IcewolfSetup-<version>.exe`) or the portable version (`IcewolfPortable-<version>.zip`).

## Build it yourself
1. Extract the zip.
2. Double-click **Build-Icewolf-Setup.bat**. It needs no extra software.
3. The `dist` folder opens with two files:

| File | What it is |
|---|---|
| `IcewolfSetup-<version>.exe` | Installer |
| `IcewolfPortable-<version>.zip` | Portable |

If something fails, send `build-log.txt`.

The build downloads the newest signed LibreWolf Windows build (portable version) from LibreWolf's official servers and **stops unless**:
- its SHA256 matches LibreWolf's published checksum,
- no file has a broken or tampered signature, and
- `librewolf.exe`, `xul.dll` and every `.exe` are signed by LibreWolf's publisher (OSSign / Scheibling Consulting AB). A few helper libraries that LibreWolf ships unsigned are protected by the SHA256 checksum.

The setup checks every signature **again** on each PC before installing.

## Portable version
The build also creates **`IcewolfPortable-<version>.zip`**. Extract it anywhere (also on a USB stick) and start **`Icewolf.exe`**:
- **Nothing to install:** no administrator rights, everything (engine, profile, settings) stays inside the folder.
- **Same Icewolf:** logo, theme, debloat, DuckDuckGo, empty start page, uBlock Origin.
- **Updates:** after you close the browser, it checks for a new engine at most every 12 hours. Checksum and signatures are verified, just like the installed version.
- **The only trace on the PC:** one per-user registry value, so Windows shows the Icewolf logo on the taskbar.

## Updates
The setup installs the **Icewolf Updater**, a scheduled task that runs 3 minutes after login and every 4 hours:
- It checks LibreWolf's official release feed and downloads a newer engine.
- It verifies the checksum and all signatures before swapping in the engine, and re-applies the Icewolf settings.
- It only updates while Icewolf is closed. Results go to `C:\Program Files\Icewolf\update-log.txt`.
- It never brings back LibreWolf shortcuts or logos.

## What Icewolf removes
| Area | Status |
|---|---|
| Telemetry | Built out of the engine by LibreWolf, plus locked off by Icewolf (Glean, DAP, studies, crash reports, region lookup, attribution) |
| Mozilla account / Sync | Removed: disabled by policy, locked, all buttons and settings hidden |
| Password manager & autofill | Removed: disabled by policy, locked, hidden, old saved data deleted |
| AI | Removed: AI engine, chatbot, link previews, smart tab groups, PDF alt-text AI, AI history search, Google Lens |
| Start page & new tab | Completely empty, just the Icewolf logo. No search box, shortcuts, sponsors, news or ads. Search runs through the address bar with **DuckDuckGo** |
| Translation, "More tools", Help/About, default-browser prompt | Removed and hidden |
| Other bloat | Autostart at Windows login, Firefox View, Pocket, screenshots, shopping checker, Reader View, promos, jump lists, quick actions, push service, notification popups, profile switcher, backup tool |
| Branding | The browser calls itself **Icewolf** everywhere: window title, About, Settings, prompts, and Windows dialogs such as the location permission. Icewolf runs as its own `icewolf.exe`, a copy of the verified LibreWolf program with the Icewolf logo embedded, so the window, taskbar, Alt+Tab and Task Manager show Icewolf. Every logo image inside the engine (new tab, About, private window, icons) is replaced with the Icewolf logo, and the logo also appears in the installer and in the Windows Apps list. Mozilla, Firefox and LibreWolf links and sections are hidden in Settings |

Locks live in `%USERPROFILE%\.librewolf\librewolf.overrides.cfg` (LibreWolf's official override file) and in policies, so engine updates don't undo them.

## Import bookmarks from another browser
Chrome, Edge, Brave, Opera, Opera GX, Vivaldi, or a bookmarks HTML file:
- **Settings → General → Import browser data → Import data**, or
- the **"Import bookmarks…"** button on the empty bookmarks toolbar (shown on new tabs), or
- **Ctrl+Shift+O** (Library) → *Import and Backup*.

Passwords can't be imported, because the password manager is removed. Import them into your password manager instead.

## Privacy & security
- **Fingerprinting:** LibreWolf's strict "resist fingerprinting" is **off**, because it breaks dark mode and time zones. Firefox's fingerprinting protection is **on** instead.
- **Protection:** uBlock Origin (bundled by LibreWolf), with its malware and phishing lists. Strict tracking protection, HTTPS-only mode, encrypted DNS (Quad9) and no WebRTC IP leaks.
- **Hardening:** TLS 1.2+, strict certificate pinning, local certificate revocation checks (no OCSP), punycode shown, no JavaScript in PDFs, add-ons injected by other programs blocked.
- **The only connections left:** engine updates (librewolf.dev / dl.librewolf.net), uBlock filter lists, security blocklists, tracking-protection lists, and encrypted DNS. None of them sends browsing or usage data.

## Troubleshooting branding
If a LibreWolf name or logo still shows up somewhere, open **`branding-log.txt`** in `C:\Program Files\Icewolf` (installed version). It records which branding step worked and which didn't, so send that file.


## Good to know (LibreWolf defaults that are kept)
- **Logged out after closing:** cookies and history are deleted when the browser closes. To stay logged in on sites, go to *Settings → Privacy & Security* and untick deleting cookies on close.
- **WebGL is off** (privacy). A few 3D sites and games need it: set `webgl.disabled` to `false` in `user.js`.
- **DRM is off.** Netflix, Disney+ and Spotify need it: *Settings → General → "Play DRM-controlled content"*.
- **Existing LibreWolf install:** if you also use regular LibreWolf, it reads the same override file, so it gets Icewolf's settings too.
- **SmartScreen:** for the unsigned setup, click *More info → Run anyway*.
- **Pin to taskbar:** Icewolf doesn't pop up a pin prompt (the engine's prompt would use the engine's built-in name). To pin it, open the Start menu, right-click **Icewolf**, and choose **Pin to taskbar**. If you pinned Icewolf earlier, unpin it first. If the taskbar shows the wrong icon right after installing, restart Windows once (Windows caches taskbar icons).
- **Screen readers:** set `accessibility.force_disabled` to `0` in `user.js`.

## Credits & licenses
- **Project Icewolf** is licensed under the **Mozilla Public License 2.0** (see `LICENSE`).
- Browser engine: [LibreWolf](https://librewolf.net), based on Mozilla Firefox, licensed under the MPL 2.0. The release files contain LibreWolf's unmodified, signed program files (Icewolf adds a renamed copy with its own icon). LibreWolf's source code (including the Firefox code it is built from): <https://codeberg.org/librewolf>.
- Ad blocker: uBlock Origin (GPLv3), bundled by LibreWolf.
- Icewolf is an independent project, not affiliated with LibreWolf or Mozilla. Firefox is a trademark of the Mozilla Foundation.
