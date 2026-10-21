# Harness² — Recursive Agent Harnessing for an Open World
 
<p align="center">
  <strong>Customized & Maintained by Rayyan Anwar</strong>
  <br>
  <em>Independent community fork of the original Harness² project</em>
</p>


<p align="center">
  <img src="assets/Harness2-wordmark.png" alt="Harness²" width="360">
</p>

<p align="center">
  <strong>A community-maintained customization of the original Harness² project.</strong>
</p>

<p align="center">
  <a href="https://arxiv.org/abs/2610.14224"><img src="https://img.shields.io/badge/Paper-arXiv-EA4335?logo=arxiv&logoColor=white" alt="Research paper"></a>
  <a href="https://harness2.github.io"><img src="https://img.shields.io/badge/Upstream-Website-4285F4?logo=googlechrome&logoColor=white" alt="Upstream website"></a>
  <a href="docs/BENCHMARKS.md"><img src="https://img.shields.io/badge/Documentation-Benchmarks-FBBC04" alt="Benchmark documentation"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache--2.0-34A853" alt="License"></a>
</p>

<p align="center">
  <em>Explore, evaluate, and improve agent harnesses while keeping model weights fixed.</em>
</p>

---

## Overview

A **harness** is the system surrounding a language model that enables it to operate as an agent. It can include prompts, guardrails, memory, tools, skills, scripts, subagents, and orchestration logic.

**Harness²** is a framework for recursive agent harnessing in an open world, where agents encounter tasks that were not necessarily anticipated during harness design.

This repository is a community customization of the upstream project. The original research, authorship, benchmark results, and licensing remain attributed to their respective sources.

## Core idea

Harness² uses two complementary levels of recursion.

### 1. Task-level recursion

A Propose–Probe–Compose workflow refines an execution harness for the current task:

1. **Propose** candidate harness modifications.
2. **Probe** their effects through candidate executions.
3. **Compose** a refined harness using evidence from observed behavior.

The approach aims to improve task execution without relying on ground-truth feedback during the refinement step.

### 2. Harnessing-level recursion

An improvement harness retains adopted harnesses and evidence-supported lessons from earlier tasks. This accumulated experience can guide subsequent refinement.

<p align="center">
  <img src="assets/harness2_pipeline.png" alt="Upstream Harness² pipeline illustrating task-level and harnessing-level recursion" width="100%">
</p>

## Harness components

The framework exposes eight categories of editable components:

| Component | Purpose |
|---|---|
| System prompt | Defines the agent's role and instructions |
| Guardrails | Specifies behavioral constraints |
| Memory | Retains useful contextual information |
| Tool guidance | Guides tool selection and usage |
| Subagents | Supports delegated execution |
| Skills | Packages reusable capabilities |
| Scripts | Provides executable workflow support |
| Plugins | Extends runtime functionality |

<p align="center">
  <img src="assets/components.png" alt="Eight Harness² harness components" width="100%">
</p>

## Installation

### Requirements

The upstream instructions specify Linux and Python 3.12 for the documented workflow. Model-backed experiments may additionally require Node.js, Google Cloud authentication, and benchmark-specific dependencies.

Install `uv` using its official [installation guide](https://docs.astral.sh/uv/getting-started/installation/).

From the repository root:

```bash
uv venv --python 3.12 .venv
source .venv/bin/activate
uv pip install -e '.[render]'
```

Run the local demonstration:

```bash
harness2-demo --output runs/demo --k 2
```

The upstream documentation describes this demo as using deterministic editors and Python executions without requiring model credentials.

### Model configuration

For model-backed runs, consult the upstream setup guide before configuring credentials:

- [Setup guide](docs/SETUP.md)
- [Benchmark guides](docs/BENCHMARKS.md)
- [Experiment guide](docs/EXPERIMENTS.md)

If the repository supports the documented environment configuration:

```bash
cp .env.example .env
```

Configure your own authorized project and model settings in `.env`. Do not commit API keys, access tokens, or other secrets.

## Run a benchmark

The following is the upstream-documented LAB example. Check that the commands and dependencies match the current repository revision before execution.

```bash
harness2-setup lab

harness2-check --bench lab

harness2 --bench lab --k 1 --mode parallel \
  --only-domains antitrust-competition \
  --max-tasks 1

harness2-judge --bench lab --k 1 --mode parallel

harness2-report --bench lab --k 1 --mode parallel
```

**Important:** Model-backed benchmark runs may incur API costs. Begin with a small task selection and review the benchmark's usage requirements.

Keep the evaluation configuration consistent with the refinement run, including the benchmark, executor, task subset, recursion budget, mode, and seed where applicable.

### Supported benchmarks

| Benchmark | Focus | Setup command |
|---|---|---|
| JobBench | Professional work | `harness2-setup jb` |
| LAB | Legal work | `harness2-setup lab` |
| WorkBuddy-Bench | Office, web, and code tasks | `harness2-setup wb` |

See [benchmark documentation](docs/BENCHMARKS.md) for dataset requirements, domains, setup, and evaluation details.

## Experiment modes

Harness² supports two task-level recursion strategies in the upstream implementation.

| Mode | Description |
|---|---|
| Parallel | Explores multiple candidate harnesses and combines execution evidence |
| Sequential | Carries refined harnesses and accumulated evidence through successive steps |

The `--k` argument controls the task-level recursion budget.

For example:

```bash
harness2 --bench lab --k 1 --mode parallel
```

Check the command's `--help` output and the experiment guide for supported options and executor-specific configuration.

<p align="center">
  <img src="assets/harness2_recursion_modes.png" alt="Comparison of parallel and sequential recursion modes" width="100%">
</p>

## Reproducibility and result management

For interpretable experiments:

- Record the repository revision, model identifiers, executor, and benchmark version.
- Document task selection, recursion budget, mode, seed, and relevant environment settings.
- Keep baseline and refined-harness evaluations comparable.
- Preserve logs and generated artifacts.
- Report limitations and unsuccessful runs rather than omitting them.
- Avoid tuning against held-out test tasks or leaking evaluation answers into the agent's execution environment.

The upstream workflow stores experiment artifacts under `runs/`. Verify the actual output paths and resume behavior in [the experiment guide](docs/EXPERIMENTS.md).

## Research and results

Read the original paper for the research motivation, methodology, and reported experiments:

**[Harness²: Recursive Agent Harnessing for an Open World — arXiv:2610.14224](https://arxiv.org/abs/2610.14224)**

Reported results belong to the original research and must not be interpreted as independently reproduced results from this customized fork.

No new experimental results are claimed by this README.

## Web access and evaluation security

Unrestricted network access can expose benchmark rubrics, reference answers, or evaluation criteria to an agent. Review the upstream [web-access and security guidance](docs/EXPERIMENTS.md#web-access) and apply appropriate restrictions before running evaluations.

Use isolated environments and authorized credentials when executing untrusted tasks or third-party tools.

## Community customization

This fork focuses on improving:

- README clarity and navigation
- Installation and setup guidance
- Benchmark reproducibility
- Experiment documentation
- Project presentation and maintainability

Changes to experimental behavior, dependencies, or evaluation logic should be documented separately and validated against the upstream implementation.

## Contributing

Contributions should include a clear description of the change and, where relevant, reproducible test steps.

Before opening a pull request:

1. Check that referenced files and commands exist.
2. Validate Markdown links and image paths.
3. Avoid committing credentials or private data.
4. Clearly distinguish verified findings from proposed improvements.

See [CONTRIBUTING.md](CONTRIBUTING.md) if present in the repository.

## Citation

Please cite the original work when using the upstream framework or discussing its research findings:

```bibtex
@article{xu2026harness2,
  title   = {Harness$^2$: Recursive Agent Harnessing for an Open World},
  author  = {Xu, Ruiyao and Chen, Yanfei and CuiZhu, Zhongying and Dalvi Mishra, Bhavana and Ming, Yifei and Yu, Han and Han, Rujun and Lee, Chen-Yu and Pfister, Tomas},
  journal = {arXiv preprint arXiv:2610.14224},
  year    = {2026},
  url     = {https://arxiv.org/abs/2610.14224}
}
```

## License and acknowledgements

Harness² and its upstream research are credited to the original project authors. This fork retains the upstream attribution and license notices.

See the repository's [`LICENSE`](LICENSE), third-party notices, benchmark licenses, and dataset terms before redistribution. Do not assume every bundled dependency or dataset uses the same license.

## Disclaimer

This is an independent community customization and is not an official Google product or an endorsement by the original authors. Refer to the upstream project for authoritative research claims and release information.
