# android-versioncat-stack

> Curated `libs.versions.toml` for modern Android Compose projects. Drop in, build.

[![Status](https://img.shields.io/badge/status-maintained-green)](#bumping-policy)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Last bump](https://img.shields.io/badge/last%20bump-2026--06--10-informational)](gradle/libs.versions.toml)

## Why

Every new Android project starts with the same painful question: *"What versions are still safe to use?"* This repo answers it with one curated `libs.versions.toml` that's:

- ✅ Compose-first (Compose BOM 2024.12.01, no manual ui-x version juggling)
- ✅ Kotlin 2.x ready (Compose compiler plugin instead of legacy `composeOptions`)
- ✅ Hilt + Coroutines + Navigation in known-good combinations
- ✅ Bundles defined so module `build.gradle.kts` stays short
- ✅ Updated quarterly — see [bumping policy](#bumping-policy) below

## Install

Drop the file into your project:

```bash
curl -o gradle/libs.versions.toml \
  https://raw.githubusercontent.com/duonglkh/android-versioncat-stack/main/gradle/libs.versions.toml
```

Or just copy paste from [`gradle/libs.versions.toml`](gradle/libs.versions.toml).

## Usage

In your module `build.gradle.kts`:

```kotlin
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.kotlin.compose)
    alias(libs.plugins.hilt)
    alias(libs.plugins.ksp)
}

android {
    buildFeatures { compose = true }
}

dependencies {
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.bundles.compose)
    debugImplementation(libs.bundles.compose.debug)

    implementation(libs.hilt.android)
    ksp(libs.hilt.compiler)

    implementation(libs.bundles.network)

    testImplementation(libs.bundles.unit.test)
    androidTestImplementation(libs.bundles.android.test)
    androidTestImplementation(platform(libs.androidx.compose.bom))
    androidTestImplementation(libs.bundles.compose.test)
}
```

That's it — no Compose compiler extension chasing, no per-library Compose version drift.

## What's included

| Category | Libraries |
| --- | --- |
| AndroidX core | core-ktx, lifecycle, activity-compose |
| Compose | BOM 2024.12.01 + UI, Material3, Navigation, tooling, test |
| DI | Hilt + hilt-navigation-compose |
| Async | Coroutines core + Android + test |
| Storage | DataStore preferences, Room (KSP) |
| Network | Retrofit + OkHttp + kotlinx-serialization |
| Image | Coil for Compose |
| Test | JUnit, MockK, Turbine, Google Truth, Espresso |

## Bumping policy

This catalog is bumped **once per quarter** and after every major release of:

- Android Gradle Plugin (AGP)
- Kotlin
- Compose BOM

Each bump is a separate commit so you can `cherry-pick` selectively. Releases are tagged `vYYYY.MM.QN` (e.g. `v2026.06.Q2`).

## Versioning

Catalog is sourced as raw file — copy + commit into your project. No transitive dependency. When a bump lands here, you decide when to pull it in.

For projects that want auto-bumps, see [Renovate](https://github.com/renovatebot/renovate)'s `gradle` manager.

## Part of the `android-template-base` ecosystem

This is repo #2 of a planned set. See [hidev.github.io](https://hidev.github.io/#projects) for the full roster.

## License

MIT © 2026 Hidev (Hung Duong)
