# gRPC Prebuilt Binaries — Linux x86_64

Prebuilt gRPC binaries for Linux x86_64. Part of the `airgap-devkit`
prebuilt-binaries submodule. Windows packages live in the sibling
[`../windows/`](../windows/) directory.

The package is a **maintainer build**, imported here as split parts by
[`scripts/internal/import-grpc-prebuilt.sh`](../../../../scripts/internal/import-grpc-prebuilt.sh)
in the main repo.

---

## Available Versions

| Version | Build | libc | Arch | Status |
|---------|-------|------|------|--------|
| 1.83.0 | RHEL 8 `gcc-toolset-13` | glibc 2.28 | x86_64 | current |

The C++ runtime is linked statically (`-static-libstdc++ -static-libgcc`), so the
single artifact runs on **RHEL 8 and RHEL 9** (and ABI-compatible rebuilds such as
Rocky/Alma 8/9) with no gcc-toolset runtime installed on the target.

---

## What's Included

Extracts to a self-contained, relocatable CMake install prefix:

| Path | Contents |
|------|----------|
| `bin/`     | `protoc`, `grpc_cpp_plugin`, all language plugins |
| `include/` | gRPC, protobuf, abseil-cpp, and all dependency headers |
| `lib/`     | Static archives for all gRPC components and dependencies |
| `share/`   | root certs, etc. |
| `activate.sh` | per-session env setup (`GRPC_ROOT` + PATH) |
| `grpc-toolchain.cmake` | CMake toolchain matching the prebuilt libs |

---

## Installation

```bash
bash tools/frameworks/grpc/setup.sh --platform linux
```

The setup script reassembles the split parts, verifies each part's SHA256 against
`manifest.json` (key `linux-x86_64`), and extracts to the install prefix. Or use
devkit-ui and pick the "Linux x86_64 (RHEL 8/9)" variant.

---

## Layout

```
prebuilt/frameworks/grpc/linux/
├── README.md                  <- this file
└── 1.83.0/
    ├── manifest.json          <- SHA256 (full archive + parts)
    └── grpc-1.83.0-linux-x86_64.tar.gz.part-aa .. part-ab
```

---

## Integrity

Every part is SHA256-verified before reassembly, and the reassembled archive is
verified against the full-archive SHA256. Hashes live in `manifest.json` next to
the parts.

---

## Notes

- **Linux x86_64 only.** Windows MSVC packages are in [`../windows/`](../windows/).
- Parts are split at `<=45MB` to stay within git's per-file size limits.
