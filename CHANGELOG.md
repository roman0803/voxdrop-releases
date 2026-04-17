# Changelog

## 0.9.0 – 2026-04-17

Erste öffentliche Beta-Release.

### Features
- **Modi:** Standard, Plus (Lektorat), Rage (sachlich umformulieren), Emoji, Übersetzen (DE → EN), **Custom** (eigener System-Prompt)
- **Hotkeys:** frei belegbar pro Modus (Modifier oder FN + Taste), per Klick im Einstellungsfeld aufnehmbar
- **Aufnahme-Modi:** Hold-Modus (Taste halten) oder Toggle-Modus (drücken → sprechen → FN zum Stoppen)
- **Dynamic-Island-Pille** in der Notch (mit Hardware-Notch verschmelzend) oder schwebend auf Nicht-Notch-Displays
- **Wörterbuch** zum Ersetzen häufiger Fehltranskriptionen
- **Sound-Feedback** mit auswählbaren System-Sounds
- **Text-Injection** via Clipboard-Paste ins aktive Fenster

### Technisch
- macOS 14+ (Sonoma oder neuer)
- Menu-Bar-App ohne Dock-Icon
- Groq API (Whisper-Large-v3-Turbo + Llama 3.3) – eigener API-Key erforderlich
- Swift 5.9, SwiftUI + AppKit, keine externen Dependencies

### Bekannte Einschränkungen
- Nicht signiert mit Apple Developer ID → erster Start erfordert Rechtsklick → "Öffnen" oder `xattr -cr /Applications/VoxDrop.app`
- Benötigt Berechtigungen für Mikrofon und Bedienungshilfen
