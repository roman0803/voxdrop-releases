# VoxDrop

**Menu-bar Speech-to-Text for macOS** – press a hotkey, speak, and the transcribed text is inserted directly into the focused app.

Powered by Whisper-Large-v3-Turbo (Groq API) for transcription, and optionally Llama 3.3 for post-processing (editorial polish, rephrasing, emojis, translation, custom prompts).

> **About the name.** *Vox* is Latin for *voice*; *drop* captures what the app does: your spoken words are dropped directly at the cursor as text – no clipboard juggling, no extra window.

---

## Download

The latest version is on the [**Releases**](https://github.com/roman0803/voxdrop-releases/releases/latest) page:

- **`VoxDrop-X.Y.Z.dmg`** – recommended, drag into `/Applications`
- **`VoxDrop-X.Y.Z.zip`** – fallback, unpacked `.app` to move manually
- **`*.sha256`** – checksums for integrity verification

---

## Installation

1. **Download the DMG** and open it.
2. **Drag VoxDrop.app into `/Applications`.**
3. **On first launch (important – the app is signed, but not notarized):**
   - Right-click VoxDrop in `/Applications` → **"Open"** → confirm **"Open"** in the dialog.
   - If that doesn't work (*"VoxDrop is damaged"*), run in Terminal:
     ```bash
     xattr -cr /Applications/VoxDrop.app
     ```
4. **System Settings → Privacy & Security:**
   - Enable **Microphone** for VoxDrop
   - Enable **Accessibility** for VoxDrop (required for global hotkey detection)
5. Click the **menu-bar icon** → **Settings** → enter your **Groq API key**.
   Create a key at: [console.groq.com/keys](https://console.groq.com/keys)

---

## Modes

| Mode | Description | Default hotkey |
|------|-------------|----------------|
| **Standard** | Raw transcription, no post-processing | FN + ⌃ |
| **Plus** | Editorial polish, grammar-corrected text | FN + ⌥ |
| **Emoji** | Enrich text with matching emojis | FN + ⇧ |
| **Translate** | German → English (casual, chat-like) | FN + ⌃⌥ |
| **Custom** | Your own system prompt | *(unbound – configure yourself)* |

All hotkeys are freely configurable in the settings (modifier combo or `FN + key`).

---

## Recording Modes

- **Hold mode:** press and hold the hotkey → speak → release → text is inserted
- **Toggle mode:** press the hotkey → speak → press **FN** to stop (an alternate stop key can be set in settings)

---

## Requirements

- macOS **14.0** (Sonoma) or newer
- Groq API key (free tier available)
- Microphone and Accessibility permissions

---

## Updates

From **0.9.8** onwards VoxDrop updates itself. It checks a signed update feed, verifies the
download's EdDSA signature and installs it in place – no more downloading DMGs by hand.

You can turn the automatic check off or trigger one manually under
**Settings → Info**.

---

## Security & Code Signing

The app is signed with a stable Apple **Development** certificate (Team `2M4LLPLRLS`), but it is
**not notarized**. Two consequences:

- macOS warns on **first** launch – see step 3 above. This is standard for macOS apps without a
  paid developer account.
- Because the signing identity stays the same across versions, macOS keeps your Microphone and
  Accessibility permissions when you update. Earlier releases were ad-hoc signed and asked for
  them again every time.

If you'd rather verify the download, compare the SHA256 checksum:

```bash
shasum -a 256 ~/Downloads/VoxDrop-0.10.0.dmg
```

The result must match the contents of the `.sha256` file from the release.

---

## Bugs & Feedback

Please open an [Issue](https://github.com/roman0803/voxdrop-releases/issues/new/choose) in this repo. Two templates are available:

- **Bug report** – something isn't working
- **Feature request** – an idea or improvement
