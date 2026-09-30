# Kongflix Fork Customization & Upgrade Guide

This document records all custom changes, graphical replacements, theme updates, and UI tweaks made in the **Kongflix** fork relative to upstream Jellyfin for Android TV. Keep this guide handy when rebasing or merging upstream updates.

---

## 1. Custom Color Palette & Theming

The fork uses a custom dark green palette with mint and banana yellow accents:
* `--accent` (`#B8F2C5`): Mint green (used for selected card borders and primary highlights)
* `--accent-off` (`#6D8472`): Sage green (used for unselected card borders and secondary strokes)
* `--accent-yellow` (`#FFE135`): Banana yellow (used for logo gradient highlights)
* `--darkest` (`#131C16`): Deep dark green (app backgrounds, icon backgrounds)
* `--dark` (`#1B241D`): Dark forest green
* `--dark-apparent` (`#1F2921`): Apparent dark green (menus, scrollers)
* `--dark-highlight` (`#3B453C`): Card & button backgrounds

### Modified Files:
* [`app/src/main/res/values/colors.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/values/colors.xml) — Mapped Jellyfin base colors to the custom palette.
* [`app/src/main/res/values/logo.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/values/logo.xml) — Set logo gradient colors (Banana Yellow `#FFE135` $\rightarrow$ Mint `#B8F2C5`) and dark background (`#131C16`).
* [`app/src/main/res/drawable/app_background_gradient.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/app_background_gradient.xml) — Custom multi-stop dark green background gradient.
* [`app/src/main/res/values/theme_jellyfin.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/values/theme_jellyfin.xml) & [`theme_emerald.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/values/theme_emerald.xml) — Pointed `defaultBackground` to `@drawable/app_background_gradient`.

---

## 2. Branding & Vector Graphics (Banana & Kongflix Text)

Custom SVG assets stored in `assets/` were converted into Android VectorDrawables (`.xml`):

### Modified Files:
* [`app/src/main/res/drawable/ic_jellyfin.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/ic_jellyfin.xml) — Replaced with standalone banana icon.
* [`app/src/main/res/drawable/app_logo.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/app_logo.xml) & [`drawable-v24/app_logo.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable-v24/app_logo.xml) — **KONGFLIX** text flanked by banana cutouts with mint-to-yellow gradient.
* [`app/src/main/res/drawable/app_icon_foreground.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/app_icon_foreground.xml) — Adaptive app icon foreground (KONGFLIX text & mint bananas).
* [`app/src/main/res/drawable/app_icon_background.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/app_icon_background.xml) & [`app_banner_background.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/app_banner_background.xml) — Set to `--darkest` green (`#131C16`).
* [`app/src/main/res/drawable/app_banner_foreground.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/drawable/app_banner_foreground.xml) — Android TV launcher banner foreground (KONGFLIX text & mint bananas).

---

## 3. UI, Navigation & Stability Tweaks

### A. Toolbar & Screen Logos
* [`app/src/main/java/org/jellyfin/androidtv/ui/shared/toolbar/MainToolbar.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/shared/toolbar/MainToolbar.kt) — Removed home/search buttons from center, replaced with centered clickable `Logo()` (aspect ratio 210:40 = 5.25:1, takes user home).
* [`app/src/main/java/org/jellyfin/androidtv/ui/shared/toolbar/Toolbar.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/shared/toolbar/Toolbar.kt) — Configured `Logo()` to use Coil's `rememberAsyncImagePainter(R.drawable.app_logo)` for native C++/Java VectorDrawable rendering in Compose.
* [`app/src/main/java/org/jellyfin/androidtv/ui/shared/TitleView.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/shared/TitleView.kt) & [`view_lb_title.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/layout/view_lb_title.xml) — Ensured `app_logo` badge is visible on Leanback browse title views.
* [`app/src/main/res/layout/clock_user_bug.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/layout/clock_user_bug.xml) (`ClockUserView`) — Replaced the generic home house icon (`ic_house`) with `app_logo` across library screens and detail views while preserving home button functionality.

### B. Startup, Player Overlay & Android 9 Stability Fix
* [`app/src/main/java/org/jellyfin/androidtv/ui/startup/fragment/SplashFragment.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/startup/fragment/SplashFragment.kt) & [`DreamContentLogo.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/integration/dream/composable/DreamContentLogo.kt) — Configured cold-start splash screen and screensavers to display centered `ic_jellyfin` banana icon.
* [`app/src/main/java/org/jellyfin/androidtv/ui/player/base/PlayerHeader.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/player/base/PlayerHeader.kt) — Added `Logo()` to the video player controls header.
* [`app/src/main/java/org/jellyfin/androidtv/ui/ClockUserView.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/ClockUserView.kt) — Set `binding.home.isVisible = true` during video playback so the header logo remains visible when media controls appear.
* [`app/src/main/AndroidManifest.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/AndroidManifest.xml) & [`StartupActivity.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/startup/StartupActivity.kt) — Added `android:configChanges` to `StartupActivity` and used `finish()` on activity launch to resolve Android 9 (API 28) `reportSizeConfigurations: ActivityRecord not found` OS crash.

### C. Background Backdrop Restricting
* Prevented dynamic movie backdrop loading on home, browse, and search screens, ensuring backdrops only load on item detail pages:
  * [`app/src/main/java/org/jellyfin/androidtv/ui/home/HomeRowsFragment.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/home/HomeRowsFragment.kt)
  * [`app/src/main/java/org/jellyfin/androidtv/ui/browsing/BrowseFolderFragment.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/browsing/BrowseFolderFragment.kt)
  * [`app/src/main/java/org/jellyfin/androidtv/ui/browsing/BrowseGridFragment.java`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/browsing/BrowseGridFragment.java)
  * [`app/src/main/java/org/jellyfin/androidtv/ui/browsing/EnhancedBrowseFragment.java`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/browsing/EnhancedBrowseFragment.java)
  * [`app/src/main/java/org/jellyfin/androidtv/ui/search/SearchFragmentDelegate.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/search/SearchFragmentDelegate.kt)
* [`app/src/main/java/org/jellyfin/androidtv/ui/background/AppBackground.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/background/AppBackground.kt) — Fixed shape/gradient drawable bitmap conversion dimension parameters (`width=1, height=1`).

### D. Card Borders (Libraries, Movies & TV Shows)
* [`app/src/main/java/org/jellyfin/androidtv/ui/composable/item/ItemCard.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/composable/item/ItemCard.kt) — Added optional `BorderStroke` support.
* [`app/src/main/java/org/jellyfin/androidtv/ui/presentation/CardPresenter.kt`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/java/org/jellyfin/androidtv/ui/presentation/CardPresenter.kt) — Added 2dp border to library, movie, and TV show cards (`#6D8472` off-accent unselected, `#B8F2C5` mint accent selected).

### E. App Branding & Release Packaging
* [`app/src/main/res/values/strings.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/res/values/strings.xml) — Set `app_name_release` and `app_name_debug` to **Kongflix**.
* [`app/src/main/AndroidManifest.xml`](file:///home/marty/jellyfin-androidtv-kongflix/app/src/main/AndroidManifest.xml) — Directed `android:icon` and `android:banner` to `@drawable/app_icon_foreground` and `@drawable/app_banner`.
* [`app/build.gradle.kts`](file:///home/marty/jellyfin-androidtv-kongflix/app/build.gradle.kts) — Configured `archivesName` to `kongflix-v0.0.2`.

---

## 4. Documentation
* [`README.md`](file:///home/marty/jellyfin-androidtv-kongflix/README.md) — Fixed file path, capitalization, punctuation, and trailing whitespace.
