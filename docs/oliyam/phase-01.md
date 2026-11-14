# Phase 01: Application independence

Status: implementation in progress; build and runtime verification pending.

## Baseline

- Upstream: https://github.com/storytold/lightcraft
- Pinned fork base: 4c9211ccd69f86aa3973a6c11dabfa7384d2bc5c
- Workspace version: 0.21.1 (upstream); Rust 1.95, edition 2024.
- Branch: feature/application-independence.
- Preserve upstream commit history, internal crate names and licence notices.

## Product contract

Oliyam by Absolute Markup. Linux first; Windows and macOS subsequent. Free, open source, no AI, local processing, Adwaita-inspired interface, lightweight filesystem browser, Snapseed-inspired tools and editable stacks, extensive film profiles with provenance.

## Initial source changes

- Desktop manifest no longer enables denoise by default or requests SAM.
- Desktop startup no longer initialises SAM, enables startup update checking, or initialises facial/denoise model paths; desktop model pickers disabled.
- Existing local lens profiles can be used without automatically fetching the lens database at startup.
- Default config storage and portable folder separated from upstream; transitional default catalogue name changed.
- Initial native window name and version output identify Oliyam; upstream runtime package name, application ID, icons, internal labels and build version remain transitional.
- Existing LIGHTCRAFT_* overrides remain temporarily for inherited tests and tooling; use isolated test settings. Explicit overrides can still select existing paths.

## Critical remaining work

1. Compile the unchanged baseline on Linux and record the result. Compile and exercise the modified branch independently. The current connector session cannot perform a real build.
2. Remove AI runtime paths completely (faces is an unconditional engine dependency; fetch is native; model command registry, drag/drop, UI, dev features and workspace-wide feature unification need inspection). Disable manual updater and other inherited network services, including Windows helper, and verify no upstream update can be applied. No current claim of an AI-free or offline-only binary.
3. Establish independent executable, version, reverse-DNS application ID and Linux packaging, with original branding and notices. Replace—not merely rename—upstream icons. Audit all runtime paths, including caches, logs and temp directories; use XDG conventions. Migrate or remove legacy env overrides after auditing scripts.
4. Audit inherited release workflows: do not publish upstream artifacts or use upstream update channels. Work out license/asset inventory, including third-party data and git dependencies.
5. Verify no AI model files in artifacts and no ML runtime dependency in resolved desktop graph, no unsolicited network traffic, isolated filesystem state, original file integrity, RAW decode, preview/export consistency, classical denoising and manual masks. Test on Linux; no merge or release until gates pass.

## Suggested local verification (development only)

Use a clean checkout, Rust 1.95 and an isolated home/profile. Do not open an existing LightCraft library. Review upstream Linux prerequisites first.

```sh
cargo check --locked -p lightcraft
cargo test --locked -p lightcraft-engine config_tests
cargo tree --locked -p lightcraft -e normal,build,features
```

The existing Cargo package name is deliberately retained while independence is incomplete. If lockfile regeneration is needed, review dependency changes before accepting them. These commands are proposed, not executed evidence. Full workspace tests alone do not prove a non-AI product because development features can unify.
