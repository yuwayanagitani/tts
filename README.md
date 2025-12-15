# TTS Auto Helper (Anki Add-on)

🎧 Add **one-click text-to-speech buttons** to your cards — configured **per note type + per field**.

This add-on uses your system/browser TTS via **Web Speech API** (`speechSynthesis`) inside Anki’s reviewer.  
No external AI calls. No audio files are generated. Just instant playback.

---

## What it does

✅ **Adds 🔊 buttons for selected fields** (e.g., Front / Back / Example / etc.)  
✅ **Per-note-type × per-field settings**: enable/disable + language + voice  
✅ **Auto-detects available voices** once per profile (cached)  
✅ **Works on both question and answer sides** (smartly detects which fields are shown)  
✅ **Auto-plays enabled fields sequentially** (plays one after another)

---

## How it looks (behavior)

- On the review screen, you’ll see buttons like:

🔊 Front 🔊 Back 🔊 Example

- Click a button → it speaks that field text.
- If multiple fields are enabled on the current side, it can **auto-play them in order** (sequential playback).

---

## Installation

1. Download the add-on ZIP (or clone the repository).
2. Put it into your Anki add-ons folder.
3. Restart Anki.

---

## Open settings

Open:

- **Tools → Add-ons → TTS Auto Helper → Config**

This opens a dedicated GUI (not the raw JSON editor).

---

## Settings overview

### Global
- **Enable TTS Auto Helper**  
  Turns the whole add-on on/off.

### Per note type + field
Pick a note type from the dropdown, then for each field:

- **Enabled**: show a 🔊 button for this field
- **Language**: e.g., `en-US`, `ja-JP`, etc.
- **Voice**: optional (choose from your available voices)

> Tip: Leave Voice empty (`""`) to use the default voice for that language.

### Reset
- **Reset to defaults**: restores the shipped default settings and rebuilds missing field settings.

---

## Voice auto-detection (important)

On profile load, the add-on runs a **one-time voice probe** using a hidden webview:
- It reads `speechSynthesis.getVoices()`
- Stores the result into config as `languages_auto` and `voices_auto`
- Next time, the settings dialog uses those auto-detected lists

If your voice list changes (OS updates, new voices installed), you can trigger re-detection by:
- Removing `voices_auto` / `languages_auto` in config (or resetting to defaults), then restarting Anki

---

## Notes / limitations

- **TTS requires Web Speech API support** in your Anki environment.
- Some systems return voices asynchronously; the add-on handles that (via `onvoiceschanged`).
- Field text is spoken as-is. If your fields contain heavy HTML, the browser may read raw text oddly.

---

## Troubleshooting

### 🔇 No voices or no sound
- Confirm your OS audio works.
- Try restarting Anki once (voice lists sometimes load only after a fresh start).
- Check if Web Speech API is available in your environment.

### 🧩 Buttons don’t appear
- Make sure the add-on is enabled.
- In settings, confirm the field is enabled for the correct note type.
- If the template doesn’t reference any fields, the add-on falls back to showing buttons for all fields.

### 🎙 Wrong language/voice
- Set the field’s **Language** explicitly (e.g., `ja-JP`).
- Choose a specific voice (optional).

---

## Privacy

- This add-on does **not** send your card text anywhere.
- No API keys are needed.
- TTS runs locally via your system’s available voices (through the reviewer webview).

---

## Technical notes (for developers)

- Buttons are injected via `gui_hooks.card_will_show`.
- Voice probing uses a hidden `AnkiWebView` and `pycmd()` bridge.
- Per-field settings live under `fieldSettings[model_id][field_name]`.

Enjoy faster pronunciation practice ✨
