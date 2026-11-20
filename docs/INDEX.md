# SDK Documentation Index

This directory documents the Wicofe software development kit for application and system-component developers.

## Architecture and design

- [Wicofe SDK architecture 0.1](SDK_ARCHITECTURE_0.1.md) — draft target profiles, compiler/linker toolchains, Rust/C sysroots, reproducibility, compatibility, packaging and validation.

## Scope
- Target triples and cross-compilation configuration.
- Sysroot, headers, linker configuration, and ABI definitions.
- Application templates, debugging, and packaging guidance.
- SDK release metadata and compatibility matrix.

## Ownership
[Wicofe-ABI](https://github.com/Waizusora/Wicofe-ABI) owns canonical ABI definitions. [Wicofe-Libc](https://github.com/Waizusora/Wicofe-Libc) owns C runtime integration. [Wicofe-Lib](https://github.com/Waizusora/Wicofe-Lib) owns shared Rust libraries. [Wicofe-Build](https://github.com/Waizusora/Wicofe-Build) owns whole-system build orchestration.

## Compatibility
Each SDK release must declare target architectures, toolchain versions, ABI compatibility range, sysroot revision, and runtime assumptions. No complete SDK release is implied by this documentation baseline.

- [Central Wicofe documentation index](https://github.com/Waizusora/Wicofe-Doc/blob/main/INDEX.md)
- [Repository documentation map](https://github.com/Waizusora/Wicofe-Doc/blob/main/REPOSITORY_DOCUMENTATION_MAP.md)
