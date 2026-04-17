# gRPC Prebuilt Binaries — Windows x64

Prebuilt gRPC binaries for Windows x64, compiled with MSVC in Release mode.
Part of the `airgap-devkit` prebuilt-binaries submodule.

---

## Available Versions

| Version | Compiler | Arch | Status |
|---------|----------|------|--------|
| 1.78.1  | MSVC 19.50 (VS 2026 Insiders) | x64 | candidate-testing |

---

## What's Included

Each version package contains the full `cmake --target install` output:

| Directory  | Contents |
|------------|----------|
| `bin/`     | `protoc.exe`, `grpc_cpp_plugin.exe`, all language plugins |
| `include/` | gRPC, protobuf, abseil-cpp, and all dependency headers |
| `lib/`     | Static `.lib` files for all gRPC components and dependencies |
| `share/`   | CMake config files (`find_package(gRPC)` support) |

---

## Installation — Developers (New Machine)

Run from **any PowerShell** (no Visual Studio required):

```powershell
cd C:\path\to\airgap-devkit\frameworks\grpc
.\install-prebuilt.ps1 -version 1.78.1 -dest C:\MyInstall\grpc-1.78.1
```

The script will:
1. Locate 7-Zip automatically (searches common paths, PATH, and falls back to
   the vendored `prebuilt-binaries/dev-tools/7zip/7z2600-x64.exe`)
2. Reassemble the split parts into a single archive
3. Verify SHA256 integrity against the manifest
4. Extract to the destination path
5. Verify key binaries are present

**No internet access required. No build tools required.**

### Options

| Flag | Default | Description |
|------|---------|-------------|
| `-version` | `1.78.1` | gRPC version to install |
| `-dest` | Auto-detected | Install path (admin → `C:\Program Files\airgap-devkit\grpc-<ver>`, user → `%LOCALAPPDATA%\...`) |
| `-format` | `7z` if 7-Zip found, else `zip` | Archive format to extract |

---

## Building from Source

If you prefer to build gRPC yourself from the vendored source tarball:

```powershell
cd C:\path\to\airgap-devkit\frameworks\grpc
.\setup.ps1 -version 1.78.1 -dest C:\MyInstall\grpc-1.78.1
```

See `frameworks/grpc/README.md` for full build requirements.

---

## Layout

```
prebuilt-binaries/frameworks/grpc/windows/
├── README.md                  <- this file
└── 1.78.1/
    ├── manifest.json          <- SHA256 hashes for all parts
    ├── grpc-1.78.1-windows-x64.7z.part-aa    <- .7z ultra compressed parts
    ├── grpc-1.78.1-windows-x64.7z.part-ab
    ├── ...
    ├── grpc-1.78.1-windows-x64.zip.part-aa   <- .zip ultra compressed parts
    ├── grpc-1.78.1-windows-x64.zip.part-ab
    └── ...
```

---

## Integrity

All parts are SHA256-verified before reassembly. Hashes are stored in
`manifest.json` alongside each version and were self-computed at package time.

---

## Notes

- These binaries are **Windows x64 only**. For Linux, build from source using
  the vendored tarball in `frameworks/grpc/vendor/`.
- The `.7z` format produces significantly smaller archives than `.zip` and is
  preferred when 7-Zip is available.
- Parts are split at `<=45MB` to stay within git's per-file size limits.