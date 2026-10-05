# Quick Build Guide for TizenTubeCobalt APK

## Current Status

✅ **userScript.js is ready!**
- Location: `starboard/android/apk/app/src/app/assets/web/userScript.js`
- Contains: Your native text input feature for Android TV platforms

## Building the APK

### Prerequisites Check

Before building, you need:

1. **Docker Desktop** (recommended) OR **WSL2 with Docker**
2. **Android SDK/NDK** (downloaded automatically by build scripts)
3. **GN build system** (installed automatically)
4. **Python 3** (for build scripts)

### Option 1: Docker Build (Recommended)

**Install Docker Desktop:**
1. Download from https://www.docker.com/products/docker-desktop
2. Enable WSL2 backend during installation
3. Restart computer

**Build commands:**

```bash
# Navigate to TizenTubeCobalt
cd C:\Users\louie\TizenTubeCobalt

# Build Android ARM64 (for most Fire TV devices)
docker compose run android-arm64

# OR build Android ARM (for older devices)
docker compose run android-arm
```

**First build takes 30-60 minutes** (downloads dependencies, compiles Cobalt)

**Output APK location:**
- ARM64: `out/android-arm64_gold/cobalt.apk`
- ARM: `out/android-arm_gold/cobalt.apk`

### Option 2: GitHub Actions Build

If Docker is too complex:

1. **Push your changes to GitHub:**
   ```bash
   cd C:\Users\louie\TizenTubeCobalt
   git add .
   git commit -m "Add native text input feature"
   git push origin main
   ```

2. **Trigger GitHub Actions:**
   - Go to your GitHub repo → Actions tab
   - The workflow should run automatically
   - Download the APK from the workflow artifacts

### Option 3: Manual Build (Advanced)

If you have Linux or WSL2 set up with all dependencies:

```bash
# Install GN
wget https://chrome-infra-packages.appspot.com/dl/gn/gn/linux-amd64/+/latest -O gn.zip
unzip gn.zip -d gn_bin
sudo mv gn_bin/gn /usr/local/bin/gn

# Generate build files
gn gen out/android-arm64_gold --args='target_platform="android-arm64" target_os="android" target_cpu="arm64" build_type="gold" sb_api_version=15'

# Download Android SDK
bash ./starboard/android/shared/download_sdk.sh

# Build
ninja -C out/android-arm64_gold cobalt_install
```

## Installing on Fire TV

### Method 1: ADB (Recommended)

```bash
# Connect Fire TV via USB or enable ADB over network
adb connect <fire-tv-ip>:5555

# Install APK
adb install out/android-arm64_gold/cobalt.apk
```

### Method 2: USB/Network Transfer

1. Copy APK to USB drive or use network file sharing
2. On Fire TV, use a file manager app to install the APK
3. Enable "Install from Unknown Sources" if prompted

## Verifying userScript.js is Loaded

After installing and running TizenTubeCobalt:

1. Open YouTube TV in the app
2. Navigate to search
3. Open browser DevTools (if available) or check logs:
   - Look for: "TizenTube Native Text Input: Module loaded"
   - Look for: "TizenTube Native Text Input: Converted search text box to native input"

## Troubleshooting

### Docker Issues
- **"Docker not found"**: Install Docker Desktop
- **"Permission denied"**: Run Docker Desktop as administrator
- **WSL2 errors**: Enable WSL2 in Windows Features

### Build Fails
- **Out of disk space**: Free up 10+ GB
- **Memory errors**: Increase Docker memory limit (Settings → Resources)
- **Network errors**: Check internet connection (build downloads many files)

### APK Not Installing
- **"App not installed"**: Uninstall existing TizenTubeCobalt first
- **"Unknown sources"**: Enable in Fire TV settings
- **Architecture mismatch**: Use correct APK (arm64 vs arm)

## Next Steps

1. **Build the APK** using one of the methods above
2. **Install on Fire TV** device
3. **Test voice search** - click the search box and verify native keyboard appears
4. **Test voice input** - use the microphone button in the native keyboard
5. **Report results** - let me know if it works or if adjustments are needed!

## Important Notes

- The `userScript.js` is in the assets directory, but TizenTubeCobalt needs to inject it into the web page
- If the script doesn't load, we may need to modify TizenTubeCobalt's C++ code to inject it
- Check TizenTubeCobalt's GitHub issues or Discord for injection mechanism details
