# airgap-cpp-devkit-prebuilt

**Author: Nima Shafie**

Pre-built binaries for the `airgap-cpp-devkit` suite.
All binaries are precompiled, integrity-verified, and ready for air-gapped
installation — no compiler, no internet access, no package manager required.

Main repo: https://github.com/NimaShafie/airgap-cpp-devkit

---

## Purpose

This submodule contains prebuilt archives that allow developers on air-gapped
systems to skip source compilation entirely. Each module in the main repo has
a corresponding entry here.

In binary-restricted environments (where prebuilt binaries are not permitted
by policy), this submodule can be skipped entirely — every tool in the main
repo can also be built from vendored source.

---

## Contents

### `build-tools/cmake/`
CMake 4.3.0 prebuilt binaries.

| Platform | Format | Parts |
|----------|--------|-------|
| Windows x86_64 | `.zip` | 2 parts |
| Windows x86_64 | `.7z` | 1 part |
| Linux x86_64 | `.tar.gz` | 2 parts |

### `dev-tools/7zip/`
7-Zip 26.00 prebuilt binaries for Windows and Linux.

| File | Purpose |
|------|---------|
| `7z2600-x64.exe` | Windows admin installer |
| `7z2600-extra.7z` | Windows portable (7za.exe) |
| `7z2600-linux-x64.tar.xz` | Linux 7zz binary |

### `dev-tools/servy/`
Servy 7.3 Windows service manager.

| File | Format | Parts |
|------|--------|-------|
| `servy-7.3-x64-portable.7z` | `.7z` | 2 parts |

### `frameworks/grpc/windows/1.78.1/`
gRPC v1.78.1 prebuilt for Windows x64 (MSVC 19.50, Release).
Full `cmake --target install` output: `bin/`, `include/`, `lib/`, `share/`.

| Format | Size | Parts |
|--------|------|-------|
| `.7z` (ultra) | 69MB | 2 parts |
| `.zip` (ultra) | 162MB | 4 parts |

Install via `frameworks/grpc/install-prebuilt.ps1`.

### `toolchains/clang/mingw/`
llvm-mingw 20260324 — LLVM-based MinGW-w64 toolchain.

| Platform | Format | Parts |
|----------|--------|-------|
| Windows x86_64 (UCRT) | `.zip` | 4 parts |
| Windows x86_64 (UCRT) | `.7z` | 2 parts |
| Ubuntu 22.04 x86_64 | `.tar.xz` | 2 parts |

### `toolchains/clang/rhel8/`
Clang 20.1.8 RPM packages for RHEL 8.

| File | Parts |
|------|-------|
| `clang-20.1.8-rhel8-rpms.tar` | 2 parts |

### `toolchains/clang/source-build/`
Prebuilt clang-format, clang-tidy, and Ninja binaries.

| File | Platform |
|------|---------|
| `clang-format.exe` | Windows |
| `clang-format-linux` | Linux |
| `clang-tidy.exe` | Windows |
| `clang-tidy.part-aa/ab` | Linux (split, 2 parts) |
| `clang-llvm-22.1.2-linux-x64-slim.tar.xz` | Linux (3 parts) |
| `ninja.exe` | Windows |
| `ninja-linux` | Linux |

### `toolchains/gcc/linux/`
GCC Toolset 15 RPM packages for RHEL 8.

| File | Parts |
|------|-------|
| `gcc-toolset-15-rhel8-rpms.tar` | 2 parts |

### `toolchains/gcc/windows/`
WinLibs GCC 15.2.0 + MinGW-w64 13.0.0 UCRT toolchain for Windows.

| Format | Parts |
|--------|-------|
| `.zip` | 6 parts |
| `.7z` | 3 parts |

---

## Archive Formats

All large archives are available in both `.zip` and `.7z` formats where
applicable. `.7z` uses ultra compression (LZMA2) and is significantly smaller.
`.zip` is provided as a fallback for systems without 7-Zip.

Install scripts auto-detect 7-Zip and prefer `.7z` automatically.
If 7-Zip is not found in standard locations or on PATH, the vendored
`dev-tools/7zip/7z2600-x64.exe` is used as a fallback.

---

## Integrity

Each module directory contains a `manifest.json` with SHA256 hashes for all
parts and reassembled archives. Install scripts verify integrity before
extraction. Nothing is extracted if verification fails.

---

## Usage

This submodule is initialized as part of the main repo setup:

```bash
git clone https://github.com/NimaShafie/airgap-cpp-devkit
cd airgap-cpp-devkit
git submodule update --init prebuilt-binaries
```

Each tool's install script references this submodule automatically.
To install a specific tool from prebuilt:

```bash
# Example: gRPC prebuilt
cd frameworks/grpc
./install-prebuilt.ps1 -version 1.78.1
```

---

## Skipping This Submodule

In binary-restricted environments, do not initialize this submodule:

```bash
git clone https://github.com/NimaShafie/airgap-cpp-devkit
cd airgap-cpp-devkit
# Do NOT run: git submodule update --init prebuilt-binaries
# Build all tools from vendored source instead
```

Every tool in the main repo has a source-build path that does not
require this submodule.