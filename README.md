# Pitboss

Pitboss is an experimental, zero-Python native generative-media runtime
feasibility study. It currently inspects supported Comfy workflows, validates
model-bundle manifests, reports host, plugin, and backend contracts, and checks
the zero-Python policy. It does **not** load models, run inference, or generate
media.

## Getting started

Requirements: Git and Swift 6.0 or newer. Install Swift using the official
[macOS](https://www.swift.org/install/macos/) or
[Linux](https://www.swift.org/install/linux/) instructions. The package
declares a macOS 13 deployment target.

Clone the repository, then run these commands from its root:

```sh
git clone https://github.com/crucible-energy/pitboss.git
cd pitboss
swift --version
swift test
swift run pitboss doctor
swift run pitboss inspect plugins
swift run pitboss validate Fixtures/comfy/basic-api-workflow.json
swift run pitboss validate-manifest Fixtures/manifests/sdxl-coreml-example.json
swift run pitboss policy-check
```

`swift run` builds the CLI when needed. Run `doctor` and `policy-check` from
the checkout root. For machine-readable workflow validation, add `--json`
before the workflow path, for example:

```sh
swift run pitboss validate --json Fixtures/comfy/basic-api-workflow.json
swift run pitboss validate-manifest --json Fixtures/manifests/sdxl-coreml-example.json
```

Workflow output uses schema `pitboss.validation.v1`; manifest output uses
schema `pitboss.manifest-validation.v1`.

The unsupported-node and invalid-tokenizer fixtures are rejection examples, not successful setup checks. Each command below is expected to exit with status 1:

```sh
swift run pitboss validate Fixtures/comfy/unsupported-api-workflow.json
swift run pitboss validate-manifest --json Fixtures/manifests/invalid-tokenizer-clip-bpe.json
```

## Current scope

- Parse supported Comfy API workflow JSON into a graph and report unsupported nodes.
- Validate model-bundle manifest structure, capabilities, tokenizer requirements, and backend artifact references.
- Describe plugin and backend contracts and report the host environment.
- Check runtime, build, and test source for zero-Python policy violations.

These checks validate inputs and describe contracts. They do not dispatch a backend or prove that a model can execute.

## Platform status

macOS and Linux are the priority portability targets. `Package.swift` requires
Swift tools 6.0 and declares macOS 13 as its Apple-platform deployment target.
The host probe has macOS and Linux branches. A detected Linux host does not by
itself prove that the package builds or tests successfully there. The Swift
tests and CLI examples above passed on macOS arm64 with Swift 6.4; a fresh
Linux build/test result is still needed before claiming parity.

On Apple Silicon macOS, `doctor` may report that Core ML can be imported. That
only confirms framework availability: Core ML inference is not implemented.
MLX is a contract stub. On Linux, Core ML is unavailable by design and MLX
remains a stub.

## Repository map

- `Sources/PitbossCore`: graph, manifest, plugin, backend, diagnostics, provenance, and policy contracts.
- `Sources/PitbossApple`: host probing and Apple framework seams.
- `Sources/PitbossCompat`: workflow and ecosystem metadata parsers.
- `Sources/PitbossCLI`: command-line interface.
- `Tests`: core and compatibility tests.
- `Fixtures`: small workflow and manifest examples.
- `ADR`: architecture and policy decisions.

For contributor and agent rules, read [`AGENTS.md`](AGENTS.md). The research
report and architecture map are [`REPORT.md`](REPORT.md),
[`pitboss-research.md`](pitboss-research.md), and
[`docs/PARTS_ATLAS.md`](docs/PARTS_ATLAS.md). Detailed contracts live in the
[architecture ADR](ADR/ADR-0001-Architecture.md), including the
[zero-Python policy](ADR/ADR-0002-Zero-Python-Policy.md),
[plugin capability system](ADR/ADR-0003-Plugin-Capability-System.md),
[model-bundle manifest](ADR/ADR-0004-Model-Bundle-Manifest.md), and
[Comfy import scope](ADR/ADR-0005-Comfy-Import-Scope.md).

## Future direction

The long-term direction is plugin-based image, video, and audio generation
with explicit model manifests, graph validation, native backend dispatch,
diagnostics, and reproducible provenance. The research report proposes a small
native Core ML component as the first real inference milestone, before full
text-to-image generation. These are proposals; tokenizer parity, model loading,
backend execution, and end-to-end evaluation are not yet implemented.

## Zero-Python policy

Pitboss runtime, build, and test code must not depend on Python, PyTorch, diffusers, transformers, ComfyUI, virtual environments, or shelling out to Python. Existing Python projects may be studied as reference material only. Documentation may discuss compatibility with Python ecosystems; runtime source must not invoke them.
