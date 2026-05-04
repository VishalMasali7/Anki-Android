# AnkiDroid - Developer Resources & Architecture

## Project Architecture Overview

```
AnkiDroid (GradleProject)
│
├── 📦 AnkiDroid (Application Module)
│   │   └─ Main Android app targeting Google Play, F-Droid
│   ├─ src/
│   │   ├─ main/        (Production code)
│   │   │   ├─ java/com/ichi2/anki/
│   │   │   ├─ res/     (UI resources, layouts, strings)
│   │   │   └─ AndroidManifest.xml
│   │   ├─ test/        (Unit tests - JUnit 5 + Robolectric)
│   │   ├─ androidTest/ (Instrumented tests - Espresso)
│   │   ├─ play/        (Play Store specific resources)
│   │   ├─ amazon/      (Amazon specific resources)
│   │   └─ testFixtures/ (Shared test utilities)
│   └─ build.gradle     (App-specific dependencies)
│
├── 📚 Common Module
│   ├─ common/          (Kotlin-only, platform-independent)
│   │   └─ src/main/    (Shared business logic)
│   └─ common:android/  (Android-specific utilities)
│
├── 🔌 API Module (LGPL-3.0)
│   └─ src/
│       ├─ main/        (Public API for third-party use)
│       ├─ debug/       (Debug-only implementations)
│       └─ test/        (API tests)
│
├── 🔨 Helper Modules
│   ├─ compat/          (Android version compatibility)
│   ├─ libanki/         (Anki database backend)
│   ├─ lint-rules/      (Custom Lint rules)
│   ├─ vbpd/            (Video playback & download)
│   └─ buildSrc/        (Build-time utilities)
│
├── 📋 Configuration Files
│   ├─ build.gradle.kts (Root build configuration)
│   ├─ settings.gradle.kts (Module definitions)
│   ├─ gradle.properties (Gradle & Java settings)
│   ├─ gradle/libs.versions.toml (Dependency versions)
│   └─ local.properties (Local overrides - GITIGNORED)
│
└── 🛠️ Tools & Documentation
    ├─ tools/          (Scripts: release, testing, translation)
    ├─ fastlane/       (App Store automation)
    ├─ docs/           (Documentation & assets)
    ├─ .github/        (GitHub Actions CI/CD)
    └─ pre-commit      (Git hook for code formatting)
```

---

## Module Dependencies

```
┌─────────────────────────────────────────────────┐
│          AnkiDroid (Main Application)            │
│  - UI Activities & Fragments                     │
│  - User interactions & business logic            │
│  - Database management                           │
│  - AnkiWeb synchronization                       │
└────────────┬────────────────────────────────────┘
             │ depends on
             ├─→ ┌──────────────────────────────┐
             │   │  Common Module (Shared Code) │
             │   │  - Utilities                 │
             │   │  - Data structures           │
             │   │  - Algorithms                │
             │   └─────────────────────────────┘
             │           ↓
             ├─→ ┌──────────────────────────────┐
             │   │  Common:Android              │
             │   │  - Android utilities         │
             │   └─────────────────────────────┘
             │
             ├─→ ┌──────────────────────────────┐
             │   │  LibAnki                     │
             │   │  - Database operations       │
             │   │  - Anki backend              │
             │   └─────────────────────────────┘
             │
             ├─→ ┌──────────────────────────────┐
             │   │  Compat                      │
             │   │  - Version compatibility     │
             │   └─────────────────────────────┘
             │
             └─→ ┌──────────────────────────────┐
                 │  VBPD                        │
                 │  - Video playback            │
                 └─────────────────────────────┘

External Dependencies:
├─ Anki Backend (ankiBackend) - v0.1.64
├─ AndroidX (androidx.*)
├─ Material Design
├─ Coroutines
├─ OkHttp
├─ SQLite
└─ Various utility libraries
```

---

## Key File Locations

### Build Configuration
| File | Purpose | Location |
|------|---------|----------|
| `build.gradle.kts` | Root build script | `/` |
| `settings.gradle.kts` | Module includes | `/` |
| `gradle.properties` | Gradle system config | `/` |
| `gradle/libs.versions.toml` | Dependency versions | `/gradle/` |
| `local.properties` | Local overrides | `/` (GITIGNORED) |

### Application Code
| Path | Purpose |
|------|---------|
| `AnkiDroid/src/main/java/com/ichi2/anki/` | App source code |
| `AnkiDroid/src/main/res/` | UI resources (layouts, strings, etc.) |
| `AnkiDroid/src/test/` | Unit tests |
| `AnkiDroid/src/androidTest/` | Instrumented tests |
| `common/src/main/` | Shared business logic |

### Build Outputs
| Path | Contents |
|------|----------|
| `AnkiDroid/build/outputs/apk/` | Built APK files |
| `AnkiDroid/build/reports/` | Test reports, lint results |
| `AnkiDroid/build/intermediates/` | Build intermediates |
| `build/` | Root build output |

---

## Development Tools & Plugins

### Gradle Plugins Used

1. **Android Gradle Plugin (AGP)** v9.0.1
   - Builds Android applications
   - Manages signing, packaging
   - Configures build variants

2. **Kotlin Gradle Plugin** v2.3.21
   - Compiles Kotlin source code
   - Provides @Parcelize for data classes
   - Enables serialization support

3. **KTlint Gradle Plugin** v14.2.0
   - Code formatting enforcement
   - Git pre-commit hooks
   - Auto-fix capability

4. **Keeper Plugin** v0.16.1
   - Verifies ProGuard/R8 optimization
   - Ensures no symbol stripping

5. **Triple Play Plugin** v4.0.0
   - Automates Google Play Store publishing
   - Manages release notes

6. **Roborazzi Plugin** v1.60.0
   - Screenshot testing
   - Visual regression testing

---

## Important Gradle Tasks

### Run & Build
```bash
./gradlew assembleDebug        # Build debug APK
./gradlew assembleRelease      # Build release APK
./gradlew installDebug         # Install to device
./gradlew runDebugUnit Emulator # Run on emulator
```

### Testing
```bash
./gradlew test                 # Unit tests
./gradlew connectedAndroidTest # Instrumented tests
./gradlew testDebug            # Debug variant tests
./gradlew jacocoTestReport     # Code coverage
```

### Code Quality
```bash
./gradlew ktlintCheck          # Check formatting
./gradlew ktlintFormat         # Auto-format
./gradlew lint                 # Android lint
./gradlew check                # All checks + tests
```

### Utilities
```bash
./gradlew dependencies         # Dependency tree
./gradlew tasks                # List all tasks
./gradlew --scan build         # Build scan (detailed)
./gradlew addKtlintFormatGitPreCommitHook  # Install git hook
```

---

## Testing Framework Setup

### Unit Testing (JUnit 5 + Robolectric)
- **Framework:** JUnit 5 (Jupiter)
- **Mocking:** MockK, Mockito
- **Android Simulation:** Robolectric 4.16.1
- **Location:** `src/test/java/**`
- **Command:** `./gradlew test`

Example test pattern:
```kotlin
class CalculationTest {
    @Test
    fun shouldCalculateCorrectly() {
        val result = calculate(2, 3)
        assertEquals(5, result)
    }
}
```

### Instrumented Testing (Espresso)
- **Framework:** Espresso 3.7.0
- **Target:** Running on Android device/emulator
- **Location:** `src/androidTest/java/**`
- **Command:** `./gradlew connectedAndroidTest`

Example test pattern:
```kotlin
@RunWith(AndroidJUnit4::class)
class MainActivityTest {
    @get:Rule
    val activityScenarioRule = activityScenarioRule<MainActivity>()

    @Test
    fun shouldDisplayContent() {
        onView(withId(R.id.content)).check(matches(isDisplayed()))
    }
}
```

### Test Configuration
- **Settings:** `gradle.properties`
  - Heap size: 1-2GB
  - Android resources included
  - Parallel execution enabled
  - JUnit 5 Platform enabled

---

## Code Style & Quality Standards

### Kotlin Style Guide
- **Tool:** ktlint 1.8.0
- **Configuration:** Automatic via gradle plugin
- **Auto-fix:** `./gradlew ktlintFormat`
- **Check:** `./gradlew ktlintCheck`

Key rules enforced:
- 4-space indentation
- No wildcard imports
- Trailing commas
- Line length limits
- Naming conventions

### Compiler Configuration
- **Kotlin Version:** 2.3.21
- **Warnings as Errors:** Enabled by default
- **Explicit Backing Fields:** Enabled
- **Coroutines Opt-in:** Enabled
- **Context Parameters:** Enabled

### Best Practices
1. Use data classes for models
2. Use sealed classes for type hierarchies
3. Prefer immutable properties (`val`)
4. Use extension functions for utilities
5. Leverage coroutines for async work
6. Write unit tests for business logic

---

## Dependency Management

### Version Catalog (Recommended)
Located in: `gradle/libs.versions.toml`

Usage in `build.gradle.kts`:
```kotlin
dependencies {
    implementation(libs.androidx.appcompat)
    implementation(libs.kotlin.coroutines)
    testImplementation(libs.junit.jupiter)
}
```

### Adding New Dependency
1. Add version to `gradle/libs.versions.toml` under `[versions]`
2. Define library in `[libraries]` section
3. Reference in module `build.gradle`

Example:
```toml
[versions]
mylib = "1.2.3"

[libraries]
my-library = { module = "com.example:mylib", version.ref = "mylib" }
```

Then use:
```kotlin
dependencies {
    implementation(libs.my.library)  // Hyphens become dots
}
```

---

## Continuous Integration

### GitHub Actions
- **CI File:** `.github/workflows/`
- **Triggers:** Push, Pull Request
- **Steps:** Build, Test, Lint, Coverage

### Local Quality Checks
Before push, run:
```powershell
./gradlew clean check ktlintFormat
git add .
git commit -m "Your message"
```

---

## Performance Optimization

### Gradle Build Optimization
- **Configuration Cache:** Enabled
- **Parallel Builds:** Enabled
- **VFS Watching:** Enabled
- **Build Caching:** Enabled

See `gradle.properties` for settings.

### App Size Optimization
- **ProGuard:** Configured for release
- **R8:** Modern desugaring enabled
- **Keeper:** Verifies optimization

### Memory Settings
```properties
org.gradle.jvmargs=-Xmx3072M -Dfile.encoding=UTF-8
```
Increase to `-Xmx4096M` if needed for large projects.

---

## Common Issues Reference

### Gradle Cache Issues
```powershell
./gradlew clean --no-build-cache
./gradlew build
```

### Dependency Resolution
```powershell
./gradlew dependencies --scan
./gradlew --refresh-dependencies build
```

### IDE Sync Problems
```powershell
./gradlew sync
# Or in Android Studio: File → Invalidate Caches → Invalidate and Restart
```

### Test Failures
```powershell
./gradlew test --info
./gradlew test --info --tests "com.ichi2.anki.ClassName"
```

---

## Resources for Developers

### Official Documentation
- **Kotlin:** https://kotlinlang.org/
- **Gradle:** https://docs.gradle.org/
- **Android:** https://developer.android.com/
- **AndroidX:** https://developer.android.com/jetpack/androidx
- **JUnit 5:** https://junit.org/junit5/

### AnkiDroid Specific
- **Repository:** https://github.com/ankidroid/Anki-Android
- **Wiki:** https://github.com/ankidroid/Anki-Android/wiki
- **Development Guide:** https://github.com/ankidroid/Anki-Android/wiki/Development-Guide
- **Discord:** https://discord.gg/xeb7bBZVJ6

### Testing Frameworks
- **Robolectric:** https://robolectric.org/
- **Espresso:** https://developer.android.com/training/testing/espresso
- **MockK:** https://mockk.io/

### Build Tools
- **ktlint:** https://ktlint.github.io/
- **ProGuard:** https://www.guardsquare.com/proguard
- **Keeper:** https://github.com/slackhq/keeper

---

## Next Steps

1. ✅ **Understand the architecture** - Review this document
2. ✅ **Check prerequisites** - Java 21+, Android SDK
3. 🔨 **Build the project** - `./gradlew clean build`
4. 🧪 **Run tests** - `./gradlew test`
5. 🎯 **Open in IDE** - Android Studio
6. 💻 **Start coding** - Create feature branch
7. 📝 **Follow conventions** - ktlint auto-formats
8. 🧑‍💼 **Create PR** - Submit to upstream

---

**Status:** ✅ Ready when Java 21+ is installed

*Last Updated: May 4, 2026*

