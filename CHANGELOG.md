# Changelog

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
