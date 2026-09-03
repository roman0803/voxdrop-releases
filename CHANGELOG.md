# Changelog

## 0.9.8 – 2026-09-03

### Features
- **Automatische Updates über Sparkle.** VoxDrop prüft selbst auf neue Versionen, lädt sie, verifiziert die EdDSA-Signatur und installiert sie – kein manuelles DMG-Kopieren mehr. Abschaltbar unter Einstellungen → Info.
- **Verlauf der letzten 20 Transkriptionen** (Einstellungen → Verlauf). Gespeichert wird vor dem Einfügen, damit ein fehlgeschlagenes Einfügen den Text nicht mehr vernichtet. Im Popover gibt es „Letzten Text kopieren". Rein lokal, abschaltbar, löschbar.
- **Whisper-Vokabular:** Die Ziel-Schreibweisen aus dem Wörterbuch werden der Transkription als `prompt` mitgegeben und wirken damit auf die Erkennung selbst statt erst auf die Ersetzung danach. Dazu ein freies Vokabular-Feld für Begriffe ohne Ersetzung.

### Changes
- Der eigene GitHub-API-Update-Check wurde durch Sparkle ersetzt.

## 0.9.7 – 2026-09-03

### Fixes
- **Hotkeys konnten stillschweigend aufhören zu funktionieren.** macOS deaktiviert Event-Taps bei `tapDisabledByTimeout` / `tapDisabledByUserInput`; der Callback hat diese Typen ignoriert, danach reagierte nichts mehr bis zum Neustart. Der Tap wird jetzt reaktiviert.
- **Mögliches Einfrieren beim Aufnahmestart.** AX-Abfragen liefen mit der Vorgabe von ~25 s Timeout – eine hängende Ziel-App blockierte den Main-Thread. Jetzt 0,25 s.
- **Zwischenablage bleibt vollständig erhalten.** Gesichert wurde nur reiner Text, Bilder/Dateien/formatierter Text gingen beim Einfügen verloren. Der diktierte Text wird zusätzlich als `org.nspasteboard.ConcealedType` markiert, damit Clipboard-Manager und die geräteübergreifende Zwischenablage ihn nicht mitschneiden.
- **Request-Timeout von 30 s** für beide Groq-Endpunkte (vorher 60 s Vorgabe) – ein hängender Request blockierte so lange jede neue Aufnahme.
- Keine Abstürze mehr, wenn gerade kein Bildschirm gemeldet wird (Displays im Ruhezustand, Dock-/Undock-Moment).

### Features
- **Modell-Feld** in Einstellungen → Optionen → Nachbearbeitung.
- **Wörterbucheinträge sind bearbeitbar** statt nur anlegen/löschen.

## 0.9.6 – 2026-09-03

### Fixes
- **Fehlermeldungen sind lesbar.** Sie wurden im Overlay auf eine Zeile gekürzt und nach 1,2 s ausgeblendet – der Statuscode war nicht erkennbar. Jetzt: eigene mehrzeilige Fehler-Pille (bis 6 Zeilen, markierbar), 12 s sichtbar.
- Groq-Fehler werden aus dem JSON ausgepackt (`error.message` + `code`) statt als rohes JSON angezeigt; die vollständige Antwort landet zusätzlich per `NSLog` im System-Log.

### Changes
- Modell für die Nachbearbeitung ist konfigurierbar (`UserDefaults`-Schlüssel `chatModel`, Vorgabe `llama-3.3-70b-versatile`), damit ein abgekündigtes Modell keinen neuen Build erfordert.

## 0.9.5 – 2026-09-03

### Fixes
- **Berechtigungen bleiben über Updates hinweg erhalten.** Das Release-Skript signierte bisher ad-hoc (`codesign --sign -`); dabei ist die Designated Requirement bei jedem Build anders, sodass macOS jede neue Version als fremde App behandelt und Bedienungshilfen + Mikrofon erneut abfragt. Jetzt wird mit dem Apple-Development-Zertifikat signiert (Team 2M4LLPLRLS), die Identität bleibt damit stabil.
- `scripts/release.sh` erkennt die Signatur-Identität automatisch (bevorzugt „Developer ID Application", sonst „Apple Development") und lässt sich per `SIGN_IDENTITY=…` überschreiben. Ohne Zertifikat fällt es mit Warnung auf Ad-hoc zurück.
- Signatur wird nach dem Build verifiziert (`codesign --verify --strict`).

## 0.9.4 – 2026-09-03

### Features
- **„VoxDrop beenden" im Popover**: Linksklick auf das Menüleisten-Symbol zeigt den Beenden-Button jetzt direkt – vorher nur im Rechtsklick-Menü oder über die Einstellungen.

### Fixes
- Einstellungen: Der Sidebar-Ein-/Ausklapp-Button sprang beim Einklappen von links nach rechts. Die Sidebar ist jetzt fest eingeblendet und der Button entfernt.

## 0.9.3 – 2026-09-03

### Features
- **Overlay über dem Eingabefeld** (neue Voreinstellung): die Pille erscheint mittig direkt über dem Textfeld, in das geschrieben wird – ermittelt über die Accessibility-API (Caret-Rechteck, sonst Feld-Bounds, sonst unterer Fensterstreifen).
- Umschaltbar in Einstellungen → Optionen → **Anzeige**: „Über dem Eingabefeld" oder „An der Notch".

## 0.9.2 – 2026-09-02

### Changes
- **Removed Rage Mode.** Freed default hotkey: `FN + ⌘`.
- Notch overlay now grows out of and wraps around the hardware notch (Dynamic-Island style) on built-in displays; floating pill unchanged on external/clamshell displays.

### Fixes
- Silent recordings (no speech) no longer produce hallucinated transcription text. Three layers: the first ~0.45 s of metering is ignored (the start sound bleeds into the mic), the recording must contain enough real speech frames, and a final phrase filter drops lone Whisper hallucinations ("Vielen Dank.", "Untertitel …", "amara.org", …).

## 0.9.1 – 2026-04-19

### Features
- **Launch at Login** toggle in Settings → Optionen (Issue #3, via `SMAppService.mainApp`)
- **Info tab** in Settings showing version, author, link to releases page and manual „Nach Updates suchen" button (Issue #4)
- **Automatic update check** against `roman0803/voxdrop-releases` on app start – shows „🔔 Update verfügbar" entry in right-click menu when a newer version is available (Issue #4)
- **About entry** in right-click menu: „VoxDrop vX.Y.Z by Roman Hohenberg" opens the releases page (Issue #4)
- **Hotkey cheatsheet** in left-click popover: shows all current hotkeys per mode (Issue #5)
- **Settings redesign**: NavigationSplitView mit Sidebar – alle Bereiche per Einzelklick erreichbar, im macOS-System-Settings-Stil
- **Compact popover status**: kompakte Statuszeile statt großem Mikrofon-Symbol, blendet Warnungen bei fehlenden Bedienungshilfen oder verfügbarem Update ein

### Fixes
- Prompt tab: „Speichern" now shows „✓ Gespeichert" feedback for 2 s after saving (Issue #6)

## 0.9.0 – 2026-04-17

First public beta release.

### Features
- **Modes:** Standard, Plus (editorial polish), Rage (rephrase emotional to professional), Emoji, Translate (DE → EN), **Custom** (your own system prompt)
- **Hotkeys:** freely configurable per mode (modifier combo or `FN + key`), recordable by clicking the settings field
- **Recording modes:** hold mode (press and hold) or toggle mode (press → speak → FN to stop)
- **Dynamic-Island pill** in the notch (merging with the hardware notch) or floating on non-notch displays
- **Dictionary** for substituting frequent mistranscriptions
- **Sound feedback** with selectable system sounds
- **Text injection** via clipboard paste into the focused window

### Technical
- macOS 14+ (Sonoma or newer)
- Menu-bar app, no Dock icon
- Groq API (Whisper-Large-v3-Turbo + Llama 3.3) – bring your own API key
- Swift 5.9, SwiftUI + AppKit, zero external dependencies

### Known Limitations
- Not signed with an Apple Developer ID → first launch requires right-click → "Open" or `xattr -cr /Applications/VoxDrop.app`
- Requires Microphone and Accessibility permissions
