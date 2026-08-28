# GitHub Repositories

Open-source implementations, benchmarking harnesses, and code related to LLM performance drift and longitudinal evaluation.

| Repository | Description | Link |
|---|---|---|
| **stanford-crfm/helm** | Reference implementation of the Holistic Evaluation of Language Models (HELM) framework (Liang et al., 2022) — the paper's primary example of an evaluation-as-infrastructure approach to drift and capability tracking. | https://github.com/stanford-crfm/helm |
| **THU-KEG/ChatLog** | Reference implementation and continuously updated dataset for "ChatLog: Carefully Evaluating the Evolution of ChatGPT Across Time" (Tu et al., 2023) — directly implements longitudinal, periodic evaluation of a deployed LLM. | https://github.com/THU-KEG/ChatLog |
| **EleutherAI/lm-evaluation-harness** | General-purpose, version-controlled benchmark runner supporting 200+ tasks — a practical backbone for building a custom drift-tracking pipeline that re-runs identical evaluations against new model releases. | https://github.com/EleutherAI/lm-evaluation-harness |
| **google/sycophancy-intervention** | Implementation accompanying Wei et al. (2023), "Simple synthetic data reduces sycophancy in large language models" — covers alignment/behavioral drift mitigation via targeted fine-tuning data. | https://github.com/google/sycophancy-intervention |
| **tensorflow/model-card-toolkit** | Implementation of the Model Cards documentation standard (Mitchell et al., 2019) — relevant to the paper's discussion of model/system-card documentation as a drift-mitigation and transparency practice. | https://github.com/tensorflow/model-card-toolkit |
