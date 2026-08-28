# Tools and Libraries

Software and frameworks useful for tracking, measuring, or mitigating LLM performance drift.

| Tool/Library | Description | Link |
|---|---|---|
| **HELM (crfm-helm)** | Stanford CRFM's open-source Python framework for holistic, reproducible evaluation of language and multimodal models — standardized datasets, a unified model API across providers, and metrics beyond accuracy (efficiency, bias, toxicity). Directly usable for reproducing longitudinal capability comparisons across model versions. | https://github.com/stanford-crfm/helm |
| **lm-evaluation-harness** | EleutherAI's widely used framework for evaluating autoregressive language models across 200+ standardized NLP benchmark tasks via a single interface, with task versioning for reproducibility — useful for re-running the same benchmark suite against successive model releases. | https://github.com/EleutherAI/lm-evaluation-harness |
| **ChatLog toolkit** | Companion codebase to the ChatLog dataset; includes scripts for monthly/daily data collection, feature extraction (knowledge & linguistic features), and a RoBERTa-based cross-version detector used to study which characteristics stay stable across ChatGPT updates. | https://github.com/THU-KEG/ChatLog |
| **sycophancy-intervention** | Google's code accompanying "Simple synthetic data reduces sycophancy in large language models" (Wei et al., 2023) — includes the data-generation pipeline for sycophancy evaluation and intervention fine-tuning. | https://github.com/google/sycophancy-intervention |
| **Model Card Toolkit** | Google's toolkit for generating structured Model Cards (machine-readable + human-readable documentation of a model's intended use, performance characteristics, and limitations) — operationalizes the model/system-card documentation practice discussed in the paper. | https://github.com/tensorflow/model-card-toolkit |
| **Weights & Biases (wandb)** | Experiment-tracking platform that can log model outputs, evaluation metrics, and prompts over time — practical for building an internal longitudinal drift-monitoring dashboard across repeated evaluation runs. | https://wandb.ai |
