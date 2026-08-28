# Awesome LLM Performance Drift

Tracking performance drift, versioning risk, and longitudinal evaluation of Large Language Models — with an AI-assisted research paper, a full citation integrity audit, and a curated collection of papers, datasets, and tools.

## Table of Contents

- [Topic Overview](#topic-overview)
- [AI-Assisted Research Paper](#ai-assisted-research-paper)
- [Citation Integrity Audit](#citation-integrity-audit)
- [Curated Research Papers](#curated-research-papers)
- [Datasets](#datasets)
- [Tools and Libraries](#tools-and-libraries)
- [License](#license)

## Topic Overview

Large language models deployed as commercial APIs are routinely updated without formal versioning guarantees, and these updates can produce substantial, unannounced changes in model behavior and task accuracy — a phenomenon known as **LLM performance drift**. This repository collects resources on how, why, and to what extent LLM performance drifts across successive version updates.

Drift is characterized along four interacting dimensions: capability drift, behavioral and stylistic drift, alignment and safety drift, and instance-level regression ("negative flips"). Detecting and mitigating drift draws on longitudinal benchmarking, holistic evaluation frameworks, backward-compatible training methods, and model/system-card documentation practices. Key methodological challenges include API opacity, benchmark data contamination, sampling non-determinism, and the lack of standardized drift metrics.

Major open research gaps include a shared drift-reporting standard, causal attribution methods that separate data, architecture, and alignment effects, and continuous evaluation-as-infrastructure — treating drift tracking as a routine part of responsible LLM deployment rather than an ad hoc exercise.

## AI-Assisted Research Paper

**Title:** *Tracking Performance Drift in Large Language Models Across Successive Version Updates: A Review of Measurement, Mechanisms, and Mitigation in Longitudinal LLM Evaluation*

A synthesis of evidence on LLM performance drift, drawing on longitudinal audits of GPT-3.5/GPT-4, backward-compatibility research in classical machine learning, and the concept-drift and catastrophic-forgetting literatures.

📄 [Read the paper](./paper/AI_Assisted_Research_Paper.pdf)

## Citation Integrity Audit

Every reference cited in the paper was checked for authenticity, correct metadata, and identifier validity (DOI / PMID / arXiv ID) before being included in this repository. This process helps guard against fabricated or hallucinated citations that AI-assisted writing tools can introduce.

📄 [View the citation integrity audit](./citation-audit/Citation_Integrity_Audit.pdf)

## Curated Research Papers

Verified papers referenced in this work, organized by theme. See [`references/references.md`](./references/references.md) for the full annotated list.

**Drift, forgetting & concept drift**
- Gama et al. (2014) — A survey on concept drift adaptation
- Kirkpatrick et al. (2017) — Overcoming catastrophic forgetting in neural networks
- McCloskey & Cohen (1989) — Catastrophic interference in connectionist networks

**LLM behavioral change over time**
- Chen, Zaharia & Zou (2024) — How is ChatGPT's behavior changing over time?
- Tu et al. (2023) — ChatLog: Carefully evaluating the evolution of ChatGPT across time

**Backward compatibility & regression-free updates**
- Bansal et al. (2019) — Updates in human-AI teams
- Yan et al. (2021) — Positive-congruent training: Towards regression-free model updates
- Xie et al. (2021) — Regression bugs are in your model!
- Schumann et al. (2024) — Backward compatibility during data updates by weight interpolation
- Echterhoff et al. (2024) — MUSCLE: A model update strategy for compatible LLM evolution

**Evaluation, alignment & sycophancy**
- Liang et al. (2022) — Holistic evaluation of language models
- Ouyang et al. (2022) — Training language models to follow instructions with human feedback
- Perez et al. (2023) — Discovering language model behaviors with model-written evaluations
- Sharma et al. (2023) — Towards understanding sycophancy in language models
- Wei et al. (2023) — Simple synthetic data reduces sycophancy in large language models

**Documentation & benchmark integrity**
- Mitchell et al. (2019) — Model cards for model reporting
- Xu et al. (2024) — Benchmark data contamination of large language models: A survey

## Datasets

_Add relevant datasets here — source, description, use case, and link._

## Tools and Libraries

_Add useful software/frameworks with brief descriptions here._

## License

See [LICENSE](./LICENSE) for details.
