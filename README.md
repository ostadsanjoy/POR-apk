# Personal Observation Records (POR)

A private, offline-first Android app for keeping structured notes on the people in your life — contacts, relationships, and a running timeline of observations about each one.

Built as a single-page web app and shipped as a native Android APK via [Capacitor](https://capacitorjs.com/).

## What it does

POR is a personal directory with a twist: every profile isn't just contact info, it's a living record you can add dated **observations** to over time — thoughts, context, or notes tied to that person.

- **Home directory** — a searchable, alphabetically-indexed list of every profile, with an A–Z scroll rail for quick jumps and a toggleable sort order.
- **Add / edit profile** — capture name, phone, "known since" date, relation (Friend, Colleague, Mentor, etc.), skill, address, remarks, free-form tags, and any number of custom key/value fields for anything the built-in fields don't cover.
- **Profile detail view** — a tabbed view splitting **Details** (the static profile info) from **Observations** (a chronological timeline).
- **Observations** — add dated notes with an optional context label and tags, building a history for each person over time.
- **Delete/edit** any profile or field at any time.

All data is stored locally on-device (`localStorage`) — nothing leaves the phone.

## Tech stack

| Layer | Technology |
|---|---|
| UI | Single-page vanilla HTML/CSS/JS (`www/index.html`) — no framework, no build step |
| Fonts | Google Fonts (Cormorant Garamond + Inter) |
| Native shell | [Capacitor](https://capacitorjs.com/) 8 (`@capacitor/core`, `@capacitor/android`) |
| Android | Gradle project (`android/`), min SDK 24, target/compile SDK 36 |
| Storage | Browser `localStorage`, persisted inside the WebView |
| CI | GitHub Actions — builds a debug APK on every push to `main` |

The entire app logic (rendering, form handling, search/sort/filter, CRUD for profiles and observations) lives in one file: `www/index.html`. Capacitor just wraps that web app in a native Android WebView shell with an app icon, splash screen, and installable APK.

## Project structure

```
.
├── www/
│   ├── index.html          # The entire app: markup, styles, and JS logic
│   └── photo/
│       └── background.jpg  # Background image used behind the frosted-glass UI
├── android/                 # Native Android (Capacitor) project
│   ├── app/                 # App module, manifest, MainActivity
│   ├── build.gradle
│   └── variables.gradle     # SDK versions & dependency versions
├── capacitor.config.json    # Capacitor config (app ID, name, web dir)
├── package.json
└── .github/workflows/
    └── build-apk.yml        # CI: builds a debug APK on push to main
```

## Getting started

### Prerequisites

- Node.js 22+
- JDK 21
- Android Studio / Android SDK (for building or running on a device/emulator)

### Setup

```bash
npm install
npx cap sync android
```

### Run on a device or emulator

Open the `android/` folder in Android Studio and hit Run, **or** build from the command line:

```bash
cd android
./gradlew assembleDebug
```

The debug APK will be output to `android/app/build/outputs/apk/debug/app-debug.apk`.

### Continuous integration

Every push to `main` triggers `.github/workflows/build-apk.yml`, which installs dependencies, syncs Capacitor, builds a debug APK, and uploads it as a workflow artifact — so you can grab a fresh build without touching Android Studio.

## App details

- **App ID:** `com.ostadsanjoy.por`
- **App name:** Personal Observation Records
- **Web dir:** `www`

## Notes

- All records are stored **only on the device** via `localStorage` inside the WebView — there is no backend, sync, or cloud storage. Uninstalling the app or clearing app data will erase all records.
- The UI is designed mobile-first with a frosted-glass aesthetic (backdrop blur, translucent surfaces) over a full-bleed background photo.
