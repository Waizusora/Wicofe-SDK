# Wicofe Software Development Kit Architecture 0.1

**Status:** Draft proposal. This document defines SDK responsibilities and release requirements. It does not claim that a complete Wicofe SDK, compiler target, sysroot or native Rust std runtime is available.

## 1. Purpose

Define a reproducible developer toolchain for building Wicofe applications and system components for x86-64 and AArch64. The SDK packages target metadata, compilers/linkers, headers, libraries, debugging tools, build integration and compatibility information.

## 2. Repository ownership

| Component | Responsibility |
|---|---|
| Wicofe-SDK | Developer-facing toolchain distribution, target profiles, sysroot assembly, templates, packaging and compatibility metadata |
| Wicofe-ABI | Canonical public kernel ABI definitions and generated headers/bindings |
| Wicofe-Libc | C runtime, headers/runtime integration and C startup objects |
| Wicofe-Lib | Shared Rust userspace libraries and safe wrappers |
| Wicofe-Kernel | Kernel implementation of the public ABI |
| Wicofe-Build | Whole-system build orchestration, image assembly and integration |
| Wicofe-Tools | Shared project tooling where not SDK-specific |

The SDK packages and coordinates these interfaces; it must not become a second source of truth for ABI, libc or shared library implementations.

## 3. Target profiles

Maintain distinct target profiles for:
- x86-64 Wicofe;
- AArch64 Wicofe.

Each target profile must explicitly record architecture, endianness, pointer width, data layout, calling convention, object format, linker flavor, relocation model, CPU baseline/features, atomic support, alignment requirements and executable ABI version.

Do not invent or publish a final target triple until the OS ABI and toolchain conventions are agreed. Keep target names consistent across compiler, linker, Cargo, CMake/Make integrations, sysroot paths and package metadata.

## 4. Compiler and linker toolchain

The SDK should define supported versions and provenance for:
- Rust compiler, Cargo, rustdoc and required components;
- C/C++ compiler and compiler runtime;
- assembler and linker (including LLD or another selected linker);
- binary inspection, symbol and debug utilities;
- build system integrations and target-aware pkg-config behavior.

Pin toolchain versions for reproducible builds. Record build configuration, patches, source revisions, binary hashes and license notices. Tool selection must be target-specific rather than inherited accidentally from the host.

## 5. Rust target and standard library

A Rust target specification describes compiler assumptions; it does not create a working OS runtime by itself. Rust custom target JSON properties are unstable and can change, so the SDK must pin the Rust toolchain version and validate the target specification against that exact compiler. The official Rust documentation describes custom target specifications and warns that their properties are unstable: https://github.com/rust-lang/rust/blob/main/src/doc/rustc/src/targets/custom.md

The Rust support plan should distinguish:
1. host tools and host-side tests;
2. cross-compilation of core/other no_std code;
3. target-specific runtime support;
4. native Wicofe std support and applications executed on a booted system.

Building Rust standard-library components for a custom target may require nightly build-std workflows; this is a toolchain/build-stage dependency, not proof that Wicofe's process, threading, filesystem or runtime interfaces are complete. Pin the compiler and keep the build procedure reproducible. See the official Rust target documentation: https://doc.rust-lang.org/stable/nightly-rustc/rustc_target/spec/.

Native std readiness requires, at minimum, a coherent process entry/startup contract, allocator, panic/runtime strategy, filesystem and I/O, clocks, threading and synchronization, TLS, environment/arguments and required platform support. Map each dependency to Wicofe-ABI, Wicofe-Kernel, Wicofe-Lib and Wicofe-Libc as appropriate.

## 6. C/C++ target and sysroot

The C/C++ SDK profile must define:
- target triple and compiler target flags;
- sysroot layout and version;
- libc headers, startup objects and libraries;
- compiler runtime and unwind policy;
- linker emulation, default search paths and dynamic interpreter policy if applicable;
- C++ standard library status and ABI policy, if C++ is supported;
- target-aware pkg-config and dependency discovery.

Never allow host headers or host libraries to leak into target builds. Fail the build when a required target dependency is missing instead of silently linking host artifacts.

## 7. Sysroot structure and provenance

The sysroot should have a documented, deterministic layout for headers, libraries, startup objects, linker scripts, target metadata and package configuration. Each sysroot release must identify:
- Wicofe ABI version/revision;
- libc version and patch set;
- Wicofe-Lib crate/API revision where relevant;
- target architecture and profile;
- compiler/linker versions;
- enabled features and runtime assumptions;
- dependency provenance and license inventory.

Generate or validate ABI-facing headers and bindings from canonical Wicofe-ABI definitions. Do not manually fork ABI constants inside the SDK.

## 8. Build, link and package workflow

Provide documented workflows for:
- compiling and linking a minimal C program;
- compiling a minimal Rust no_std target program where supported;
- compiling a Rust std application only after native runtime prerequisites exist;
- building a system service against target libraries;
- inspecting ELF/object metadata and dependencies;
- packaging an application for Wicofe.

Build metadata must distinguish target artifacts from host tools. Support clean, offline/reproducible builds where feasible and record required network-fetched source dependencies.

## 9. Debugging and developer experience

Document debugger support, symbols, unwind/debug format, source mapping, emulator launch configuration, serial/log capture and crash artifact handling. Debugging features must match the actual kernel and executable format capabilities.

Provide templates only for supported application classes. Templates must not imply unsupported APIs or ABI stability.

## 10. Compatibility and release management

Every SDK release must publish:
- SDK version and source/toolchain provenance;
- supported target architectures and CPU baselines;
- minimum/maximum compatible Wicofe ABI revisions;
- sysroot and libc revisions;
- Rust target/toolchain pin;
- supported C/C++ and Rust language/runtime profiles;
- known limitations and migration notes;
- license and third-party notices.

Use immutable release artifacts and checksums. Do not silently replace toolchains or sysroots behind an unchanged SDK version.

## 11. Security and licensing

Treat compilers, build scripts, package metadata and fetched dependencies as part of the supply chain. Pin and verify inputs, audit build scripts, avoid executing untrusted build steps with unnecessary privileges, and record provenance.

Original SDK code targets MPL-2.0. Bundled toolchains and third-party components retain their upstream licenses; every shipped component needs a license/provenance record and compatibility review.

## 12. Testing and validation

Required test layers:
- target-spec validation with the pinned compiler;
- compiler/linker smoke tests for both architectures;
- sysroot contamination tests to detect host headers/libraries;
- C compile/link/run tests against Wicofe-Libc;
- Rust no_std compile/link tests;
- Rust std compile/link/run tests only when runtime support exists;
- ABI header/binding drift checks;
- reproducibility checks and artifact metadata inspection;
- emulator and hardware execution tests where supported;
- dependency/license and provenance audits.

A successful compiler invocation is not proof that the resulting executable can run. Report compile, link, boot/emulator and physical execution as separate validation levels.

## 13. Open decisions

- Final target triples and target naming policy.
- LLVM/Clang versus GCC toolchain strategy and version pins.
- Linker choice and executable/object format details.
- Rust stable/nightly policy and custom target maintenance strategy.
- C compiler runtime, C++ support and unwind policy.
- Static versus dynamic linking and sysroot layout.
- Package format and SDK distribution channels.
- ABI compatibility range and release cadence.
- Debugger/emulator integration and supported hardware matrix.

## 14. Completion criteria

An SDK release is ready only when target profiles and toolchain versions are pinned, sysroots are reproducible and free of host contamination, ABI artifacts trace to Wicofe-ABI, C and Rust smoke tests pass at the claimed support level, license/provenance data is complete, and limitations are published. Native Rust std support must be claimed only after representative applications execute successfully on Wicofe.
