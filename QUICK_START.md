# AgriGo Quick Start Guide

## What Changed?

Your project now has a **multi-module Gradle build structure** that allows you to build from the root directory while maintaining clear separation between client and server.

## Quick Commands

Run these commands from the root directory:

```powershell
# Build debug APK
.\gradlew.bat :client:app:assembleDebug

# Build release APK  
.\gradlew.bat :client:app:assembleRelease

# Install on device
.\gradlew.bat :client:app:installDebug

# Run tests
.\gradlew.bat :client:app:test

# View all tasks
.\gradlew.bat :client:app:tasks
```

## Project Structure

```
AgriGo/                     (Root - multi-module project)
├── server/                 (Backend + Web UI)
│   ├── backend/           (Python Flask API)
│   └── agrinova-ui-main/  (React Web UI)
├── client/                (Android app module)
│   ├── app/               (Android app source)
│   ├── build.gradle.kts   (Module build config)
│   └── gradle/            (Versions & config)
├── settings.gradle.kts    (Root: Defines multi-module structure)
├── build.gradle.kts       (Root: Common config)
├── gradlew.bat            (Root: Gradle wrapper)
└── local.properties       (SDK configuration)
```

## Key Files Created/Modified

| File | Purpose |
|------|---------|
| `settings.gradle.kts` (root) | Defines multi-module structure, includes client modules |
| `build.gradle.kts` (root) | Root-level build configuration |
| `gradlew.bat` (root) | Gradle wrapper for root directory |
| `local.properties` (root) | Android SDK path |
| `client/settings.gradle.kts` | Module-specific plugin management (updated) |
| `PROJECT_STRUCTURE.md` | Complete documentation |

## Before vs After

### Before (Error)
```
cd C:\Users\aksha\AndroidStudioProjects\AgriGo
.\client\gradlew.bat :app:assembleDebug  # Complex path
# Error: No Gradle build at root
```

### After (Clean)
```
cd C:\Users\aksha\AndroidStudioProjects\AgriGo
.\gradlew.bat :client:app:assembleDebug  # Simple from root
# Works! ✓
```

## How It Works

1. **Root `settings.gradle.kts`** tells Gradle about:
   - Available modules (`:client`, `:client:app`)
   - Where they're located (`client/`, `client/app/`)
   - Shared version catalog (`client/gradle/libs.versions.toml`)

2. **Gradle Wrapper at root** forwards commands to the client's gradle

3. **Clear abstraction** means:
   - Server stays separate (`server/`)
   - Client is fully self-contained module (`client/`)
   - Both can be developed independently

## For Android Development

- **Android Studio**: Open root folder, it auto-detects modules
- **Command Line**: Use `.\gradlew.bat :client:app:assembleDebug` from root
- **VS Code**: Open root folder with Gradle extension

## Common Tasks

```powershell
# Development
.\gradlew.bat :client:app:assembleDebug
.\gradlew.bat :client:app:installDebug

# Testing  
.\gradlew.bat :client:app:test

# Production
.\gradlew.bat :client:app:assembleRelease
.\gradlew.bat :client:app:bundleRelease

# Cleanup
.\gradlew.bat clean
```

## Troubleshooting

**Q: "SDK location not found"**  
A: Check `local.properties` has your Android SDK path

**Q: Module not found**  
A: Ensure project structure exists:
- `client/` folder ✓
- `client/app/` folder ✓

**Q: Command doesn't work**  
A: Run from root directory, not subdirectories

## Next Steps

1. Open project root in Android Studio
2. Build: `.\gradlew.bat :client:app:assembleDebug`
3. Run on device: `.\gradlew.bat :client:app:installDebug`

See `PROJECT_STRUCTURE.md` for complete documentation.

