# Song Pronunciation Trainer — Web App Requirements

## Overview

A single-page web app for voice learning of pronunciation in French, German, and Italian songs. Users paste song lyrics, read them line-by-line with a teleprompter-style display, record themselves repeating each line, and receive a pronunciation score with feedback.

## Output

Single self-contained HTML file: `index.html` at the repo root. No external dependencies (no build tools, no npm). All CSS and JS inline in the HTML file.

## Tech Stack

- **Vanilla HTML/CSS/JS** — no frameworks, no build step
- **Web Speech API** (`SpeechRecognition` / `webkitSpeechRecognition`) for voice capture and scoring
- **Web Speech API** (`SpeechSynthesisUtterance`) for line playback (reference pronunciation)
- **Responsive design** — works on desktop and mobile browsers

## Core Features

### 1. Language Selection (Top of page)

Three large toggle buttons at the top: **Français** | **Deutsch** | **Italiano**

- Selected button: filled with accent color, unselected: outlined
- Language selection affects:
  - Speech recognition language (`SpeechRecognition.lang`: `fr-FR`, `de-DE`, `it-IT`)
  - Speech synthesis voice selection
  - UI labels remain in English (the user reads English UI)

### 2. Lyrics Input Area

A large multi-line text box (`<textarea>`) placeholder: "Paste your song lyrics here..."

- Below it: a **"Start Practice"** button (accent color, large)
- On click: parses lyrics into lines (split by newline, filter empty lines)
- Each line is trimmed of leading/trailing whitespace
- Lines shorter than 3 characters are skipped (they're likely stage directions or punctuation-only lines)

### 3. Practice Mode UI (shown after Start Practice)

The main practice interface has three zones:

#### Zone A — Line Display (Teleprompter)

- Current line displayed in large centered text (2.5rem+ font)
- Previous line shown above in dimmed/muted color (60% opacity)
- Next line shown below in dimmed/muted color (40% opacity)
- Smooth slide animation when advancing to next line
- Line counter: "Line 3 of 24"

#### Zone B — Control Buttons (below the line display)

Three large buttons in a horizontal row:

| Button | Icon | Action |
|--------|------|--------|
| **🔊 Play** | Speaker icon | Speaks the current line using SpeechSynthesis (correct pronunciation) |
| **🎤 Record** | Microphone icon | Starts speech recognition, listens for user's repetition |
| **⏭ Next** | Forward arrow | Advances to the next line, resets record state |

Button behavior:
- **Play** button: disabled while speaking, re-enabled after speech ends
- **Record** button: changes to red "🔴 Recording..." while listening; disabled if already recording
- **Next** button: disabled while recording; re-enabled when stopped or result received

#### Zone C — Score & Feedback Panel (below control buttons)

After each recording attempt, shows:

| Metric | Display | Source |
|--------|---------|--------|
| **Score** | Large percentage (0-100%) colored green/amber/red | Derived from SpeechRecognition confidence × transcript accuracy |
| **You said** | Italic text showing what the user said (the transcript) | SpeechRecognition result |
| **Reference** | The original line text | From lyrics |
| **Accuracy** | Bar or indicator showing exact word match % | String comparison of transcript vs reference |
| **Tips** | One-line tip if score < 70% | Rule-based: "Check vowel sounds" / "Slow down" / "Try again" |

### 4. Progress Bar

- A horizontal bar at the top of the practice area
- Fills from left to right as user progresses through lines
- Shows "5/24" fraction in center

### 5. Session Summary

When the user reaches the last line and clicks Next, show a session summary panel:

- **Overall Score**: Average of all line scores (large display)
- **Best Line**: Line number, text preview, and score
- **Worst Line**: Line number, text preview, and score
- **Language practiced**: French / German / Italian
- **Total lines completed**: N
- **"Practice Again"** button: resets to initial state (clears lyrics, goes back to textarea)

## Pronunciation Scoring Algorithm

The scoring has two components combined into a final 0-100% score:

### Component 1: SpeechRecognition Confidence (0-1.0)
Web Speech API returns a `confidence` value per alternative. Use the top alternative's confidence.

### Component 2: Word Match Rate (0-100%)
Compare the transcript (lowercased, punctuation stripped) against the reference line (lowercased, punctuation stripped):
- Split both into words
- Count exact word matches (order-independent, i.e., bag-of-words)
- Word match % = count(matching words) / count(reference words) × 100

### Final Score Formula
```
confidence_score = SpeechRecognition.confidence × 100
word_match_score = word_match_percentage
final_score = (confidence_score × 0.4) + (word_match_score × 0.6)
```

Round to nearest integer. Clamp to [0, 100].

### Score Color Coding
- ≥ 80%: **green** (#00cc66)
- 50-79%: **amber** (#ffaa00)
- < 50%: **red** (#ff4444)

## Visual Design

### Color Palette

| Role | Hex | Usage |
|------|-----|-------|
| Background | `#0d0d14` | Page background |
| Panel background | `#16161f` | Card/panel backgrounds |
| Panel border | `#2a2a3a` | Card borders |
| Primary text | `#e8e8f0` | Main body text |
| Muted text | `#8888a0` | Secondary text, line context |
| **Accent** | `#7c5cfc` | Primary buttons, active state |
| Accent hover | `#9b7cff` | Button hover |
| Success | `#00cc66` | High score display |
| Warning | `#ffaa00` | Medium score display |
| Danger | `#ff4444` | Low score display |
| Record active | `#ff3344` | Recording state indicator |

### Typography

- Body: `-apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif`
- Lyrics display: `Georgia, "Times New Roman", serif` (for the teleprompter area)
- Monospace for score display: `"SF Mono", "Fira Code", "Cascadia Code", monospace`
- Base font size: 16px

### Layout

- Max content width: 720px, centered with auto margins
- Top section: language selector (3 buttons, horizontal, full-width)
- Middle section: textarea or practice area (takes full width)
- Bottom section: stats/summary (full width)

### Responsive Breakpoints

- Desktop (>768px): side-by-side where possible, comfortable padding (32px)
- Mobile (<768px): stacked layout, smaller font for lyrics (1.5rem), full-width buttons, padding reduced to 16px

## Edge Cases & Behavior

| Scenario | Behavior |
|----------|----------|
| Empty textarea on Start Practice | Show inline error: "Please paste song lyrics first" in red, don't proceed |
| Single line of lyrics | Practice mode with just 1 line; Next button shows "Finish" instead |
| Very long line (>100 chars) | Display truncated to 100 chars with "..." in the line display; full version shown in reference area |
| SpeechRecognition not supported | Show warning banner at top: "⚠️ Speech recognition is not supported in your browser. Try Chrome or Edge." Disable Record button, show score section with note "Recording unavailable" |
| No microphone permission | Show error: "🎤 Microphone access denied. Please allow microphone access in your browser settings." |
| SpeechRecognition returns empty transcript | Show score 0 with tip: "No speech detected. Try speaking louder or check your microphone." |
| Special characters in lyrics (accents, umlauts, è, é, ü, ö, ß) | Must be preserved and displayed correctly. UTF-8 throughout. |
| Punctuation-only lines | Filtered out during line parsing (length check after stripping punctuation) |
| Browser tab hidden during recording | SpeechRecognition may timeout — handle with onerror/onend fallback, show "Recording interrupted" message |

## Browser Support

Minimum: Chrome 80+, Edge 80+, Safari 14.1+ (iOS 14.5+). Speech recognition requires HTTPS or localhost.

## File Output

Single file: `/Users/fudongli/Projects/song-pronunciation-trainer/index.html`

Self-contained. All CSS `<style>` and JS `<script>` inline. No external resources except:
- Google Fonts or system fonts (specified via `@font-face` or system stack)
- No external JS libraries, no CDN scripts

## File Encoding

UTF-8 without BOM. LF line endings.

## Deliverables Checklist

- [ ] `index.html` — single self-contained file
- [ ] Language selector: Français, Deutsch, Italiano
- [ ] Textarea for lyrics input
- [ ] Start Practice button with validation
- [ ] Teleprompter line display (current, previous, next)
- [ ] Play button (SpeechSynthesis)
- [ ] Record button (SpeechRecognition)
- [ ] Next button with Finish on last line
- [ ] Score panel (confidence, word match, final score, tips)
- [ ] Progress bar with counter
- [ ] Session summary on completion
- [ ] Error handling (no mic, no support, empty lyrics)
- [ ] Responsive design
- [ ] Dark theme with specified color palette
