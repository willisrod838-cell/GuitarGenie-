# Quick Build Guide for Todo List APK

## Prerequisites

Before building, ensure you have:

### Linux/macOS
```bash
# Install Homebrew (macOS only)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Java
brew install openjdk@11
echo 'export PATH="/usr/local/opt/openjdk@11/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Install Android SDK
brew install android-sdk
echo 'export ANDROID_HOME=/usr/local/share/android-sdk' >> ~/.zshrc
echo 'export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH' >> ~/.zshrc
source ~/.zshrc

# Install Node.js and Cordova
brew install node
npm install -g cordova
```

### Windows
```powershell
# Install Chocolatey (PowerShell as Administrator)
Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install dependencies
choco install openjdk11 -y
choco install androidstudio -y
choco install nodejs -y
npm install -g cordova

# Set environment variables
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\OpenJDK\jdk-11", [EnvironmentVariableTarget]::User)
[Environment]::SetEnvironmentVariable("ANDROID_HOME", "C:\Users\$env:USERNAME\AppData\Local\Android\Sdk", [EnvironmentVariableTarget]::User)
```

---

## Build Steps

### Linux/macOS

```bash
# Make build scripts executable
chmod +x build-apk.sh
chmod +x sign-apk.sh

# Run the build
./build-apk.sh
```

### Windows

```cmd
# Run the build
build-apk.bat
```

---

## Output

After successful build, you'll have:

1. **Debug APK** (for testing)
   - Location: `todo-list/platforms/android/app/build/outputs/apk/debug/app-debug.apk`
   - Can be installed directly on devices
   - Includes debugging symbols

2. **Release APK** (unsigned, for distribution)
   - Location: `todo-list/platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk`
   - Must be signed before distribution

---

## Testing the APK

### Install Debug APK

```bash
# Via ADB
adb install -r todo-list/platforms/android/app/build/outputs/apk/debug/app-debug.apk

# Or via USB file transfer
# 1. Connect phone via USB
# 2. Copy APK to phone Downloads folder
# 3. Tap APK to install
```

---

## Signing for Distribution

After building, sign the release APK:

### Linux/macOS
```bash
./sign-apk.sh
```

### Windows
```cmd
sign-apk.bat
```

This will:
1. Create a keystore (first time only) - **SAVE THIS SECURELY!**
2. Sign the APK with jarsigner
3. Optimize with zipalign
4. Create final APK: `TodoList.apk`

---

## Troubleshooting

### Issue: `cordova: command not found`
```bash
npm install -g cordova
```

### Issue: `ANDROID_HOME not set`
```bash
# Linux/macOS
echo $ANDROID_HOME

# If empty, set it:
export ANDROID_HOME=/path/to/android/sdk

# Windows - Check Environment Variables
# Settings > System > Advanced > Environment Variables
```

### Issue: `No Android SDK found`
1. Install Android Studio
2. Open Android Studio > SDK Manager
3. Install Android SDK 36 and build tools
4. Set ANDROID_HOME variable

### Issue: `Gradle build failed`
```bash
# Clean and rebuild
cd todo-list
cordova clean android
cordova build android
```

### Issue: APK not created
1. Check Java version: `java -version` (must be 11+)
2. Check Android SDK: `sdkmanager --list_installed`
3. Check available disk space
4. Try: `cordova build android --debug`

---

## Next Steps

1. **Test on device**: Install debug APK
2. **Verify functionality**: Check all features work
3. **Sign APK**: Run sign script for release version
4. **Distribute**: Share APK or upload to Play Store

---

## File Structure

```
GuitarGenie-/
├── build-apk.sh           # Build script (Linux/macOS)
├── build-apk.bat          # Build script (Windows)
├── sign-apk.sh            # Sign script (Linux/macOS)
├── sign-apk.bat           # Sign script (Windows)
└── todo-list/
    ├── index.html         # App interface
    ├── styles.css         # Styling
    ├── script.js          # Logic
    ├── config.xml         # Cordova config
    ├── package.json       # Dependencies
    └── platforms/
        └── android/       # Generated Android project
            └── app/build/outputs/apk/
                ├── debug/
                │   └── app-debug.apk
                └── release/
                    └── app-release-unsigned.apk
```

---

## Support

For issues:
1. Check [BUILD_APK_INSTRUCTIONS.md](BUILD_APK_INSTRUCTIONS.md)
2. Check [todo-list/BUILD_TODO_APK.md](todo-list/BUILD_TODO_APK.md)
3. Review Cordova documentation: https://cordova.apache.org/

---

**Happy Building! 🚀**
