# Wicofe-SDK

Software development kit for building applications and system components targeting Wicofe OS.

## Scope

- Target definitions and cross-compilation configuration.
- Sysroot, headers, and documented ABI interfaces.
- Linker and compiler integration.
- Application templates and packaging guidance.
- SDK versioning and compatibility metadata.

## Boundaries

Wicofe-ABI owns canonical ABI definitions. Wicofe-Libc owns the C runtime. Wicofe-Lib owns shared userspace libraries. Wicofe-Build owns whole-system build orchestration.

## Compatibility

Each SDK release must declare the minimum and maximum supported ABI version, target architectures, toolchain versions, and runtime assumptions.

## Status

Initial repository baseline; no complete SDK release is available.

## License

Original project code targets MPL-2.0. Bundled toolchains, headers, and third-party components require license and provenance records.
