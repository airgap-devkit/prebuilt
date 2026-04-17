# airgap-devkit-prebuilt

**Author: Nima Shafie**

Prebuilt binary archives for the [`airgap-devkit`](https://github.com/NimaShafie/airgap-devkit) ecosystem.
All binaries are precompiled, integrity-verified, and ready for air-gapped
installation -- no compiler, no internet access, no package manager required.

Main repo: https://github.com/NimaShafie/airgap-devkit

---

## Purpose

This repository is an **optional submodule** of `airgap-devkit`. Tools that
declare `"uses_prebuilt": true` in their `devkit.json` will look for their
binary archives here under the same relative path (e.g.
`prebuilt-binaries/toolchains/clang/mingw/`).

Tools without `uses_prebuilt: true` are built from vendored source and do not
require this submodule at all. In binary-restricted environments the entire
submodule can be skipped -- every tool has a source-build path.

### Adding this submodule to your clone

```bash
git submodule add https://github.com/NimaShafie/airgap-devkit-prebuilt prebuilt-binaries/
```

Or if cloning the main repo from scratch:

```bash
git clone https://github.com/NimaShafie/airgap-devkit
cd airgap-devkit
git submodule update --init prebuilt-binaries
```

---

## Contents

### `build-tools/cmake/`
CMake 4.3.1 prebuilt binaries. Both files are under 49MB -- no split required.

| File | Platform |
|------|---------|
| `cmake-4.3.1-windows-x86_64.zip` | Windows x86_64 |
| `cmake-4.3.1-linux-x86_64.tar.gz` | Linux x86_64 |
| `cmake-4.3.1.tar.gz` | Source tarball |

### `dev-tools/7zip/`
7-Zip 26.00 prebuilt binaries for Windows and Linux. All files under 49MB.

| File | Purpose |
|------|---------|
| `7z2600-x64.exe` | Windows admin installer |
| `7z2600-extra.7z` | Windows portable (7za.exe) |
| `7z2600-linux-x64.tar.xz` | Linux 7zz binary |

### `dev-tools/servy/`
Servy 7.8 Windows service manager. Single file under 100MB -- no split required.

| File | Format |
|------|--------|
| `servy-7.8-x64-portable.7z` | `.7z` portable |

### `dev-tools/conan/`
Conan 2.27.0 self-contained executables. Both files under 49MB -- no split required.

| File | Platform |
|------|---------|
| `conan-2.27.0-windows-x86_64.zip` | Windows x86_64 |
| `conan-2.27.0-linux-x86_64.tgz` | Linux x86_64 |

### `frameworks/grpc/windows/1.78.1/`
gRPC v1.78.1 prebuilt for Windows x64 (MSVC 19.50, Release).
Full `cmake --target install` output: `bin/`, `include/`, `lib/`, `share/`.

| Format | Size | Parts | Split at |
|--------|------|-------|----------|
| `.zip` (deflate-9) | 170MB | 4 | 49MB |

Install via `frameworks/grpc/install-prebuilt.ps1`.

### `languages/dotnet/10.0.201/`
.NET SDK 10.0.201 for Windows and Linux.

| File | Size | Parts | Split at |
|------|------|-------|----------|
| `dotnet-sdk-10.0.201-win-x64.zip` (deflate-9) | 283MB | 6 | 49MB |
| `dotnet-sdk-10.0.201-linux-x64.tar.gz` | 231MB | 6 | 45MB |

### `languages/python/`
Python 3.14.4 portable interpreters.

| File | Platform | Parts |
|------|---------|-------|
| `python-3.14.4-embed-amd64.zip` | Windows (embeddable) | 1 |
| `cpython-3.14.4+20260408-x86_64-unknown-linux-gnu-install_only.tar.gz` | Linux | 2 |

### `toolchains/clang/mingw/`
llvm-mingw 20260324 -- LLVM-based MinGW-w64 toolchain.

| File | Platform | Parts | Split at |
|------|---------|-------|----------|
| `llvm-mingw-20260324-ucrt-x86_64.zip` (deflate-9) | Windows x86_64 (UCRT) | 4 | 49MB |
| `llvm-mingw-20260324-ucrt-ubuntu-22.04-x86_64.tar.xz` | Ubuntu 22.04 x86_64 | 2 | 50MB |

### `toolchains/clang/rhel8/`
Clang 20.1.8 RPM packages for RHEL 8.

| File | Parts | Split at |
|------|-------|----------|
| `clang-20.1.8-rhel8-rpms.tar` | 2 | 50MB |

### `toolchains/clang/source-build/`
Prebuilt clang-format, clang-tidy, and Ninja binaries.

| File | Platform |
|------|---------|
| `clang-format.exe` | Windows |
| `clang-format-linux` | Linux |
| `clang-tidy.exe` | Windows |
| `clang-tidy.part-aa/ab` | Linux (2 parts, split at 50MB) |
| `clang-llvm-22.1.2-linux-x64-slim.tar.xz` | Linux (3 parts, split at 50MB) |
| `ninja.exe` | Windows |
| `ninja-linux` | Linux |

### `toolchains/gcc/linux/`
GCC Toolset 15 RPM packages for RHEL 8.

| File | Parts | Split at |
|------|-------|----------|
| `gcc-toolset-15-rhel8-rpms.tar` | 2 | 50MB |

### `toolchains/gcc/windows/`
WinLibs GCC 15.2.0 + MinGW-w64 13.0.0 UCRT toolchain for Windows.

| File | Size | Parts | Split at |
|------|------|-------|----------|
| `winlibs-x86_64-posix-seh-gcc-15.2.0-mingw-w64ucrt-13.0.0-r6.zip` (deflate-9) | 264MB | 6 | 49MB |

---

## Archive Formats

Windows binaries use `.zip` (deflate level 9) -- extractable with PowerShell
`Expand-Archive`, `unzip`, or any standard zip tool. No 7-Zip required.

Linux binaries use `.tar.gz` or `.tar.xz` -- extractable with standard `tar`.

The only `.7z` files present are the 7-Zip extras package and Servy, both of
which are either self-bootstrapping or require 7-Zip as a prerequisite by design.

---

## Split Parts

Large archives are split into parts named `<archive>.part-aa`, `part-ab`, etc.
Reassemble before extraction:

```bash
# Linux
cat archive.zip.part-* > archive.zip

# Windows (Git Bash)
cat archive.zip.part-* > archive.zip

# Windows (PowerShell)
$parts = Get-ChildItem -Filter "archive.zip.part-*" | Sort-Object Name
$out = [System.IO.File]::OpenWrite("archive.zip")
foreach ($p in $parts) {
    $bytes = [System.IO.File]::ReadAllBytes($p.FullName)
    $out.Write($bytes, 0, $bytes.Length)
}
$out.Close()
```

Install scripts handle reassembly automatically -- no manual steps required.

---

## Integrity

Each module directory contains a `manifest.json` with SHA256 hashes for all
parts and reassembled archives. Install scripts verify integrity before
extraction. Nothing is extracted if verification fails.

---

## Skipping This Submodule

In binary-restricted environments, do not initialize this submodule:

```bash
git clone https://github.com/NimaShafie/airgap-devkit
cd airgap-devkit
# Do NOT run: git submodule update --init prebuilt-binaries
# Build all tools from vendored source instead
```

Every tool in the main repo has a source-build path that does not
require this submodule.
