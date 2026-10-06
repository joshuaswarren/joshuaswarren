# Joshua Warren

**Open-source ML systems, on-device inference, and developer tools.**

I build the software between a model and the machine it runs on: GPU backends, accelerator drivers, compilers, inference servers, and the tools that make them usable.

I'm an Omarchy team member working on [Omarchy M](https://github.com/omacom/omarchy-mac), bringing MLX and Apple Neural Engine workloads to Apple silicon Linux. I also build local-first agent infrastructure and native Mac developer tools.

## MLX, Apple silicon, and local inference

### [omarchy-mlx](https://github.com/joshuaswarren/omarchy-mlx) — MLX on the Apple GPU under Linux

The familiar `import mlx.core as mx`, backed by Vulkan compute through Mesa's Honeykrisp driver rather than Metal. The project maintains a pinned upstream MLX overlay and patch set, with GPU execution on supported Apple silicon Linux machines.

My work spans GPU kernels and fusion, quantized inference, numerical validation, model serving, and distribution. The stack includes local chat, memory-aware model admission, approval-gated downloads, and reproducible release wheels. Its private Vulkan driver stays separate from the desktop's Mesa installation.

The speech path combines a Parakeet encoder on the Apple Neural Engine with decoding on the GPU.

### [mesa](https://github.com/joshuaswarren/mesa) (`honeykrisp-omarchy`) — Honeykrisp Vulkan for Apple GPU compute

Fork of Mesa carrying Honeykrisp Vulkan patches that omarchy-mlx performance depends on: correctness fixes, cooperative matrix, and CDM work. Upstreamable series lives on `upstream/correctness`.

### [omarchy-ane](https://github.com/joshuaswarren/omarchy-ane) + [mil-hwx-compiler](https://github.com/joshuaswarren/mil-hwx-compiler) — the accelerator stack

`omarchy-ane` extends the original [eiln/ane](https://github.com/eiln/ane) work with additional M1/M2-family bring-up, Linux kernel modules, a userspace library, firmware loading, device-tree integration, and hardware validation. Tested configurations include bit-exact operator checks and Parakeet encoder comparisons against macOS references.

`mil-hwx-compiler` compiles a supported subset of Core ML's textual MIL into ANE program formats without invoking Apple's compiler. Its H13 output runs on M1 hardware under Linux; other backends have their own documented validation status. Static shapes, supported operations, and numerical envelopes are explicit.

The work extends through installation and updates: DKMS, package-owned device-tree overlays, boot-chain verification, and opt-in Omarchy packages for the runtime, driver, and compiler.

### [Coreglass](https://github.com/joshuaswarren/coreglass) — understanding inference performance

Coreglass connects hardware signals to real model workloads: prefill and decode rates, time to first token, token gaps, energy per token, CPU time, and available GPU/ANE counters. It captures remote machines over SSH, compares engines, tracks regressions, and produces both human-readable visualizations and machine-readable findings.

Measured, modeled, replayed, and synthetic data are labeled separately.

Related work: [pinned, cross-platform inference benchmarks](https://github.com/joshuaswarren/omarchy-inference-fleet) and [Linux ANE experiments](https://github.com/joshuaswarren/ane-linux-experiments).

## Selected upstream contributions

| Project | Contribution | Status |
| --- | --- | --- |
| [oMLX](https://github.com/jundot/omlx/pull/2975) | Persistent reuse of compiled ANE programs, with cross-process locking, invalidation, and fallback handling. | Merged |
| [mlx-serve](https://github.com/ddalcu/mlx-serve/pull/473) | Linux/Vulkan port of the Zig-based server, including MLX/MLX-C integration, platform compatibility, and end-to-end serving validation. | Merged |
| [Omarchy M](https://github.com/omacom/omarchy-mac/pull/677) | Package-owned device-tree overlays and DKMS-aware boot verification. | Merged |
| [Omarchy packages](https://github.com/omacom/omarchy-pkgs/pull/791) | Integration of the MLX runtime, private Vulkan driver, ANE driver, and MIL compiler into an opt-in package set. | Merged |
| [oMLX runtime observability](https://github.com/jundot/omlx/pull/3003) | Expose the effective DFlash engine and fallback reason rather than only the requested configuration. | Open PR |

**A measured result:** my oMLX ANE compile-cache contribution reduced fresh-process model-load time by **53–66%** in the documented Qwen3.8-27B tests across M1 Max, M2 Max, and M1 Ultra. On the M1 Ultra, a cold cache miss took **65.17 seconds** versus **22.42 seconds** for a warm hit, with identical response text. [Benchmark details and failure-path tests](https://github.com/jundot/omlx/pull/2975).

## Apple-platform developer tools

### [omarchy-apple-dev](https://github.com/joshuaswarren/omarchy-apple-dev)

An iOS development workflow from Linux: Swift toolchains, SDK setup, physical-device deployment, debugging, and signing and packaging checks. The documented device workflow builds and installs SwiftUI apps without running Xcode or macOS; it still uses an Apple-supplied SDK.

My related [xtool work](https://github.com/xtool-org/xtool/pulls?q=is%3Apr+author%3Ajoshuaswarren) addresses dependency resolution, dynamic-library linking and embedding, and app-extension packaging.

### [Allward](https://github.com/joshuaswarren/allward)

A native Mac terminal for coding-agent work across local and remote machines. It combines a custom VT engine and Metal renderer with real PTYs, direct SSH, workspace organization, agent attention routing, and an MCP control surface. It builds and runs on real hardware.

## Memory and context for user-aware agents

### [Remnic](https://github.com/joshuaswarren/remnic)

I'm the creator and maintainer of Remnic: open-source, local-first memory and context shared across coding assistants and other AI agents.

Memories remain inspectable Markdown files. Hybrid retrieval, graph recall, provenance, correction, and MCP/HTTP integrations make context useful without locking it inside one vendor's conversation history. Integrations include Claude Code, Codex CLI, OpenClaw, Cursor, Replit, Pi, and OMP.

As of September 2026, Remnic was seeing approximately **one million monthly package downloads across its integrations**.

Remnic also powers **[What Helps Me](https://github.com/joshuaswarren/remnic/blob/main/docs/hackathons/build-for-good-2026.md)**, my first-place project in OpenAI's 2026 Build for Good hackathon: a support passport where the person approves what is shared and can revoke access.

## More projects and experiments

<details>
<summary>Agent operations, browser memory, and the broader Omarchy ecosystem</summary>

### Agent infrastructure

- [Remnic Canvas](https://github.com/joshuaswarren/remnic-canvas): shared browser-agent memory through WebMCP, with visible approval, correction, and deletion controls.
- [modelctl](https://github.com/joshuaswarren/modelctl): an in-development, provider-neutral control plane for workload contracts, model state, budgets, and bounded agent recovery. The public repository contains generic contracts and synthetic fixtures.
- [Tower](https://github.com/joshuaswarren/tower): self-hosted agent-fleet monitoring with heartbeats, run-state transitions, stale-state detection, and artifact-linked receipts.
- [Fleet Shepherd](https://github.com/joshuaswarren/omarchy-fleet-shepherd): a read-only Omarchy panel for local and SSH-connected agent fleets, with partial-failure and stale-data handling.

### Desktop tools and creative software

[Omaloop](https://github.com/joshuaswarren/omaloop) turns an Omarchy theme into a playable groovebox using a Rust synthesizer and PipeWire. [Dealt](https://github.com/joshuaswarren/dealt) is a daily music-making tool built with deterministic generation and Web Audio, without a model in the product.

Other Omarchy projects include [Omastorm](https://github.com/joshuaswarren/omastorm), [Omarchy Chase](https://github.com/joshuaswarren/omarchy-chase), [Apple Bridge](https://github.com/joshuaswarren/omarchy-apple-bridge), [Remnic integration](https://github.com/joshuaswarren/omarchy-remnic), [Hardstop](https://github.com/joshuaswarren/omarchy-hardstop), [Shiplog](https://github.com/joshuaswarren/omarchy-shiplog), [CodexBar integration](https://github.com/joshuaswarren/codexbar-omarchy), [YouTube Mini](https://github.com/joshuaswarren/omarchy-ytmini), [Plex Mini](https://github.com/joshuaswarren/omarchy-plexmini), and [SomaFM](https://github.com/joshuaswarren/omarchy-somafm).

### Contributions beyond my own projects

I also contribute fixes and integration work to other maintainers' projects, including [HookEcho](https://github.com/d4vid87/hookecho/pull/322), [Blip](https://github.com/nixfred/blip/pull/49), and [Infinitty](https://github.com/jasonkneen/infinitty-free/pull/7).

</details>

## How I work

I use coding agents throughout the development process. My focus is defining the problem, designing the interfaces and constraints, investigating failures, and making results inspectable: hardware tests, reproducible benchmarks, numerical checks, regression gates, and explicit support boundaries.

Across these projects I work with **C/C++, Objective-C++, Python, Swift, Zig, Rust, and TypeScript**, alongside Linux, Vulkan, Metal, MLX, and the Apple Neural Engine.

## Background and interests

I bring more than 25 years of software delivery, architecture, integrations, and developer education. Earlier work included founding and leading Creatuity, serving as the founding chair of the Magento Association, and speaking at more than 20 conferences during the Magento 2 transition.

Today I'm interested in hands-on **ML systems, inference optimization, on-device ML, compilers and runtimes, and open-source developer tooling** roles. Based in Dallas, Texas; focused on remote opportunities.

[Website and field notes](https://joshuawarren.com) · [LinkedIn](https://www.linkedin.com/in/joshuawarren/) · [X](https://x.com/joshuaswarren)

<!-- Contribution status reviewed 2026-10-06. Hardware support and open-PR status change; linked repositories and PRs are authoritative. -->
