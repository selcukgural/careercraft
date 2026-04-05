# CareerCraft — AI-Powered Career Assistant

> **Your smart career companion, powered by AI.** CareerCraft helps professionals craft standout CVs, compelling cover letters, optimized LinkedIn bios, and prepare for interviews — all in seconds.

[![Platform](https://img.shields.io/badge/Platform-Android-green?logo=android)](https://play.google.com/store)
[![Flutter](https://img.shields.io/badge/Flutter-3.x-blue?logo=flutter)](https://flutter.dev)
[![Firebase](https://img.shields.io/badge/Firebase-Firestore%20%7C%20Auth%20%7C%20Remote%20Config-orange?logo=firebase)](https://firebase.google.com)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-black?logo=openai)](https://openai.com)
[![RevenueCat](https://img.shields.io/badge/RevenueCat-Subscriptions-red)](https://revenuecat.com)

---

## 📱 Screenshots

| Home | CV Builder | Cover Letter | Interview Prep |
|------|-----------|--------------|----------------|
| ![Home](../store_ready/phone/en/phone_en_01_home.png) | ![CV](../store_ready/phone/en/phone_en_02_cv_resume.png) | ![Cover Letter](../store_ready/phone/en/phone_en_03_cover_letter.png) | ![Interview](../store_ready/phone/en/phone_en_05_interview.png) |

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 📄 **AI CV Generator** | Generates professional, metrics-driven CV summaries. Bans clichés like "results-driven" — forces concrete achievements instead. |
| ✉️ **Cover Letter Writer** | Writes targeted cover letters for specific companies and roles. References company details, avoids template openers. |
| 💼 **LinkedIn Bio Optimizer** | Crafts compelling "About" section bios with strong opening hooks and quantifiable achievements. |
| 🎯 **Interview Prep** | Generates 8 role-specific questions with full STAR-method answers, success tips, and common mistakes to avoid. |
| 📂 **History** | Cloud-backed history of all generated content. Free users see today's generations; premium users access everything. |
| ⚙️ **Custom Instructions** | Users can save personal AI instructions (e.g., "always use formal tone") and apply them to any generation. |
| 🌓 **Theme & Language** | Full dark/light/system theme support. Available in English and Turkish (🇬🇧 🇹🇷). |

---

## 🏗️ Architecture

CareerCraft follows a **feature-first, service-oriented architecture** with Riverpod as the reactive backbone.

```
lib/
├── core/                    # Global services, providers, theme, router
│   ├── services/            # Firebase Auth, Firestore, RevenueCat, OpenAI, Remote Config
│   ├── providers/           # Riverpod providers (auth, usage, settings, instructions)
│   ├── models/              # Shared data models
│   └── router.dart          # GoRouter — 9 declarative routes
│
├── features/                # Self-contained feature modules
│   ├── tools/               # cv / cover_letter / linkedin / interview
│   │   └── {feature}/
│   │       ├── providers/   # StateNotifier — OpenAI call + Firestore save
│   │       └── screens/     # ConsumerStatefulWidget UI
│   ├── home/
│   ├── paywall/
│   ├── history/
│   ├── instructions/
│   ├── settings/
│   └── auth/
│
└── shared/                  # Reusable widgets (AiResultView, InstructionSelector)
```

**State management pattern:** `StateNotifier` with immutable state + `copyWith`. Real-time data (history, instructions) flows through `StreamProvider` connected to Firestore.

---

## 🛠️ Tech Stack

### Core
| Layer | Technology |
|-------|-----------|
| Language | Dart 3.x |
| Framework | Flutter (Material Design 3) |
| State Management | **Riverpod 2.x** (StateNotifier pattern) |
| Navigation | **GoRouter 14** (declarative, deep-link ready) |

### Backend & Cloud
| Service | Purpose |
|---------|---------|
| **Firebase Auth** | Anonymous sign-in (transparent, zero friction) + Google OAuth ready |
| **Cloud Firestore** | Stores generated history & custom instructions per user |
| **Firebase Remote Config** | Securely distributes OpenAI API key at runtime (never hardcoded) |
| **Firebase Analytics** | User behavior tracking |
| **Firebase Crashlytics** | Production crash monitoring |

### AI & Monetization
| Service | Purpose |
|---------|---------|
| **OpenAI GPT-4o-mini** | Powers all 4 career tools via REST API |
| **RevenueCat** | Manages subscriptions (Monthly / Yearly / Lifetime) across platforms |

### Local
| Package | Purpose |
|---------|---------|
| `shared_preferences` | Caches usage count, premium status, theme, language |
| `google_fonts` | Inter font family |
| `flutter_animate` | Smooth UI animations |

---

## 💡 Notable Technical Decisions

**🔐 API Key Security via Remote Config**
The OpenAI API key is never embedded in the app binary. It's fetched at runtime from Firebase Remote Config, enabling key rotation without app updates.

**👤 Zero-Friction Authentication**
Users are silently signed in anonymously on first launch — no login screen, no barriers. Their data is immediately synced to Firestore. Google Sign-In can be layered on top later.

**📊 Smart Usage Limiting**
Free users get 3 generations/day. Premium users get 100+. Limits are tracked locally with `SharedPreferences` (date-keyed, timezone-safe) and verified against RevenueCat entitlements.

**🧠 Persona-Driven AI Prompts**
Each tool has a distinct AI persona with strict writing rules. The CV tool forbids buzzwords and mandates metrics. The Cover Letter tool prohibits "I am writing to apply" openers. This produces noticeably better output than generic prompts.

**🔄 Offline-Resilient Premium Check**
Premium status is checked via RevenueCat first. On network failure, the app falls back to a locally cached premium flag — users never lose access due to connectivity issues.

**📦 Firestore Data Gating at Query Level**
Free users' history query is filtered server-side by date (`where createdAt >= today`). No data leakage, minimal reads, efficient billing.

---

## 💰 Monetization

| Plan | Price (USD) |
|------|------------|
| Monthly | $3.99 / month |
| Yearly | $19.99 / year (~$1.67/mo) |
| Lifetime | $34.99 one-time |

Managed entirely through **RevenueCat** — handles purchase validation, restoration, and entitlement management across Android (and iOS in the future).

Free tier: **3 AI generations per day**
Premium tier: **100 generations per day + full history access**

---

## 🌍 Localization

Fully localized in **English** and **Turkish** using Flutter's built-in `intl` package with ARB files.

---

## 📦 Download

> 🚀 Coming soon to Google Play

---

*Built with ❤️ using Flutter · Firebase · OpenAI · RevenueCat*
