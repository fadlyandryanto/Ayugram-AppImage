# AyuGram Desktop AppImage

This repository provides an automated build pipeline that packages the `ayugram-desktop-git` package from Arch Linux into a portable, highly compressed AppImage.

## Features

* **Automated Updates:** A GitHub Actions workflow runs daily to check the [Chaotic-AUR](https://www.google.com/search?q=https://chaotic.cx/) repository for upstream updates. If a new version is detected, the pipeline automatically builds and releases it.
* **High Compression:** Packages are built using the `quick-sharun` utility and compressed with the Dwarfs file system and Zstandard (`zstd:level=22`) to ensure an incredibly small file size without sacrificing launch speed.
* **Arch-Based Core:** The AppImage is compiled within a pristine Arch Linux container, ensuring that it captures the latest rolling-release dependencies.
* **Zero-Setup Portability:** Bundles all necessary libraries (via `sharun`), meaning it can run on almost any modern Linux distribution without requiring manual dependency installation.

## How to Use

1. Navigate to the [Releases](https://www.google.com/search?q=../../releases) page of this repository.
2. Download the latest `ayugram-desktop-*.AppImage` file.
3. Make the file executable by running the following command in your terminal:
```bash
chmod +x ayugram-desktop-*.AppImage

```


4. Run the application:
```bash
./ayugram-desktop-*.AppImage

```


*Alternatively, you can double-click the file in most desktop environments.*

## How the Build Works

The automated pipeline is defined in `.github/workflows/main.yml` and performs the following steps:

1. **Environment Setup:** Spins up an `archlinux:latest` Docker container in privileged mode.
2. **Chaotic-AUR Integration:** Installs the Chaotic-AUR keyring and mirrorlist to access the pre-compiled `ayugram-desktop-git` package.
3. **Version Checking:** Queries `pacman` for the latest version and compares it against the local `VERSION` file in the repository. The build aborts if no new updates are found.
4. **Packaging:** Installs the package alongside `xorg-server-xvfb` and `patchelf`. It then utilizes [quick-sharun](https://www.google.com/search?q=https://github.com/pkgforge-dev/Anylinux-AppImages) to trace, bundle, and package the binary (`/usr/bin/AyuGram`), its icon, and its `.desktop` file into a self-contained `.AppDir`.
5. **Release:** Compresses the output into an AppImage, updates the repository's `VERSION` tracking file, and publishes the artifact via GitHub Releases.

## Disclaimer

This is an unofficial, automated build. The AppImage is generated directly from the Chaotic-AUR package repository and is not officially endorsed by or affiliated with the original AyuGram project.