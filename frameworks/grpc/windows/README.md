# gRPC Prebuilt Binaries — Windows x64

Prebuilt gRPC binaries for Windows x64, compiled with MSVC in Release mode.
Part of the `airgap-devkit` prebuilt-binaries submodule.

The packages are the **dso-suite maintainer build** (release configuration),
imported here as split parts by
[`scripts/internal/import-grpc-prebuilt.sh`](../../../../scripts/internal/import-grpc-prebuilt.sh)
in the main repo.

---

## Available Versions

Because the static libraries are **ABI-locked** to the MSVC toolset they were
built with, one package ships per Visual Studio version:

| Version | Toolset | Visual Studio | Arch | Status |
|---------|---------|---------------|------|--------|
| 1.81.1 | MSVC v143 | Visual Studio 2022 (default) | x64 | current |
| 1.81.1 | MSVC v145 | Visual Studio 2026 | x64 | current |
| 1.81.1 | MSVC v142 | Visual Studio 2019 | x64 | current |

---

## What's Included

Each package extracts to a self-contained, relocatable CMake install prefix:

| Path | Contents |
|------|----------|
| `bin/`     | `protoc.exe`, `grpc_cpp_plugin.exe`, all language plugins |
| `include/` | gRPC, protobuf, abseil-cpp, and all dependency headers |
| `lib/`     | Static `.lib` files for all gRPC components and dependencies |
| `share/`   | root certs, etc. |
| `activate.ps1` | per-session env setup (`GRPC_ROOT` + PATH) |
| `grpc-toolchain.cmake` | forces `/MT` so your app matches the prebuilt libs |

---

## Installation

Install through the devkit — no Visual Studio required to *consume* the package,
no internet, no admin:

```bash
# CLI (pick the toolset matching your Visual Studio)
bash tools/frameworks/grpc/setup.sh --toolset v143    # VS 2022 (default)
bash tools/frameworks/grpc/setup.sh --toolset v145    # VS 2026
bash tools/frameworks/grpc/setup.sh --toolset v142    # VS 2019
```

Or use devkit-ui and pick your Visual Studio version from the gRPC tool's
selector. The setup script reassembles the split parts, verifies each part's
SHA256 against `manifest.json`, and extracts to the install prefix.

Not sure which toolset you have? Run
`powershell -File tools/frameworks/grpc/Check-Environment.ps1`.

---

## Layout

```
prebuilt/frameworks/grpc/windows/
├── README.md                  <- this file
└── 1.81.1/
    ├── manifest.json          <- per-toolset SHA256 (full archive + parts) + variants
    ├── grpc-1.81.1-msvc142-x64-release.zip.part-aa .. part-ae
    ├── grpc-1.81.1-msvc143-x64-release.zip.part-aa .. part-ae
    └── grpc-1.81.1-msvc145-x64-release.zip.part-aa .. part-ae
```

---

## Integrity

Every part is SHA256-verified before reassembly, and the reassembled archive is
verified against the full-archive SHA256. Hashes live in `manifest.json` next to
the parts.

---

## Notes

- **Windows x64 only.**
- Only the **Release** packages ship in the product. Debug packages (much larger)
  remain upstream in `dso-suite/grpc/dist/` for maintainers.
- Parts are split at `<=45MB` to stay within git's per-file size limits.
