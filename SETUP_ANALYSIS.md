# AnkiDroid Application - Setup Analysis & Guide

**Generated:** May 4, 2026  
**Project Type:** Android Mobile Application  
**Build System:** Gradle (Kotlin DSL)

---

## 1. Project Overview

**AnkiDroid** is a semi-official port of the open-source Anki spaced repetition flashcard system to Android. It's a mature, well-structured project with multiple modules, comprehensive testing, and strict code quality standards.

### Repository Structure
```
AnkiDroid/ (root)
├── AnkiDroid/          # Main application module
├── api/                # AnkiDroid API module (LGPL-3.0)
├── common/             # Shared code module
├── common:android/     # Android-specific shared code
├── compat/             # Compatibility module
├── libanki/            # Anki backend module
├── lint-rules/         # Custom lint rules
├── vbpd/               # Video playback download module
├── buildSrc/           # Build-time plugins and utilities
├── gradle/             # Gradle wrapper and version catalog
└── tools/              # Development scripts and utilities
```

### Key Features
- Night mode & whiteboard support
- Statistics and progress tracking
- AnkiWeb synchronization
- Text-to-speech integration
- Spaced repetition with AI-optimized FSRS algorithm
- Multi-format content support (text, images, sounds, MathJax)
- Available on Google Play Store, F-Droid, and Obtainium

---

## 2. Technology Stack

### Core Technologies
| Component | Version | Details |
|-----------|---------|---------|
| **Kotlin** | 2.3.21 | Primary programming language |
| **Android Gradle Plugin (AGP)** | 9.0.1 | Build system for Android |
| **Gradle** | 9.5.0 | Build automation tool |
| **Java** | JDK 21-25 | Required (currently missing: only JDK 17) |
| **Min API Level** | 24 (Android 7.0) | Minimum supported Android version |
| **Target API Level** | 35 (Android 15) | Latest Android version support |
| **Compile SDK** | 36 | Current compilation target |

### Framework & Libraries
- **androidx** - AndroidX libraries for compatibility
- **Material Design** - Material components library v1.13.0
- **Coroutines** - kotlinx.coroutines v1.10.2 for async operations
- **OkHttp** - HTTP client v5.3.2
- **Anki Backend** - Custom backend v0.1.64-anki25.09.2

### Testing Framework
- **JUnit 5** - v6.0.3 for unit testing
- **Robolectric** - v4.16.1 for Android testing
- **Espresso** - v3.7.0 for UI testing
- **MockK** - v1.14.9 for mocking
- **Turbine** - v1.2.1 for coroutine flow testing

### Code Quality & Style
- **ktlint** - v1.8.0 (Kotlin linter)
- **Keeper** - v0.16.1 (R8 optimization verification)
- **JaCoCo** - v0.8.14 (Code coverage)

### Build Configuration
- **compileSdk:** 36
- **targetSdk:** 35
- **minSdk:** 24

---

## 3. System Requirements

### ⚠️ **CRITICAL ISSUE: Java Version Mismatch**

**Current Status:** ❌ SETUP NOT READY
- **Installed Java:** JDK 17.0.8 LTS
- **Required Java:** JDK 21-25
- **Action Required:** Upgrade to compatible JDK version

**Solution:**
Download and install JDK 21 or later from:
- Official: https://www.oracle.com/java/technologies/downloads/
- OpenJDK: https://jdk.java.net/
- Eclipse Temurin: https://adoptium.net/
- Azul Zulu: https://www.azul.com/downloads/

After installation, update Android Studio settings:
- **Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JDK** → Select JDK 21+

### Software Requirements

| Requirement | Status | Details |
|-------------|--------|---------|
| **JDK 21-25** | ❌ Missing | Need to upgrade from JDK 17 |
| **Android SDK** | ✅ Configured | Located at `C:\Users\Vishal\AppData\Local\Android\Sdk` |
| **Gradle Wrapper** | ✅ Present | Version 9.5.0 configured |
| **Git** | ✅ Required | For version control and operations |
| **Git Hooks** | 🔧 Pre-commit | ktlint & Prettier hooks configured |

### Hardware Requirements
- **RAM:** 8GB minimum (16GB recommended for builds)
- **Disk Space:** 10GB+ for SDK, build artifacts, and cache
- **CPU:** Multi-core processor recommended

---

## 4. Project Configuration

### Gradle Configuration

**Build Properties** (`gradle.properties`):
```properties
org.gradle.jvmargs=-Xmx3072M -Dfile.encoding=UTF-8
android.useAndroidX=true
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.configuration-cache=true
org.gradle.vfs.watch=true
```

**Local Configuration** (`local.properties`):
```properties
sdk.dir=C:\Users\Vishal\AppData\Local\Android\Sdk
```

### Code Quality Standards

**Project enforces:**
1. **ktlint** - Kotlin code formatting (auto-fixed on commit)
2. **Keeplint** - R8 optimization verification
3. **Fatal warnings** - Compiler warnings treated as errors
4. **Explicit backing fields** - Modern Kotlin compiler features
5. **Configuration cache** - Faster builds

### Pre-commit Hooks

Located in `.git/hooks/pre-commit`:
1. **ktlint** - Auto-formats Kotlin files before commit
2. **Prettier** - Auto-formats JavaScript files (optional, requires npm)

---

## 5. Module Description

### AnkiDroid (Main App Module)
- Primary application module
- Contains UI, business logic, and database integration
- Uses AGP application plugin
- ProGuard rules for release builds
- Robolectric support for testing

### Common & Common:Android
- Shared utilities and base classes
- Platform-independent (common) and Android-specific code
- Accessible to all other modules

### API Module
- **License:** LGPL-3.0
- Public API for third-party integrations
- Stable API surface

### Compatibility (compat) Module
- Compatibility layer for older Android versions
- Handles version-specific implementations

### LibAnki
- Anki backend integration
- Database operations
- Spaced repetition algorithm

### Lint Rules
- Custom Android Lint rules
- Project-specific code quality checks

### VBPD (Video Playback Download)
- Video download and playback support
- ProGuard rules included

### BuildSrc
Custom Gradle plugins and build utilities:
- `TestSummaryService` - GitHub Actions test reporting
- `GitHubActionsTestListener` - CI integration
- Custom build configuration

---

## 6. Setup Checklist

### Phase 1: Environment Setup (CRITICAL - Must complete first)

- [ ] **Upgrade Java to JDK 21+**
  - Download from https://adoptium.net/
  - Install and verify: `java -version`
  - Update Android Studio Gradle JDK setting

- [ ] **Verify Android SDK**
  ```powershell
  $env:ANDROID_SDK_ROOT
  # Should output: C:\Users\Vishal\AppData\Local\Android\Sdk
  ```

- [ ] **Verify Git**
  ```powershell
  git --version
  ```

### Phase 2: Project Setup

- [ ] **Clone repository (if not already done - appears already cloned)**
  ```powershell
  cd C:\Users\Vishal\Documents\projects\Anki-Android
  ```

- [ ] **Install pre-commit hooks**
  ```powershell
  cd .git/hooks
  # Copy pre-commit hook (already exists)
  # Make executable: chmod +x pre-commit
  ```

- [ ] **Configure Git user** (if not already set)
  ```powershell
  git config user.email "your.email@github.com"
  git config user.name "Your Name"
  ```

- [ ] **Initial Gradle sync**
  ```powershell
  .\gradlew --version
  .\gradlew clean
  ```

### Phase 3: Dependency Resolution

- [ ] **Download Robolectric dependencies**
  ```powershell
  .\gradlew --scan
  ```

- [ ] **Verify all dependencies**
  ```powershell
  .\gradlew dependencies
  ```

### Phase 4: Build Validation

- [ ] **Build Debug APK**
  ```powershell
  .\gradlew assembleDebug
  ```

- [ ] **Run Unit Tests**
  ```powershell
  .\gradlew test
  ```

- [ ] **Run Lint Checks**
  ```powershell
  .\gradlew ktlintCheck
  .\gradlew lint
  ```

### Phase 5: IDE Setup

- [ ] **Open in Android Studio**
  - File → Open → Select project root
  - Wait for Gradle sync to complete

- [ ] **Configure Run Configuration**
  - Select emulator or connected device
  - Run → Run 'AnkiDroid'

---

## 7. Important Notes for Contributors

### ⚠️ AI Policy Notice

**NEW CONTRIBUTORS BEWARE:**

AnkiDroid has a **strict AI policy**. Before you have 3 merged PRs:
- ❌ **Cannot use AI tools** to produce code, documentation, or comments
- ❌ **Cannot use AI for grammar corrections** in PRs
- ⚠️ **Violations result in warnings and potential bans**

See: [AI_POLICY.md](AI_POLICY.md)

**If accepted contributors use AI:**
- Must disclose in commit message using `Assisted-by:` trailer
- Must explain tool and version used
- Example:
  ```
  docs: example title
  
  GPT-5.2 implemented `complexMethod`
  
  Assisted-by: GPT-5.2
  ```

### Development Guidelines

1. **Branch Workflow:**
   ```powershell
   git checkout main
   git pull upstream main
   git checkout -b feature/my-feature
   ```

2. **Before Commit:**
   - Run: `.\gradlew check`
   - Pre-commit hooks auto-format ktlint issues
   - Add formatted files: `git add .`

3. **Code Standards:**
   - **Kotlin 2.3.21** with modern features
   - **Explicit backing fields** enabled
   - **Nullable handling** with proper annotations
   - **Coroutines** for async operations

4. **Testing:**
   - JUnit 5 for unit tests
   - Robolectric for component testing
   - Espresso for UI testing
   - Required: `@Test` annotation with visibility

5. **Git Practices:**
   - Use `git rebase` (no merge commits)
   - Each commit should compile independently
   - Clean up history before PR
   - Use `git push --force` to update feature branches

---

## 8. Gradle Commands Reference

### Basic Commands

```powershell
# Clean build
.\gradlew clean

# Build debug APK
.\gradlew assembleDebug

# Build release APK
.\gradlew assembleRelease

# Run on connected device
.\gradlew installDebug

# Uninstall from device
.\gradlew uninstallDebug
```

### Testing

```powershell
# Run all unit tests
.\gradlew test

# Run specific test class
.\gradlew test --tests "com.ichi2.anki.SomeTest"

# Generate code coverage report
.\gradlew jacocoTestReport

# Run instrumented tests (Android Test)
.\gradlew connectedAndroidTest
```

### Code Quality

```powershell
# Run ktlint checks
.\gradlew ktlintCheck

# Fix ktlint issues automatically
.\gradlew ktlintFormat

# Run lint analysis
.\gradlew lint

# Run all checks (ktlint + lint + tests)
.\gradlew check
```

### Build Analysis

```powershell
# View dependency tree
.\gradlew dependencies

# View task tree
.\gradlew tasks

# Run with build scan (detailed analysis)
.\gradlew --scan build

# Check build performance
.\gradlew buildEnvironment
```

### Development

```powershell
# Generate ktlint git pre-commit hook
.\gradlew addKtlintFormatGitPreCommitHook

# Sync gradle configuration
.\gradlew sync

# Refresh dependencies
.\gradlew --refresh-dependencies
```

---

## 9. Common Issues & Solutions

### Issue 1: Java Version Error
**Error:** `ERROR: AnkiDroid builds with JVM versions between 21 and 25. Incompatible major version detected: '17'`

**Solution:**
1. Install JDK 21 or later
2. Update Android Studio: Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JDK
3. Run: `.\gradlew clean`

### Issue 2: Android SDK Not Found
**Error:** `Unable to find SDK`

**Solution:**
- Verify `sdk.dir` in `local.properties`
- Should be: `sdk.dir=C:\Users\Vishal\AppData\Local\Android\Sdk`
- Download missing SDK levels from Android Studio SDK Manager

### Issue 3: Kotlin Compilation Errors
**Error:** Unsafe flag issues or explicit backing field warnings

**Solution:**
- Run: `.\gradlew clean build`
- Invalidate IDE cache: File → Invalidate Caches → Invalidate and Restart
- Update to Android Studio latest version

### Issue 4: Gradle Build Cache Issues
**Error:** Stale cache preventing builds

**Solution:**
```powershell
.\gradlew clean --no-build-cache
.\gradlew build
```

### Issue 5: Out of Memory Error
**Error:** `java.lang.OutOfMemoryError: GC overhead limit exceeded`

**Solution:**
- Current JVM args allow 3072M heap
- Increase if needed in `gradle.properties`:
  ```properties
  org.gradle.jvmargs=-Xmx4096M -Dfile.encoding=UTF-8
  ```

---

## 10. Next Steps

### Immediate Actions Required

1. **[CRITICAL] Upgrade Java JDK to 21+** ← DO THIS FIRST
2. Verify Android SDK installation
3. Test Gradle wrapper: `.\gradlew --version`
4. Clone/update repository and run: `.\gradlew clean build`

### For Development

1. Open in Android Studio
2. Let IDE sync Gradle configuration
3. Create feature branch: `git checkout -b feature/your-feature`
4. Make changes following code standards
5. Run: `.\gradlew check` before committing
6. Push and create Pull Request

### For Code Review

- Check [CONTRIBUTING.md](CONTRIBUTING.md) for PR requirements
- Verify commits compile independently
- Check code quality: no warnings as errors 
- Write tests for new functionality
- Use `git rebase` for clean history

---

## 11. Useful Resources

### Official Documentation
- **Main Repository:** https://github.com/ankidroid/Anki-Android
- **Development Guide:** https://github.com/ankidroid/Anki-Android/wiki/Development-Guide
- **Wiki:** https://github.com/ankidroid/Anki-Android/wiki
- **Issues:** https://github.com/ankidroid/Anki-Android/issues

### Community
- **Discord:** https://discord.gg/xeb7bBZVJ6
- **Reddit:** https://www.reddit.com/r/Anki
- **Forums:** https://forums.ankiweb.net/

### Build Tools & Technologies
- **Gradle Documentation:** https://docs.gradle.org/
- **Android Gradle Plugin:** https://developer.android.com/build/releases/gradle-plugin
- **Kotlin Documentation:** https://kotlinlang.org/docs/
- **AndroidX:** https://developer.android.com/jetpack/androidx

### Testing
- **JUnit 5:** https://junit.org/junit5/
- **Robolectric:** https://robolectric.org/
- **Espresso:** https://developer.android.com/training/testing/espresso

---

## 12. Project Health Indicators

### Code Quality
- ✅ Strict compilation rules (warnings as errors)
- ✅ Comprehensive test coverage (JUnit + Robolectric + Espresso)
- ✅ Automated code formatting (ktlint)
- ✅ CI/CD integration (GitHub Actions)
- ✅ Code coverage tracking

### Maintenance
- ✅ Active development (main branch tracked)
- ✅ Regular dependency updates (Gradle catalogs)
- ✅ Clear contribution guidelines
- ✅ Responsive maintainers

### Documentation
- ✅ Comprehensive README
- ✅ Contributing guidelines
- ✅ API documentation (LGPL-3.0)
- ✅ Wiki with development guide

---

## Summary

AnkiDroid is a **well-maintained, production-grade Android application** with:
- Modern tech stack (Kotlin 2.3.21, AGP 9.0.1)
- Strict code quality standards
- Comprehensive testing framework
- Clear contribution process
- Active community support

**Status:** ⚠️ **SETUP INCOMPLETE - Requires JDK upgrade before building**

**Action Required:** Install JDK 21+ then proceed with setup checklist in Section 6.

---

*Document Generated: May 4, 2026*  
*Project: AnkiDroid - Semi-official Anki port for Android*  
*Repository: https://github.com/ankidroid/Anki-Android*

