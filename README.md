# 🐺 Wolfi OS Rootfs for Termix

Automated builds of [Wolfi Linux](https://github.com/wolfi-dev/os) rootfs for use in [Termix](https://github.com/jeffusion/Termix) terminal emulator.

## What is this?

Wolfi is a secure, container-optimized Linux distribution from [Chainguard](https://www.chainguard.dev/). This repo automatically builds Wolfi rootfs tarballs that can be used with Termix on Android.

## 📥 Latest Release

**[Download from Releases](../../releases/latest)**

| File | Architecture |
|------|-------------|
| `wolfi-rootfs-aarch64.tar.gz` | ARM64 (most phones) |
| `wolfi-rootfs-x86_64.tar.gz` | x86_64 (emulators) |
| `wolfi-rootfs-armv7.tar.gz` | ARM32 (older devices) |

## 🔄 How it works

1. GitHub Actions pulls `cgr.dev/chainguard/wolfi-base:latest`
2. Exports the container filesystem as a tar.gz
3. Publishes it as a GitHub Release

The build runs:
- **Automatically** every Sunday (to get latest Wolfi updates)
- **Manually** via the "Run workflow" button

## 🛠 Manual Build

1. Go to **Actions** → **Build & Release Wolfi Rootfs**
2. Click **Run workflow**
3. Enter a version tag (e.g., `v1.0.0`)
4. Click the green **Run workflow** button

## 📦 Using in Termix

### Option 1: Manual
1. Download the rootfs for your device
2. Push to device: `adb push wolfi-rootfs-aarch64.tar.gz /sdcard/`
3. Place in Termix data directory

### Option 2: Auto-download (modify Termix)
Add this URL to Termix's `Downloader.kt`:

```kotlin
// Replace YOUR_USERNAME with the repo owner
wolfi = "https://github.com/YOUR_USERNAME/wolfi-os-rootfs/releases/download/wolfi-latest/wolfi-rootfs-aarch64.tar.gz"
```

## 🔐 Why Wolfi over Alpine?

| Feature | Alpine | Wolfi |
|---------|--------|-------|
| Package manager | `apk` | `apk` |
| Base | musl libc | glibc |
| CVE patches | Manual | Auto (daily) |
| Security focus | General | Container-hardened |
| Size | ~5 MB | ~25 MB |

## 📝 License

MIT
