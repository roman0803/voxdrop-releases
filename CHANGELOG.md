# Changelog

## 0.10.6 – 2026-09-03

### Fixes
- **Pille landete in Electron-Apps unten über dem Dock.** Meldet eine App kein fokussiertes Element – bei Electron der Normalfall –, gab `CaretLocator` sofort `nil` zurück und übersprang den Fenster-Rückfall, den 0.10.5 eingeführt hatte. Jetzt wird auch dort die Fenstergeometrie genutzt.
- **App fragt jetzt nach, wenn die Berechtigung im Betrieb wegfällt** – etwa nachdem ein Update das Bundle ersetzt hat. Bisher wurde nur beim Start gefragt; danach merkte man den Verlust nur daran, dass nichts mehr reagierte. Der Hinweis erklärt zusätzlich, dass ein bestehender Haken nach einem Update neu gesetzt werden muss.

## 0.10.5 – 2026-09-03

### Fixes
- **Schlüsselbund fragte ständig nach dem Passwort.** Zwei Ursachen: Der API-Key wurde bei *jeder* Anfrage neu aus dem Schlüsselbund gelesen, und die Migrationsprüfung beim Start las das Passwort mit, obwohl sie nur ein Attribut brauchte. Der Key wird jetzt im Speicher gehalten, und die Prüfung liest nur noch Attribute und merkt sich ihr Ergebnis.
- **Pille erschien unten über dem Dock statt über dem Eingabefeld.** Ohne Bedienungshilfen-Berechtigung liefert die Accessibility-API nichts, und der Rückfall war die Bildschirmmitte. Jetzt wird die Fenstergeometrie über `CGWindowListCopyWindowInfo` ermittelt – das braucht keine Berechtigung –, und die Pille sitzt am unteren Rand des aktiven Fensters.

### Changes
- Glow etwas kräftiger: höhere Grundstärke, größere Radien, deutlicherer Rand.

## 0.10.4 – 2026-09-03

### Features
- **Glow um die Pille** in der Farbe des Modus – zwei Ebenen für Tiefe, im Fehlerfall orange. Während der Aufnahme folgt die Stärke dem Mikrofonpegel, danach bleibt sie ruhig.
- **Abstand über dem Eingabefeld einstellbar** (Optionen → Anzeige, 40–300 pt, Vorgabe 140). Betrifft nur Apps, die die Cursor-Position nicht melden – dort ist der Abstand zwangsläufig geschätzt, und wer ihn sieht, kann ihn passend machen.

### Fixes
- Testlauf brach mit „runner hung before establishing connection" ab: Der modale Bedienungshilfen-Dialog beim Start blockiert im Test-Host den Runloop. Unter XCTest wird er jetzt übersprungen.

## 0.10.3 – 2026-09-03

### Fixes
- **Overlay sah aus wie zwei Fenster.** Die Pille zeigte 0,14 s lang eine schmale, leere Kapsel und tauschte dann auf den vollen Inhalt – eine Choreografie, die für die Notch-Variante gedacht war. Die freistehende Pille blendet jetzt einstufig ein.
- **Pille lag auf dem Eingabefeld statt darüber.** In Apps ohne genaue Caret-Position (Electron) wird ein Streifen am Fensterboden als Anker genommen; der war mit 56 pt niedriger als ein typisches Chat-Eingabefeld. Jetzt 96 pt, und der Anker meldet, wie genau er ist – beim Fensterrückfall gibt es zusätzlich etwas mehr Abstand.

## 0.10.2 – 2026-09-03

### Fixes
- **Einstellungen wurden beim Custom-Prompt leer** – auch die Sidebar. Kein Absturz, sondern ein Layout, das unbegrenzte Höhe verlangt hat: `List` und `TextEditor` in einem `VStack` ohne Scroll-Container. Beide sind ersetzt durch eine `ScrollView` mit fester Höhe und ein mehrzeiliges Textfeld. Der Prompts-Bereich ist insgesamt scrollbar.

### Changes
- **Anzeigedauer nach dem Einfügen einstellbar** (Optionen → Anzeige): 0,4 / 0,8 / 1,2 / 2 s. Vorgabe ist jetzt 0,8 s statt 1,2 s. Fehlermeldungen bleiben unabhängig davon 12 s stehen.

## 0.10.1 – 2026-09-03

### Fixes
- **„Systemeinstellungen öffnen" tat nichts.** Der Deep-Link nutzte die Adresse von vor macOS 13 (`com.apple.preference.security`); auf aktuellen Systemen läuft die ins Leere. Jetzt die richtige Adresse mit Rückfall, und zusätzlich wird der systemeigene Berechtigungsdialog angestoßen.
- **Hotkeys blieben nach dem Erteilen der Berechtigung tot.** Der Event-Tap wurde nur einmal beim Start erzeugt – ohne Bedienungshilfen sieht er nur Events der eigenen App, weshalb Hotkeys nur im VoxDrop-Fenster griffen. Er wird jetzt automatisch neu aufgebaut, sobald die Berechtigung erteilt wird. Ein Neustart der App ist nicht mehr nötig.
- **Einstellungen wurden beim Custom-Prompt leer.** Die Preset-Oberfläche arbeitete mit Index-Zugriffen auf ein `@Published`-Array; ein Index wird ungültig, sobald sich das Array ändert. Jetzt direkte Bindings über `ForEach($…)`.
- **App blieb nach einer Update-Prüfung im Dock** und startete nach dem Update nicht neu. Für Sparkles Fenster wird auf `.regular` umgeschaltet, aber nie zurück – das widerspricht `LSUIElement`. Wird jetzt am Ende der Update-Sitzung zurückgesetzt.
- Der `NSEvent`-Monitor läuft wieder parallel zum Event-Tap, doppelte Auslösungen werden herausgefiltert statt einen der beiden Wege wegzulassen.

## 0.10.0 – 2026-09-03

### Features
- **Prompt-Presets für den Custom-Modus.** Statt eines einzigen Prompts jetzt mehrere benannte – etwa „Mail", „Slack", „Ticket" –, von denen einer aktiv ist. Umschaltbar in den Einstellungen und ab zwei Presets auch direkt im Popover. Ein vorhandener Custom-Prompt wird beim ersten Start automatisch als Preset übernommen.
- **Nochmal verarbeiten.** Jeder Eintrag im Verlauf lässt sich ohne neues Diktat durch einen anderen Modus schicken. Das Ergebnis landet in der Zwischenablage und als neuer Verlaufseintrag, markiert mit „↻".

### Technical
- 7 weitere Tests (Preset-Speicherung, aktives Preset, Rückfall bei verwaister ID, Migration des alten Prompts). Gesamt 28.

## 0.9.11 – 2026-09-03

### Features
- **Transkriptionssprache wählbar** (Einstellungen → Optionen → Transkription). Bisher war `language=de` fest verdrahtet, englisches Diktat wurde also in deutsche Phonetik gezwungen. Neben fünf Sprachen gibt es „Automatisch erkennen", das das Feld weglässt und Whisper selbst entscheiden lässt.

### Technical
- **Erstes Test-Target mit 21 Tests.** Dafür wurde Logik aus privaten Funktionen am Singleton herausgelöst: `TranscriptFilter` als eigener Typ, `DictionaryManager.substitute(_:using:)` und `vocabularyTerms(entries:extraField:)` als reine Funktionen. Abgedeckt sind Wörterbuch-Ersetzung, Vokabular-Zusammenstellung, Halluzinations-Filter, Hotkey-Speicherrundlauf und Sprachauswahl.

## 0.9.10 – 2026-09-03

### Fixes
- **Jeder Tastendruck wurde doppelt verarbeitet.** `HotkeyManager` betrieb einen `NSEvent`-Monitor und einen `CGEventTap` gleichzeitig auf dieselben Events. Das verlängert den Callback und provoziert genau den Timeout, wegen dem macOS den Tap deaktiviert. Der Tap ist jetzt der einzige Weg; der Monitor bleibt nur als Rückfall, falls `tapCreate` scheitert – vorher startete die App in dem Fall stillschweigend ganz ohne Hotkeys.

### Features
- **Verarbeitung abbrechbar.** Ein hängender Request blockierte bis zum Timeout jede neue Aufnahme. Im Popover erscheint während der Verarbeitung „Verarbeitung abbrechen".

## 0.9.9 – 2026-09-03

### Fixes
- **API-Key bleibt auf diesem Gerät.** Der Schlüsselbund-Eintrag nutzte `kSecAttrAccessibleAfterFirstUnlock` und konnte damit in Backups wandern; jetzt `…ThisDeviceOnly`. Bestehende Installationen werden beim Start einmalig migriert – der Schlüssel muss nicht neu eingegeben werden.
- **Verwaiste Aufnahmen werden aufgeräumt.** Nach einem Absturz blieb die Temp-Datei liegen. Beim Start werden `voxdrop_*.m4a` entfernt, und laufende Aufnahmen bekommen die Dateirechte `0600`.
- **Weniger im System-Log.** Die vollständige Groq-Antwort landete im systemweit lesbaren Unified Log, jetzt auf 500 Zeichen gekürzt. Ein übrig gebliebenes Debug-Log ist entfernt.

### Features
- **Datenschutzhinweis** in Einstellungen → API: benennt, dass jede Aufnahme an api.groq.com geht, dass die Modi Plus, Emoji, Übersetzen und Custom den Text zusätzlich ans Sprachmodell schicken, und was lokal bleibt.

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
