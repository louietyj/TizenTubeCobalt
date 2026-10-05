# Building TizenTubeCobalt APK

## Prerequisites

### Option 1: Docker Desktop (Recommended for Windows)

1. **Install Docker Desktop for Windows**
   - Download from: https://www.docker.com/products/docker-desktop
   - Make sure WSL2 backend is enabled during installation
   - Restart your computer if prompted

2. **Verify Docker is working:**
   ```powershell
   docker --version
   docker compose version
   ```

### Option 2: WSL2 with Docker (Alternative)

If you have WSL2 installed:
1. Install Docker in WSL2
2. Use WSL2 terminal for building

## Build Steps

### 1. Prepare the Build Environment

The userScript.js has already been copied to:
```
starboard/android/apk/app/src/app/assets/web/userScript.js
```

### 2. Build Using Docker

**For Android ARM64 (most Fire TV devices):**
```bash
# Navigate to TizenTubeCobalt directory
cd C:\Users\louie\TizenTubeCobalt

# Build Android ARM64 APK
docker compose run android-arm64
```

**For Android ARM (older devices):**
```bash
docker compose run android-arm
```

**Note:** The first build will take a long time (30+ minutes) as it downloads dependencies and compiles Cobalt.

### 3. Find the Built APK

After building, the APK will be located at:
```
out/android-arm64_gold/cobalt.apk
```
or
```
out/android-arm_gold/cobalt.apk
```

### 4. Install on Fire TV

**Using ADB:**
```bash
adb install out/android-arm64_gold/cobalt.apk
```

**Or transfer via USB/network:**
1. Copy APK to USB drive or use network file sharing
2. Install using a file manager on Fire TV

## Troubleshooting

### Docker Issues on Windows

If Docker commands fail:
1. Make sure Docker Desktop is running
2. Check WSL2 is enabled: `wsl --status`
3. Try using WSL2 directly: `wsl` then run docker commands inside

### Build Fails

- Ensure you have enough disk space (build requires 10+ GB)
- Check Docker has enough memory allocated (8GB+ recommended)
- First build takes much longer than subsequent builds

### userScript.js Not Loading

The userScript.js needs to be injected by TizenTubeCobalt's custom code. If it's not loading:
1. Check if TizenTubeCobalt has custom JavaScript injection code
2. Verify the file is in the correct assets location
3. Check Cobalt's JavaScript loading mechanism

## Alternative: Use GitHub Actions

If local building is too complex, you can:
1. Push your changes to a GitHub fork
2. Use GitHub Actions to build (see `.github/workflows/android_release.yaml`)
3. Download the built APK from Actions artifacts
