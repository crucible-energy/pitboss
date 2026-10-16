# README orientation and platform status

- Date: 2026-10-16
- Owner: Sam's organization-wide README review
- Branch: `codex/docs-pitboss-readme-platform-start`
- Source: `main` at `cbd0c7847336b95f564d9ce06178deeaae0f688a` (tree `bb22e519fea165003123d65b3a283422bee35de4`)
- History: No earlier implementation log was present at the source revision. The prior framing and milestone proposal remain in `README.md` history and `REPORT.md`.

## User Experience Findings

- The README had no direct getting-started path or links to agent guidance, ADRs, and the architecture map.
- Its command block mixed successful setup checks with two negative fixtures. Source execution confirmed both negative examples intentionally exit with status 1, which can stop a copied shell block early.
- “Linux Now, Mac Later” conflicted with the current macOS and Linux priorities and repeated an old Swift version without identifying current validation evidence.
- The root `AGENTS.md` began with the stray text `žée`, before the actual policy heading.

## Engineering Decisions

- Lead with a short capability boundary and commands that pass from the repository root. Keep intentionally rejected fixtures in a separate section with their expected exit status.
- Derive the package/toolchain facts from `Package.swift`; distinguish Linux host detection from a Linux build/test result. The README makes no platform-parity claim from a host-probe code path.
- Explain that Core ML framework importability does not mean model inference is implemented, and retain the MLX stub boundary.
- Link to existing research, architecture, agent, and ADR documents instead of repeating the evolving inference proposal.
- Remove only the stray prefix from `AGENTS.md`; preserve its policy text. Do not resolve open architecture or commercialization issues as part of a README edit.

## Known Limitations

- Pitboss remains a feasibility study: no model loading, backend dispatch, inference, media generation, or end-to-end evaluation is implemented.
- The source contains Linux host detection, but this increment has no fresh Linux build/test receipt. A macOS arm64 result is not Linux parity evidence.
- Core ML can be imported on Apple platforms, but no Core ML inference is qualified; MLX remains a contract stub.
- Commercialization classification remains open in [issue #4](https://github.com/crucible-energy/pitboss/issues/4). This documentation makes no licensing or commercialization claim.
- Frozen verification and review evidence belong in the pull request and its external validation receipt so the validated documentation stays unchanged.
