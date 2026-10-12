# BayMax — Android AI Voice Assistant

A production-ready Android AI Voice Assistant built with Kotlin + Gemini Live WebSocket API.

---

## 🚀 Setup in Android Studio

### Step 1 — Open Project
1. Open **Android Studio**
2. Click **File → Open** and select the `BayMax` folder
3. Wait for Gradle sync to complete (it will download dependencies automatically)

### Step 2 — Get Your Gemini API Key (FREE)
1. Go to **https://aistudio.google.com/app/apikey**
2. Click **Create API key**
3. Copy your key

### Step 3 — Enter API Key in App
- You do **NOT** hardcode the key in any file
- On first launch, tap the ⚙️ **Settings** button → paste your API key → Save
- The app stores it securely in SharedPreferences on-device

### Step 4 — Grant Permissions
On first run, grant all requested permissions:
- Microphone (for voice input)
- Phone / Contacts (for calls)
- Overlay (for floating orb — go to Settings → Apps → BayMax → Draw over other apps)
- Accessibility (go to Settings → Accessibility → BayMax → Enable)

---

## 📦 Build Requirements
- Android Studio Hedgehog (2023.1.1) or newer
- JDK 17 (included with Android Studio)
- Min SDK 26 (Android 8.0+)
- Internet connection for Gradle sync

---

## 🎤 Features
- **Gemini Live WebSocket** — native bidirectional audio streaming
- **Human-like voice** — Aoede, Charon, Kore, Fenrir, Puck, Leda, Orus, Zephyr
- **Hinglish GF mode** — warm, caring, natural Hinglish conversation
- **Voice commands** — open apps, make calls, send SMS, control volume/flashlight/WiFi
- **Incoming call announcement** — BayMax announces caller + listens for accept/reject
- **Prime Contacts** — quick-dial contacts via voice ("close friend ko call karo")
- **Floating orb overlay** — double-press power button to show overlay on any screen
- **7-layer animated orb** — idle pulse, listening rings, speaking particles, thinking arcs
- **20-bar waveform** — amplitude-reactive visualization

---

## 🔑 SharedPreferences Keys
| Key | Description |
|-----|-------------|
| `api_key` | Your Gemini API Key |
| `user_name` | Your name (used in prompts) |
| `personality_mode` | `gf` / `professional` / `assistant` |
| `gemini_model` | Model string (see Settings) |
| `gemini_voice` | Voice name (Aoede default) |
| `prime_contacts_json` | JSON array of prime contacts |

---

## 🏗️ Architecture
```
MVVM
├── ai/           ← GeminiLiveClient (WebSocket) + AudioEngine (PCM) + CommandParser
├── model/        ← AppCommand data class
├── service/      ← AccessibilityService, CallMonitor, OverlayService, Receivers
├── ui/main/      ← MainActivity, OrbAnimationView, WaveformView, ChatAdapter
├── ui/settings/  ← SettingsActivity, PrimeContactAdapter
└── viewmodel/    ← MainViewModel (phone actions, contacts, commands)
```

---

## ⚠️ Notes
- **BidiGenerateContent WebSocket** (`v1alpha`) is used — this is Gemini Live, not REST
- Audio: Mic → 16kHz PCM → WebSocket → Gemini → 24kHz PCM → Speaker
- Session auto-renews every 9 minutes (keepalive every 8 seconds)
- Auto-reconnects on disconnect with 3s delay
