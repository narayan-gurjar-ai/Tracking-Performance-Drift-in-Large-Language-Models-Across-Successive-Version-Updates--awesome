# Datasets

Datasets relevant to LLM performance drift, longitudinal evaluation, sycophancy, and regression testing.

| Dataset | Source | Description | Use Case | Link |
|---|---|---|---|---|
| **ChatLog** | Tu et al. / THU-KEG (Tsinghua) | An ever-updating dataset of large-scale ChatGPT responses collected monthly (and daily) across 21 NLP benchmark tasks, tracking how answers to the same prompts change over successive model versions. | Longitudinal behavioral drift analysis; training drift-detectors on stable vs. shifting response features | https://github.com/THU-KEG/ChatLog |
| **HELM Scenarios & Results** | Stanford CRFM | Standardized collection of evaluation scenarios, prompts, and model outputs used in the Holistic Evaluation of Language Models framework, with historical leaderboard snapshots. | Cross-model and cross-time capability benchmarking; reproducing published drift/capability comparisons | https://crfm.stanford.edu/helm/ |
| **SycophancyEval** | Sharma, Tong et al. (Anthropic, 2023) | Test sets covering four canonical sycophantic behaviors (feedback sycophancy, "are you sure?" answer-reversal, mimicry of user errors, and biased free-form feedback) used to probe RLHF-trained assistants. | Measuring alignment/behavioral drift tied to sycophancy across model versions | Released with the paper: arXiv:2310.13548 |
| **Simple Synthetic Data (Sycophancy Intervention)** | Wei, Huang, Lu, Zhou & Le (Google, 2023) | Synthetic training and evaluation data (including addition-statement sycophancy probes) used to measure and reduce sycophancy via fine-tuning. | Benchmarking sycophancy reduction techniques; before/after drift comparison | https://github.com/google/sycophancy-intervention |
| **HC3 (Human ChatGPT Comparison Corpus)** | Guo et al. (2023) | ~26k questions with paired human-expert and ChatGPT answers across computer science, finance, medicine, law, and open-domain topics; used as a data source within ChatLog. | Baseline human-vs-model response comparison; input corpus for monthly drift snapshots | Referenced via ChatLog's data sources (see ChatLog repo above) |
| **GSM8K** | Cobbe et al. (OpenAI, 2021) | Grade-school math word problem benchmark, one of the source datasets aggregated into ChatLog-Monthly for tracking mathematical reasoning drift over time. | Detecting capability drift on reasoning-heavy tasks across model versions | https://github.com/openai/grade-school-math |

_Note: some entries (SycophancyEval, HC3) don't have a single canonical GitHub link — they're distributed alongside their originating paper or through the dataset that incorporates them. Update the link column if you've sourced a specific mirror._
