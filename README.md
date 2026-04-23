# Spendly — Personal Finance & Expense Tracker

> 🌐 **Website**: [tejnc.github.io/spendly](https://tejnc.github.io/spendly)
> 📱 **Google Play**: [Spendly on Play Store](https://play.google.com/store/apps/details?id=com.tejnc.spendly)

A modern, privacy-first personal finance app built with Flutter. Track expenses, scan receipts with AI, manage multiple accounts, split bills with friends online, plan budgets, track loans, and earn achievements — all stored locally on your device with no account required. Includes a fully shipped, privacy-isolated cloud-backed Split Tracker for shared group expenses.

![App Icon](assets/icons/app_icon.png)

---

## Features

### Transactions & Accounts
- **Expense, Income & Transfer** tracking with custom categories, notes, and images
- **Multi-account management** — Cash, Bank, Credit Card, E-Wallet, Investment, and more
- **Seamless transfers** between accounts with automatic balance updates
- **Recurring transactions** — automate bills and subscriptions (daily / weekly / monthly / yearly), processed on app start; individual occurrences can be skipped
- **Person & loan linking** — attach transactions to contacts or loans for full context
- **Quick category creation** directly from the add-transaction, budget, recurring, and bill splitter screens — newly created category auto-selected
- **Speed-dial FAB** — one-tap access to manual entry, camera scan, or gallery import

### AI Receipt Scanning
- **OCR via Google ML Kit** — snap a receipt photo and auto-fill amount, date, and title
- **AI extraction layer** refines OCR output for better accuracy; AI-refined results are flagged with an "AI REFINED" badge
- Image preprocessing runs in an isolate for smooth UI performance
- Works fully offline — no image upload required
- **Biometric-safe**: lock grace period prevents re-authentication mid-scan when returning from the camera app

### Budgets & Goals
- Set **spending limits** per category — weekly, monthly, or yearly
- Create **savings goals** with target amounts and deadlines
- Real-time progress tracking with visual indicators
- Budget threshold notifications before you overspend

### Analytics & Reports
- **Pie chart** — category-wise expense breakdown for any period
- **Bar chart** — income vs. expense trends over time
- Filter by date range (Today / Week / Month / Year / Custom) and account
- **Amounts visibility toggle** — hide all monetary values with one tap across the entire app
- Export as **PDF or CSV** and share via any installed app
- **Per-transaction PDF receipts** — generate and share a receipt for any individual transaction

### Calendar
- **Monthly & week views** — swipe left/right to navigate, swipe up/down to toggle view
- **Activity heatmap** — color-coded transaction density per day at a glance
- Tappable month/year header for quick navigation via a month-picker sheet

### Split Tracker (Cloud)
- **Supabase-backed group expense tracking** — Splitwise-style shared expenses with real-time sync
- Create groups, invite members, and add multi-payer expenses with equal, percentage, or exact splits
- **Settlement tracking** — record payments between members and see live balance summaries
- **Group Activity feed** — All / Mine / Payments tab views with "See all" full-screen pagination
- **Leave group** — members can leave; last admin must promote a successor first
- **Profile screen** — edit display name, phone, profession, currency, and bio from within Split Tracker
- Lazy initialization — no network connection opened until the user deliberately enables Split Tracker
- Configurable via `--dart-define` at build time (see [Setup](#setup))

### Bill Splitting (Local)
- Split expenses with friends using **equal, percentage, or exact** amounts
- Track settlements with a 4-case settlement flow (payer ↔ participant logic)
- Full history of past splits with per-person breakdowns
- Auto-links to People contacts for consistent tracking

### Loans & Debt Tracking
- Track money **lent to** or **borrowed from** contacts
- Record partial repayments and view full payment history
- Automatic **overdue detection** with status badges (Active / Paid / Overdue)
- Link loan-related transactions for a complete picture

### People & Contacts
- Manage financial contacts in a dedicated screen
- Attach a **relation** (Friend, Family, Client…) and optional description
- Link contacts to transactions, loans, and bill splits
- Bulk selection, search, and delete

### Achievements & Gamification
- **Daily streak** tracking — consecutive days with at least one logged transaction
- **Longest streak** and total log-day counters
- Earn **badges** for milestones: first transaction, savings goals, budget discipline, and more
- Achievement unlock **animation** displayed app-wide when a badge is earned
- Full achievements screen with progress tracking across all categories

### Categories
- Custom categories with a **Material icon picker** (20 presets) or a **gallery image**
- 10 color options with live preview
- Dedicated **full-screen add/edit category** experience — opens above the current screen and returns the new category UID so it is auto-selected in the calling screen
- Default seed categories provided on first install; defaults are protected from accidental deletion

### Notifications
- Bill payment reminders tied to recurring transactions
- Budget alert notifications when thresholds are crossed
- **Daily expense logging reminder** — configurable time and toggle in Settings
- **Notification history screen** — review, mark as read, and clear past alerts

### Privacy & Security
- **100% local storage** — all data lives on-device using Isar (no cloud sync, no account required)
- **Biometric lock** — Face ID / fingerprint authentication via `local_auth`
- **Smart lock grace period** — suppresses re-lock when the app briefly backgrounds for the camera, file picker, or other system activities
- **Encrypted backup & restore** — AES-encrypted JSON exports with a user-set PIN via `encrypt` + `flutter_secure_storage`

### Auto Backup
- **Scheduled daily backups** — runs automatically on app start if enabled and overdue
- Configurable **retention count** (default 7 files) — older backups pruned automatically
- Backups stored in the app documents directory (`Spendly/auto_backups/`)
- Encrypted with the same PIN as manual backups

### Android Home Screen Widgets
- **Budget widget** — at-a-glance spending vs. budget for the current period
- **Quick Add widget** — add a transaction directly from the home screen
- **Split Bill widget** — view pending splits without opening the app
- Data pushed from `WidgetService` Flutter bridge; refreshed whenever transactions change

### Export & Sharing
- Export transaction history as **PDF** or **CSV**
- Share reports directly from the app via `share_plus`

### Personalization
- **Dark / Light / System** theme modes; true-black OLED option (`justBlack` scheme)
- **8 color schemes** — Teal, Purple, Indigo, Pink, Coffee, Earthy, Wood, and more
- Custom category icons — choose from preset Material icons or pick from your gallery
- **5 font size levels** — Small to XX-Large, scaled responsively across all screens
- Google Fonts typography

### App Configuration (`lib/core/config/app_defaults.dart`)
- Single file to set **out-of-box defaults** without touching individual providers:
  - Default theme mode & color scheme
  - Default font size & currency
  - Ads enabled state + hard kill-switch (`adsForceDisabled`) that hides the toggle from Settings
  - Auto-backup enabled state and retention count

### Onboarding & Updates
- First-run tutorial and profile setup flow
- **In-app update** support via `in_app_update` — checks Play Store on launch and prompts to install when a flexible update is downloaded

---

## Tech Stack

| Area | Library |
|---|---|
| Framework | [Flutter](https://flutter.dev) (Dart, SDK ≥ 3.10) |
| State Management | [Riverpod](https://riverpod.dev) 2.x (NotifierProvider, StreamProvider, FutureProvider) |
| Database | [Isar Community](https://pub.dev/packages/isar_community) 3.x — high-performance local NoSQL |
| Navigation | [GoRouter](https://pub.dev/packages/go_router) — declarative routing + shell routes |
| Cloud (Split Tracker) | [Supabase](https://supabase.com) — auth, Postgres DB, realtime sync |
| AI / OCR | [Google ML Kit Text Recognition](https://developers.google.com/ml-kit) + custom AI extraction via HTTP |
| Charts | [fl_chart](https://pub.dev/packages/fl_chart) |
| Calendar | [table_calendar](https://pub.dev/packages/table_calendar) |
| Animations | [Lottie](https://pub.dev/packages/lottie) |
| Notifications | [awesome_notifications](https://pub.dev/packages/awesome_notifications) |
| Security | [local_auth](https://pub.dev/packages/local_auth), [flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage), [encrypt](https://pub.dev/packages/encrypt) |
| Export | [pdf](https://pub.dev/packages/pdf), [csv](https://pub.dev/packages/csv), [share_plus](https://pub.dev/packages/share_plus) |
| Home Widgets | [home_widget](https://pub.dev/packages/home_widget) + native Kotlin widget receivers |
| Ads | [google_mobile_ads](https://pub.dev/packages/google_mobile_ads) |
| In-App Updates | [in_app_update](https://pub.dev/packages/in_app_update) |
| Environment | [flutter_dotenv](https://pub.dev/packages/flutter_dotenv) |
| Networking | [http](https://pub.dev/packages/http), [connectivity_plus](https://pub.dev/packages/connectivity_plus) |

---

## Architecture

```
lib/
├── core/
│   ├── config/          # AppDefaults — centralized first-run defaults & feature flags
│   ├── di/              # Global Riverpod providers
│   ├── router/          # GoRouter config & route constants
│   ├── theme/           # AppTheme, ModernColors ThemeExtension
│   ├── services/        # OCR, AI extraction, backup, export, ads, security,
│   │                    #   notifications, gamification, recurring transactions,
│   │                    #   widget bridge
│   ├── constants/       # App-wide constants (colors, dimensions, strings)
│   └── extensions/      # BuildContext, DateTime, Number extensions
├── data/
│   ├── models/          # Isar @collection classes + .g.dart generated files
│   └── repositories/    # Data access layer (one repo per model)
├── presentation/
│   ├── features/        # Feature folders, each with screens/ providers/ widgets/
│   │   ├── home/        # Dashboard — balance card, streak, budget summary, quick actions
│   │   ├── transactions/# Add/edit/list transactions + recurring transactions
│   │   ├── accounts/    # Multi-account management
│   │   ├── categories/  # Full-screen add/edit categories with icon & color pickers
│   │   ├── analytics/   # Charts, filters, date-range selection, export
│   │   ├── calendar/    # Monthly/week calendar with activity heatmap
│   │   ├── budgets/     # Category budgets with period selection
│   │   ├── goals/       # Savings goals with progress
│   │   ├── loans/       # Lent/borrowed money with repayment history
│   │   ├── bill_splitter/ # Local expense splitting with settlement tracking
│   │   ├── people/      # Financial contacts management
│   │   ├── achievements/# Gamification — badges, streaks, milestones
│   │   ├── notifications/ # Notification history and management
│   │   ├── settings/    # Theme, currency, font, security, backup, ads
│   │   ├── auth/        # Biometric lock screen
│   │   ├── onboarding/  # First-run tutorial and profile setup
│   │   └── about/       # App info, help, privacy policy
│   └── widgets/         # Shared widgets — FormSectionCard, FormFieldRow,
│                        #   NativeAdWidget, SuccessAnimation, AppScaler
├── main.dart            # Entry point — DB init, SharedPrefs, notifications, ads,
│                        #   recurring transactions, auto-backup, in-app updates
packages/
└── split_tracker/       # Self-contained Supabase-backed group expense package
    ├── lib/
    │   ├── auth/        # Supabase auth service (lazy init)
    │   ├── data/        # Models, repositories, sync engine
    │   └── presentation/# Screens, providers, widgets for Split Tracker feature
    └── test/
```

**State Management patterns:**
- `StreamProvider` — live Isar queries (auto-updates on DB writes)
- `FutureProvider` — one-shot async data (dashboard summary, category lists)
- `NotifierProvider` — state + logic pairs (theme, currency, security, font size)
- `StateNotifierProvider` — simple boolean state (ads, amounts visibility)

---

## Setup

### Prerequisites
- Flutter SDK ≥ 3.10
- Android Studio or VS Code
- A `.env` file in the project root (required by `flutter_dotenv`)

### Installation

```bash
# 1. Clone
git clone https://github.com/tejnc/spendly.git
cd spendly

# 2. Install dependencies
flutter pub get

# 3. Generate Riverpod + Isar code
dart run build_runner build --delete-conflicting-outputs

# 4. Run
flutter run
```

> After modifying any `@collection` (Isar) or `@riverpod` annotated file, re-run step 3.

### Split Tracker (optional)

Split Tracker requires a Supabase project. Pass credentials at build time via `--dart-define`:

```bash
flutter run \
  --dart-define=SPLIT_TRACKER_SUPABASE_URL=https://<project>.supabase.co \
  --dart-define=SPLIT_TRACKER_SUPABASE_ANON_KEY=<anon-key>
```

For VS Code, add a git-ignored `.vscode/launch.json` with the `--dart-define` args, or use `--dart-define-from-file=secrets.json`. No credentials means Split Tracker is simply disabled — the rest of the app functions normally.

### Build release APK

```bash
flutter build apk --release
```

---

## Roadmap

- [x] **Phase 1** — Core transactions, accounts, categories, analytics
- [x] **Phase 2** — Budgets & savings goals
- [x] **Phase 3** — OCR receipt scanning, biometric lock, encrypted backup
- [x] **Phase 4** — Bill splitting, recurring transactions, loan tracking
- [x] **Phase 5** — Lottie animations, shimmer loading, performance optimizations
- [x] **Phase 6** — People/contacts, achievements & gamification, notification history
- [x] **Phase 7** — Full-screen category editor, auto-backup, in-app updates, app defaults config
- [x] **Phase 8** — Android home screen widgets (Budget, Quick Add, Split Bill), calendar with heatmap, PDF receipts, daily reminders
- [x] **Phase 9** — Cloud-backed Split Tracker (isolated package, privacy-safe) — complete
- [ ] **Phase 10** — iOS release

---

## Privacy Policy

[View Privacy Policy](https://tejnc.github.io/spendly/privacy_policy.html)
