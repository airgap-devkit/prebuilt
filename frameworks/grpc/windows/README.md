# gRPC Prebuilt Binaries — Windows x64

Prebuilt gRPC binaries for Windows x64, compiled with MSVC. Part of the
`airgap-devkit` prebuilt-binaries submodule. Linux packages live in the sibling
[`../linux/`](../linux/) directory.

The packages are a **maintainer build**, imported here as split parts by
[`scripts/internal/import-grpc-prebuilt.sh`](../../../../scripts/internal/import-grpc-prebuilt.sh)
in the main repo.

---

## Available Versions

Because the static libraries are **ABI-locked** to the MSVC toolset they were
built with, one package ships per Visual Studio version — in both **Release** and
**Debug** configurations:

| Version | Toolset | Visual Studio | Config | Arch | Status |
|---------|---------|---------------|--------|------|--------|
| 1.83.0 | MSVC v143 | Visual Studio 2022 (default) | Release / Debug | x64 | current |
| 1.83.0 | MSVC v145 | Visual Studio 2026 | Release / Debug | x64 | current |
| 1.83.0 | MSVC v142 | Visual Studio 2019 | Release / Debug | x64 | current |

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
| `grpc-toolchain.cmake` | forces `/MT` (Release) or `/MTd` (Debug) so your app matches the prebuilt libs |

---

## Installation

Install through the devkit — no Visual Studio required to *consume* the package,
no internet, no admin:

```bash
# CLI (pick the toolset matching your Visual Studio; default config is Release)
bash tools/frameworks/grpc/setup.sh --toolset v143 --config release   # VS 2022 (default)
bash tools/frameworks/grpc/setup.sh --toolset v145 --config release   # VS 2026
bash tools/frameworks/grpc/setup.sh --toolset v142 --config debug     # VS 2019, Debug libs
```

Or use devkit-ui and pick your Visual Studio version + configuration from the
gRPC tool's selector. The setup script reassembles the split parts, verifies each
part's SHA256 against `manifest.json`, and extracts to the install prefix.

Not sure which toolset you have? Run
`powershell -File tools/frameworks/grpc/Check-Environment.ps1`.

---

## Layout

```
prebuilt/frameworks/grpc/windows/
├── README.md                  <- this file
└── 1.83.0/
    ├── manifest.json          <- per-toolset+config SHA256 (full archive + parts) + variants
    ├── grpc-1.83.0-msvc142-x64-release.zip.part-aa .. part-ad
    ├── grpc-1.83.0-msvc142-x64-debug.zip.part-aa   .. part-ak
    ├── grpc-1.83.0-msvc143-x64-release.zip.part-*
    ├── grpc-1.83.0-msvc143-x64-debug.zip.part-*
    ├── grpc-1.83.0-msvc145-x64-release.zip.part-*
    └── grpc-1.83.0-msvc145-x64-debug.zip.part-*
```

---

## Integrity

Every part is SHA256-verified before reassembly, and the reassembled archive is
verified against the full-archive SHA256. Hashes live in `manifest.json` next to
the parts, keyed by `windows-msvc<toolset>-<config>`.

---

## Notes

- **Windows x64 only.** The Linux RHEL 8/9 package is in [`../linux/`](../linux/).
- Both **Release** and **Debug** packages ship. Debug packages are substantially
  larger (~490 MB vs ~180 MB); expect ~11 parts each versus ~4 for Release.
- Parts are split at `<=45MB` to stay within git's per-file size limits.
