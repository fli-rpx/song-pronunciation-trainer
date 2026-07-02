# 🎵 Song Pronunciation Trainer

A single-page web app for voice learning of pronunciation in song lyrics. Supports **7 languages**: English, French, German, Italian, Spanish, Russian, and Chinese.

> **Open `index.html` in Chrome, Edge, or Safari (requires HTTPS or localhost).**

---

## Quick Start

| Step | Action |
|------|--------|
| 1 | Pick the song's language at the top |
| 2 | Paste song lyrics into the text area |
| 3 | (Optional) Paste an English translation line-by-line |
| 4 | Click **Start Practice** |
| 5 | For each line: press 🔊 **Play** → 🎤 **Record** → ⏭ **Next** |

---

## Language Selection

| Button | Language | Speech Recognition | Voice |
|--------|----------|-------------------|-------|
| **English** | English | en-US | English |
| **Français** | French | fr-FR | French |
| **Deutsch** | German | de-DE | German |
| **Italiano** | Italian | it-IT | Italian |
| **Español** | Spanish | es-ES | Spanish |
| **Русский** | Russian | ru-RU | Russian |
| **中文** | Chinese (Simplified) | zh-CN | Chinese |

Click a button to select — active one gets filled accent color. The selection affects both speech recognition language and text-to-speech voice.

---

## How to Practice

### 1. Input Lyrics

- **Lyrics**: paste song text (one line per row)
- **English Translation** (optional): paste a line-by-line translation — it shows below the current line during practice
- Click **Start Practice**

### 2. Practice Mode

Three zones appear:

#### Zone A — Teleprompter

- **Large centered text** — current line
- Above (dimmed) — previous line
- Below (further dimmed) — next line
- Line counter e.g. `Line 3 of 24`
- English translation shown below if provided

#### Zone B — Controls

| Button | Action | When to use |
|--------|--------|-------------|
| 🔊 **Play** | Speaks current line via TTS | First — hear the reference |
| 🎤 **Record** | Listens via microphone | After Play — repeat the line |
| 🔤 **Play Word** | Pronounces one word with highlight | For word-by-word drilling |
| ⏭ **Next Word** | Advance to the next word | Paired with Play Word |
| ⏭ **Next** | Advance to next line | After receiving a score |
| ⏭ **Finish** | (on last line) Show session summary | After completing all lines |

> 🎯 **Keyboard shortcut:** Spacebar toggles recording on/off while in practice mode (when focus isn't on a button or textarea).

#### Zone C — Score & Feedback

After each recording attempt:

| Metric | Meaning |
|--------|---------|
| **Pronunciation Score** | Final 0–100% |
| **You said** | What the mic transcribed |
| **Reference** | The original line |
| **Confidence** | Recognition confidence % |
| **Accuracy** | Word match percentage |

Color coding:
- 🟢 **≥ 80%** — great
- 🟡 **50–79%** — decent, room to improve
- 🔴 **< 50%** — try again

---

## Extra Settings

### Word by Word Mode

Toggle 🔤 **Word by Word** — then **Play** speaks the line one word at a time with live highlighting. Great for breaking down tough lines.

### Playback Speed

**0.5x · 0.75x · 1.0x · 1.25x · 1.5x**

- 0.5x — slow (for dissection)
- 1.0x — normal (default)
- 1.5x — fast (natural song tempo)

---

## Session Summary

After the last line:

| Metric | Description |
|--------|-------------|
| **Overall Score** | Average across all lines |
| **Language Practiced** | Which language |
| **Total Lines Completed** | How many lines recorded |
| **Best Line** | Highest scoring line |
| **Worst Line** | Lowest scoring line |

Click **Practice Again** to clear and start with a new song.

---

## Browser Support

| Browser | Status |
|---------|--------|
| Chrome 80+ | ✅ Full support |
| Edge 80+ | ✅ Full support |
| Safari 14.1+ (iOS 14.5+) | ✅ Mostly works |
| Firefox, Opera | ⚠️ May have limitations |

Speech recognition requires **HTTPS** or **localhost**.

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Record button disabled | Browser doesn't support SpeechRecognition. Use Chrome/Edge. |
| Yellow warning banner | SpeechRecognition not supported in this browser |
| "Microphone access denied" | Grant mic permission in browser settings |
| "No speech detected" | Check mic, speak louder, move closer |
| Recording interrupted | Don't switch tabs mid-recording |
| Chinese/Russian accuracy low | SpeechRecognition for zh-CN/ru-RU is less accurate than en-US — browser limitation |

---

## Tips for Effective Practice

1. **Always listen first** — hit Play before Record
2. **Repeat out loud** — the mic captures your pronunciation
3. **Track progress** — the progress bar shows how far you've come
4. **Use word-by-word** for tricky phrases
5. **Slow it down** on complex lines (0.5x–0.75x)
6. **Read the translation** — understanding helps pronunciation

Happy practicing! 🎤🎶
