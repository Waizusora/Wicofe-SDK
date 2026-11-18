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

## Documentation

See [docs/INDEX.md](docs/INDEX.md) for this repository's technical documentation. The [central repository documentation map](https://github.com/Waizusora/Wicofe-Docs/blob/main/REPOSITORY_DOCUMENTATION_MAP.md) identifies canonical ownership and cross-repository boundaries.

## Wicofe-wide principles

Wicofe is a Unix-like, POSIX-compatible, non-GNU alternative. Native components must use Wicofe interfaces and must not require Linux libc or GNU compatibility layers. Prefer permissive licenses; avoid GPL/AGPL, and consider LGPL only when no suitable permissive alternative exists. Reuse or fork upstream software when implementing it from scratch would be disproportionately difficult. See the [canonical project principles](https://github.com/Waizusora/Wicofe-Docs/blob/main/PROJECT_PRINCIPLES.md).
