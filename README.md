# TapLedger (Quick Expense)

> **100% offline-first, privacy-focused Android expense tracker built around `< 3 second` expense capture.**  
> Repository hosts documentation and packaged APK releases only. Download the latest Android APK from **[Releases](https://github.com/Sonawanemayur-IT/TapLedger/releases)**.

---

## Highlights

- **Instant `< 3s` Expense Capture** — Trigger quick entry from anywhere with a double-press of your Volume button, Quick Settings Tile, Persistent Notification, Home Screen shortcut, or the in-app `+` button.
- **100% Offline & Private** — Zero `INTERNET` permission in `AndroidManifest.xml`, no accounts, no cloud sync, no ads, and no analytics SDKs. Every rupee stays on your device.
- **Cash vs. Digital & Cash Wallet** — Track `Cash`, `UPI`, `Debit Card`, `Credit Card`, and `Other` separately, plus a dedicated Cash Wallet balance (`Starting Cash + Cash Additions − Cash Expenses`) that keeps ATM withdrawals and cash gifts separate from spending.
- **Did You Know? Earning Targets** — Translates your last 30 calendar days of spending into understandable monthly, daily, working-day (`22 days/mo`), and hourly (`176 hours/mo`) take-home income estimates.
- **Insights & Budget Alerts** — Daily / Weekly / Monthly charts, Month-to-Month same-day comparisons, Cash vs. Digital breakdown, highest spending days, local recurring-expense pattern suggestions, and `75% / 90% / 100%` monthly budget alerts.
- **Smart Local Helpers** — Note-based category suggestions learned strictly from your own confirmed expenses, non-blocking 2-minute duplicate warnings, personalized evening check-in reminders respecting quiet hours, and full **CSV** export + **JSON** backup/restore.

---

## Download & Install (Android 8.0+ / API 26+)

> **App Drawer Name:** After installing, the app appears in your launcher as **Quick Expense** (package ID `com.quickexpense.app.debug` for debug builds / `com.quickexpense.app` for release builds, version `1.0.0`).

1. Open the **[Releases](https://github.com/Sonawanemayur-IT/TapLedger/releases)** page and download the `.apk` asset from the latest release.
2. Open the downloaded `.apk` on your Android phone and allow **Install unknown apps** for your browser or file manager if prompted.
3. Launch **Quick Expense** from your app drawer.

### Recommended Setup on OnePlus / OxygenOS 16 (and Android 13+)

1. Allow **Notifications** when prompted so heads-up triggers, check-in reminders, and budget alerts work.
2. Open **Settings → Volume Button Shortcut** inside the app to enable double-press **Volume Down** (or **Volume Up**) quick capture.
3. Complete the built-in **OxygenOS 16 & Permission Setup** checklist:
   - Enable **Banners** for the *Quick Expense Shortcut* notification channel.
   - Enable **Quick Expense Volume Shortcut** in Android Accessibility settings (optional; single volume presses still pass through normally).
   - Set **Battery Usage** to **Unrestricted / Allow background activity** so the OS does not suspend the volume shortcut listener.
   - If you switch Trigger Experience from *Heads-Up Banner* to *Overlay Panel*, grant **Display over other apps**.
4. *(No Accessibility option)* You can still trigger quick entry anytime via the **Quick Settings Tile** (`Quick Expense`), **Persistent Quick-Add Notification**, or the pinned **Quick Add** home screen shortcut.

---

## Core Features in Detail

### 1. Instant Quick-Entry Triggers

| Trigger | How It Works | Requires Accessibility? |
| :--- | :--- | :--- |
| **Volume Double-Press** | Double-press **Volume Down** or **Volume Up** within a configurable `250 ms – 700 ms` window (`850 ms` cooldown guard, optional haptic pulse). Single presses change media/ring volume normally. | Yes (`canRetrieveWindowContent="false"`) |
| **Heads-Up Banner + Inline Log** | Default trigger experience: posts a high-priority heads-up banner with **Open Quick Panel** and **Quick Log** inline notification reply (type `50` or `50 tea` right inside the notification), plus confirmation with **Undo**. | Notification permission only |
| **Floating Overlay Panel** | Optional direct floating panel (`QuickEntryOverlayService`) displayed over any app. | Requires *Display over other apps* |
| **Quick Settings Tile** | Pull down Android Quick Settings and tap **Quick Expense**. | No |
| **Persistent Indicator** | Ongoing low-priority notification displaying today's spend and a 1-tap Quick Entry button. | No |
| **Home Screen Shortcut** | Long-press the app icon → **Quick Add** (`Log Quick Expense`). | No |

### 2. Fast Numeric Popup (`QuickEntryPopupCard`)

- **Auto-Focused Decimal Input**: Numeric keypad opens immediately (~80 ms) with live digit grouping (`1,00,000.5` Indian 3-2-2 grouping for `INR`, 3-digit grouping for other currencies).
- **One-Tap Categories**: Top 8 categories sorted by usage frequency and recency (expandable to all active categories), pre-selecting your `lastUsedCategoryId`.
- **Local Note Suggestions**: Typing a note (minimum 2 characters) suggests a matching category strictly from your own previously confirmed expenses with notes.
- **Payment Modes**: Choose `Cash`, `UPI`, `Debit Card`, `Credit Card`, or `Other` (remembers `lastUsedPaymentMode`, defaulting to `Cash`).
- **Non-Blocking Safety Checks**:
  - Alerts if a matching `(amount, category, payment mode)` entry was logged within the last **2 minutes** (never blocks saving).
  - Warns if a `Cash` expense will push your Cash Wallet balance below zero.
- **Instant Undo**: Confirmation banner counts down for `2.8 seconds` with a one-tap **Undo** button that deletes the entry and restores focus.

### 3. Five Main Screens

1. **Overview (`Home`)**
   - Live **Shortcut Ready / Setup Shortcut** indicator chip linking directly to Settings.
   - **Today's Spending** hero card (tabular numerals `tnum`, entry count, **Quick** button) alongside **This Week** and **This Month** totals.
   - **Cash Wallet** summary card with **Set Starting Cash**, **Add Cash**, and **Cash Ledger** controls.
   - Unspecified payment-mode cleanup prompt with bulk assignment dialog (`Cash`, `UPI`, `Debit Card`, `Credit Card`, `Other`).
   - **Monthly Budget** progress bar (switches smoothly between accent, warning at `>= 75%`, and danger at `>= 100%`).
   - **14-Day Spending Trend** bar chart highlighting today and peak day.
   - **Category Breakdown** (top 5 for the current month) and **Recent Expenses** with source badges (`Volume`, `Notification`, `Tile`, `Shortcut`) and tap-to-edit.

2. **Ledger (`History`)**
   - Full-text search across notes, category names, and payment mode labels.
   - Multi-axis filter chips: **Date** (`All`, `Today`, `This Week`, `This Month`), **Category**, and **Payment Mode**.
   - Sort cycling (**Newest**, **Oldest**, **Highest**).
   - Sticky-style date headers (`TODAY`, `YESTERDAY`, formatted dates) with daily totals, tap-to-edit dialog, and delete with Snackbar **Undo**.

3. **Did You Know? (`Earning Targets`)**
   - Consumes the trailing **30 calendar days** of recorded expenses in your local timezone (excluding Cash Additions by design).
   - Computes four transparent take-home earning equivalents using standard `HALF_UP` `BigDecimal` arithmetic:
     - **Monthly Target** (trailing 30-day net spend)
     - **Daily Target** (`Monthly Target ÷ 30 calendar days`)
     - **Working-Day Target** (`Monthly Target ÷ 22 working days/month`)
     - **Hourly Target** (`Monthly Target ÷ 176 working hours/month`, assuming `22 days × 8 hours/day`)
   - Shows **Preliminary Estimate** notices when history spans under 30 days or fewer than 5 entries, plus expandable calculation breakdowns.

4. **Insights (`Analytics`)**
   - **Avg. Daily (MTD)** and **Top Category** summary cards.
   - **Month-to-Month Same-Day Range Comparison** (compares days `1..N` of the current month against `1..N` of the previous month).
   - **Cash vs. Digital Split** (`Cash` vs. `UPI + Debit Card + Credit Card`, keeping `Other` and `Unspecified` separate).
   - **Payment Modes Breakdown** with per-mode counts, totals, and percentages.
   - Interactive **Daily (14d) / Weekly (6w) / Monthly (6m)** bar chart with period total and peak callout.
   - **Monthly Budget** editor with configurable `75%`, `90%`, and `100%` notification alerts.
   - **Highest Spending Days**, **Category Breakdown** (with per-category budget progress bars), and read-only **Recurring Expense Suggestions** (detects `>= 3` consistent Weekly, Biweekly, or Monthly entries sharing a category and note).

5. **Settings & Category Management**
   - **14 Pre-Seeded Categories** (`Food & Dining`, `Groceries`, `Transportation`, `Shopping`, `Stationery`, `Education`, `Entertainment`, `Health`, `Bills & Utilities`, `Rent`, `Travel`, `Personal Care`, `Subscriptions`, `Other`) with full create, edit, reorder, archive/restore, icon picker (`15` icons), `10`-color palette, and optional per-category monthly limits.
   - **Gentle Daily Check-In Reminders**: Scheduled via `WorkManager` in your configured evening window (default `7:30 PM – 9:30 PM`), skipping quiet hours (default `10:30 PM – 7:00 AM`), rotating check-in prompts, supporting **Snooze** (`30 min` default) and **Dismiss**, rescheduling automatically on device boot/timezone change, and personalizing reminder timing locally once `>= 7` timing signals exist.
   - **End-of-Day & Weekly Summaries**: Optional `9:00 PM` daily summary and weekly recap notifications.
   - **Themes & Currencies**: **System**, **Light**, and **Dark** themes in the **Warm Graphite + Soft Amber** visual identity (`#121110` / `#E5A93C` dark, `#F5F3EE` / `#B4690E` light); supports `INR ₹` (default, `Asia/Kolkata`), `USD $`, `EUR €`, and `GBP £`.
   - **Backup & Export (`BackupCodec` v2)**:
     - **CSV** export & share (`EXPENSE`, `CASH_STARTING`, and `CASH_ADDITION` rows).
     - **JSON** backup & restore for categories, expenses, budgets, settings, cash wallet configuration, and cash additions with UID-based duplicate skipping.

---

## Cash Wallet Mechanics

The Cash Wallet computes cash-in-hand purely from persisted records rather than mutable running counters:

```text
Cash Balance = Starting Cash
             + SUM(Cash Additions dated >= As-Of Date)
             − SUM(Cash Expenses dated >= As-Of Date)
```

- Cash additions support **ATM withdrawal**, **Gift**, and **Other** sources with optional notes and never appear in expense charts or `Did You Know?` totals.
- Non-cash modes (`UPI`, `Debit Card`, `Credit Card`, `Other`, `Unspecified`) never alter your Cash Wallet balance.

---

## Privacy & Permissions

| Permission | Purpose | Required? |
| :--- | :--- | :--- |
| `INTERNET` | **Intentionally omitted** — zero network access. | Never requested |
| `POST_NOTIFICATIONS` | Heads-up quick triggers, daily check-ins, summaries, and budget alerts. | Optional (recommended) |
| `BIND_ACCESSIBILITY_SERVICE` | Detects volume button double-presses when enabled; sets `canRetrieveWindowContent="false"` and never reads screen content. | Optional |
| `SYSTEM_ALERT_WINDOW` + `FOREGROUND_SERVICE_SPECIAL_USE` | Direct floating popup overlay mode and persistent quick-add shade indicator. | Optional |
| `RECEIVE_BOOT_COMPLETED` | Restores scheduled WorkManager reminders and summaries after reboot or app updates. | Automatic |
| `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | Shortcut for OnePlus / OxygenOS users to set battery usage to Unrestricted. | Optional |
| `VIBRATE` | Subtle haptic feedback on double-press and expense save. | Automatic |

- Local storage uses Room SQLite (`quick_expense.db`, schema v5) with `android:allowBackup="false"`.

---

## Technical Specifications

- **Platform:** Native Android (`minSdk 26`, `compileSdk 35`, `targetSdk 35`)
- **Language & UI:** Kotlin `2.0.21`, Jetpack Compose (BOM `2024.11.00`), Material 3, Material Icons Extended
- **Persistence & Background Work:** Room `2.6.1` (KSP), WorkManager `2.10.0`, Kotlin Coroutines `1.9.0`
- **Build Info:** AGP `8.7.3`, Gradle `8.10.2`, Java 17 bytecode, Version `1.0.0` (`versionCode = 1`)
