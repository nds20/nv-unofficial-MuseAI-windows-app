Muse for Windows (unofficial)
Muse has apps for iOS, Android, and Mac — but no Windows desktop app.
This is a small passion project that fixes that: a standalone Windows
app that gives muse.ai a native home on a Windows PC.
Unofficial community project by Nicholas Smith, founder of 824.
Not affiliated with, endorsed by, or supported by Meta. "Muse" is a
product of Meta; this wrapper simply loads the public muse.ai website
in a desktop window, the way a browser would.
What it does
Opens muse.ai in its own window (1280×860) titled Muse
Sign-in sticks between launches (persistent session)
Single instance — launching it again focuses the existing window
Links that would leave Muse open in your default browser instead of
  hijacking the app; Meta sign-in flows stay in-app so login works
System tray icon with Show Muse / Quit
Native menu: Reload, zoom controls, fullscreen, and an About box
Football Dude app icon 🏈 (Nicholas's hand-drawn character)
Closing behavior: standard Windows — closing the window quits the app.
The tray icon is a convenience while the app runs, not a
minimize-to-tray daemon.
Install
Download MuseForWindows-Setup-1.0.0.msi from dist/.
Double-click it and follow the prompts. It installs per-machine to
   C:\Program Files\Muse for Windows and adds Start Menu and Desktop
   shortcuts. Uninstall any time from Settings → Apps.
SmartScreen warning is expected. The installer is unsigned (code
   signing certificates cost real money, and this is a passion project),
   so Windows will say "Windows protected your PC." Click
   More info → Run anyway to proceed.
Sign in with your normal Muse account. The app needs an internet
   connection — it is a window onto muse.ai, not an offline copy.
If the MSI ever misbehaves, MuseForWindows-1.0.0-portable.zip is the
same app unpackaged: extract it anywhere and run Muse.exe.
How it was built
Shell: Electron 45 (main.js, ~200
  lines) packaged with electron-builder (--win dir, win32 x64).
Installer: MSI authored with a generated WiX v3-schema file
  (Product.wxs, produced by scripts/generate-wxs.py from the
  packaged app folder) and compiled with wixl from msitools.
  The build machine is a Linux VM, so the whole pipeline — including
  the MSI — was built and verified without a Windows machine. The MSI
  was verified structurally (tables, shortcuts, file list) and by
  round-trip extraction: all 72 payload files extract byte-identical.
  The final double-click install test happens on a real Windows PC.
Installer GUIDs (keep these stable across versions)
Item
GUID
UpgradeCode (permanent — never change)
451BD98F-4218-4D13-8F57-480A3DA4E402
ProductCode for v1.0.0 (new GUID per version)
EBF471BF-CFB2-4E72-B91A-1D24D2606101
The MSI uses MajorUpgrade: installing a newer version replaces the old
one, and installing an older version over a newer one is blocked.
Rebuild
npm install                      # Electron + electron-builder
npm run dist:dir                 # -> release/win-unpacked/Muse.exe
python3 scripts/generate-wxs.py  # -> Product.wxs (file list from payload)
wixl -a x64 -o dist/MuseForWindows-Setup-1.0.0.msi Product.wxs
msiextract -l dist/MuseForWindows-Setup-1.0.0.msi   # verify contents
For the next version: bump version in package.json, generate a new
ProductCode, keep the same UpgradeCode, rebuild.
Honest limitations
Not tested on real Windows yet. It was built and verified on a
  Linux VM; the first true install test is on Nicholas's PC. The
  portable ZIP is the fallback if the MSI has a surprise.
Unsigned. Expect the SmartScreen warning described above.
Needs internet and a Muse account. No offline mode — there is
  nothing to run offline; the app is a dedicated window for muse.ai.
Not a Meta product. If Meta ships an official Windows app, use
  theirs. This exists because, as of October 2026, they haven't.
Sign-in popups for Meta's auth domains are allowed in-app windows so
  OAuth can complete; other external links open in your browser.
Credits
Built by Nicholas Smith (824) with his AI pair-programmer, Football
Dude 🏈 — which is also the icon.
