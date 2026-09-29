# 🐧 Linux Rootfs for Termix

Automated builds of lightweight, terminal-only Linux rootfs tarballs for use in the [Termix](https://github.com/RohitKushvaha01/ReTerminal) terminal emulator on Android.

## What is this?

This repo builds three glibc-based rootfs images, all published together in **one release**:

- **[Wolfi](https://github.com/wolfi-dev/os)**: secure, container-optimized distro from [Chainguard](https://www.chainguard.dev/) (`apk`)
- **[Void Linux](https://voidlinux.org/) (glibc)**: independent, minimal distro with a fast package manager (`xbps`)
- **[Debian](https://www.debian.org/) stable-slim**: the classic, with the biggest package repository (`apt`)

Every image is minimal and terminal-only (no GUI) and is built for both ARM64 and x86_64.

## 📥 Latest Release

**[Download from Releases](../../releases/latest)**

| Distro | ARM64 (most phones) | x86_64 (emulators, ChromeOS) |
|--------|---------------------|------------------------------|
| Wolfi | `wolfi-rootfs-aarch64.tar.gz` | `wolfi-rootfs-x86_64.tar.gz` |
| Void (glibc) | `void-rootfs-aarch64.tar.gz` | `void-rootfs-x86_64.tar.gz` |
| Debian slim | `debian-rootfs-aarch64.tar.gz` | `debian-rootfs-x86_64.tar.gz` |

> ⚠️ Only ARM64 and x86_64 are built. Wolfi does not support ARM32 (armv7); use Alpine for older devices.

### 📦 Pre-installed packages

`fastfetch` `curl` `git` `gh` `bash` `openssh`

Install more with the distro's package manager:

| Distro | Command |
|--------|---------|
| Wolfi | `apk add <package>` |
| Void | `xbps-install -S <package>` |
| Debian | `apt update && apt install <package>` |

## 🔄 How it works

1. GitHub Actions checks the digests of the three base images:
   - `cgr.dev/chainguard/wolfi-base:latest`
   - `ghcr.io/void-linux/void-glibc:latest`
   - `debian:stable-slim`
2. If any of them changed, it builds all three for ARM64 and x86_64 (using QEMU), installs the packages, and exports each container filesystem as a `.tar.gz`
3. Publishes all six files in a single GitHub Release

The build runs:
- **Automatically** every day at midnight UTC (only builds if a base image changed)
- **Manually** via the "Run workflow" button

## 🛠 Manual Build

1. Go to **Actions** → **Build & Release Rootfs (Wolfi, Void, Debian)**
2. Click **Run workflow**
3. Optionally enter a release tag (e.g., `v1.0.0`). Leave it empty to auto-generate one from the image digests
4. Click the green **Run workflow** button

> Tags can't contain spaces. Invalid characters are replaced with `-`.

## 📦 Using in Termix

### Option 1: Manual
1. Download the rootfs for your device and distro
2. Push to device: `adb push wolfi-rootfs-aarch64.tar.gz /sdcard/`
3. Place in Termix data directory

### Option 2: Auto-download (modify Termix)
Release tags change on every update, so link to the latest release instead of a fixed tag. Add URLs like these to Termix's `Downloader.kt`:

```kotlin
// Replace YOUR_USERNAME with the repo owner
wolfi  = "https://github.com/YOUR_USERNAME/wolfi-os-rootfs/releases/latest/download/wolfi-rootfs-aarch64.tar.gz"
void   = "https://github.com/YOUR_USERNAME/wolfi-os-rootfs/releases/latest/download/void-rootfs-aarch64.tar.gz"
debian = "https://github.com/YOUR_USERNAME/wolfi-os-rootfs/releases/latest/download/debian-rootfs-aarch64.tar.gz"
```

## ⚖️ Which one should I pick?

| Feature | Wolfi | Void (glibc) | Debian slim |
|---------|-------|--------------|-------------|
| Package manager | `apk` | `xbps` | `apt` |
| libc | glibc | glibc | glibc |
| Init system | none (container-style) | runit (not used in rootfs) | systemd (not used in rootfs) |
| Updates | Rolling, frequent CVE fixes | Rolling | Stable releases |
| Package selection | Smaller, security-focused | Large, up to date | Largest |
| Best for | Hardened, minimal setups | Fast, up-to-date tooling | Maximum compatibility |

## 📝 License

MIT
