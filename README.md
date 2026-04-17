# VoxDrop

**Menu-Bar Speech-to-Text für macOS** – drücke einen Hotkey, sprich, und der transkribierte Text wird direkt ins aktive Fenster eingefügt.

Angetrieben von Whisper-Large-v3-Turbo (Groq API) für die Transkription und optional Llama 3.3 für Nachbearbeitung (Lektorat, Umformulierung, Emojis, Übersetzung, eigene Prompts).

---

## Download

Die aktuelle Version findest du unter [**Releases**](https://github.com/roman0803/voxdrop-releases/releases/latest):

- **`VoxDrop-X.Y.Z.dmg`** – empfohlen, installiere per Drag nach `/Applications`
- **`VoxDrop-X.Y.Z.zip`** – Fallback, entpackte `.app` manuell verschieben
- **`*.sha256`** – Prüfsummen zur Integritätsprüfung

---

## Installation

1. **DMG herunterladen** und öffnen.
2. **VoxDrop.app nach `/Applications` ziehen.**
3. **Beim ersten Start (wichtig – die App ist nicht mit Apple Developer ID signiert):**
   - Rechtsklick auf VoxDrop in `/Applications` → **"Öffnen"** → im Dialog erneut **"Öffnen"** bestätigen.
   - Falls das nicht hilft (*"VoxDrop ist beschädigt"*), im Terminal ausführen:
     ```bash
     xattr -cr /Applications/VoxDrop.app
     ```
4. **Systemeinstellungen → Datenschutz & Sicherheit:**
   - **Mikrofon** für VoxDrop aktivieren
   - **Bedienungshilfen** für VoxDrop aktivieren (für globale Hotkey-Erkennung)
5. **Menu-Bar-Icon** anklicken → **Einstellungen** → **Groq API Key** eintragen.
   API-Key erstellen: [console.groq.com/keys](https://console.groq.com/keys)

---

## Modi

| Modus | Beschreibung | Default-Hotkey |
|-------|--------------|----------------|
| **Standard** | Rohtranskription ohne Nachbearbeitung | FN + ⌃ |
| **Plus** | Lektorat, grammatikalisch geglätteter Text | FN + ⌥ |
| **Rage Mode** | Emotionale Nachrichten sachlich umformulieren | FN + ⌘ |
| **Emoji** | Text mit passenden Emojis anreichern | FN + ⇧ |
| **Übersetzen** | Deutsch → Englisch (casual, chat-like) | FN + ⌃⌥ |
| **Custom** | Dein eigener System-Prompt | (nicht belegt – frei konfigurierbar) |

Alle Hotkeys sind in den Einstellungen frei belegbar (Modifier-Kombi oder FN + Taste).

---

## Aufnahme-Modi

- **Hold-Modus:** Hotkey halten → sprechen → loslassen → Text wird eingefügt
- **Toggle-Modus:** Hotkey drücken → sprechen → **FN** zum Stoppen (alternative Beenden-Taste in Einstellungen wählbar)

---

## Anforderungen

- macOS **14.0** (Sonoma) oder neuer
- Groq API Key (kostenlos erstellbar)
- Mikrofon- und Bedienungshilfen-Freigabe

---

## Sicherheit & Signatur

Die App ist **ad-hoc signiert**, nicht über eine Apple Developer ID und nicht notarisiert. Das bedeutet, dass macOS beim ersten Start warnt. Der Installations-Workaround (Schritt 3 oben) ist Standard für Open-Source-macOS-Apps ohne Developer-Account.

Wer dem nicht vertraut, kann die SHA256-Prüfsumme vergleichen:

```bash
shasum -a 256 ~/Downloads/VoxDrop-0.9.0.dmg
```

Der Wert muss mit dem Inhalt der `.sha256`-Datei aus der Release übereinstimmen.

---

## Bugs & Feedback

Bitte [Issues](https://github.com/roman0803/voxdrop-releases/issues/new/choose) in diesem Repo öffnen. Es gibt zwei Vorlagen:

- **Bug melden** – wenn etwas nicht funktioniert
- **Feature vorschlagen** – wenn dir eine Idee fehlt
