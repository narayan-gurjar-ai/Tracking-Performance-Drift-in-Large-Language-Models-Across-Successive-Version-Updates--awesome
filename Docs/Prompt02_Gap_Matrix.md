# Prompt 02 — Evidence-based research gaps (gap matrix)

Student: Narayan Gurjar (MSE2026002) · Source: revised Paper 1 (section numbers refer to it).
Rules applied: no novelty is claimed (that needs a separate literature search); "reported" = the limitation appears in the cited original paper, "inferred" = my reading of the review; items needing verification are flagged.

## Gap matrix (five computationally testable gaps)

| # | Source passage in review | Reported in an original paper, or inferred? | Testable question | Experiment that would test it | Evidence still missing |
|---|---|---|---|---|---|
| **G1** | §5.3 + Table 2 list positive-congruent training and weight interpolation (BCWI) as separate mitigations; §6.4: "absence of a shared, standardized drift-reporting metric"; §7.5 | **Inferred.** The claim that the two are not compared with each other under one protocol is my reading and is **UNVERIFIED** against the two original papers | How do naive retraining, a consistency-penalty method and BCWI compare in accuracy vs negative flip rate as update magnitude grows? | Controlled comparison, shared splits/seeds, magnitude sweep, ablation of each method's hyperparameter, per-class failure analysis | Read Yan et al. 2021 and Schumann et al. 2024 to see what baselines they used; separate novelty search |
| **G2** | §2.1 (Eq. 2) and §3.4: aggregate accuracy "can mask" instance-level regression | **Reported** in Bansal et al. 2019 for human-AI teams (verify page); extension to LLMs is inferred | How strongly does NFR track accuracy change across update types? Can accuracy delta ever predict NFR? | Train old/new pairs under several update types (more data, noisy data, class shift); report NFR vs Δaccuracy with correlation per seed | Check whether this has already been measured; define the update types a priori |
| **G3** | §6.3: single-sample comparisons "risk conflating genuine drift with ordinary sampling variance" | **Inferred** for the noise-floor framing; the underlying concern is reported for LLM APIs. The review's statement about Chen et al.'s bootstrapping is from recollection — **verify** | What is the NFR "noise floor" between two retrainings of the *same* configuration with different seeds, and how does an observed update's NFR compare to it? | Train N seeds of one config; compute pairwise NFR; compare to NFR of a real update | Literature on prediction churn (e.g. Milani Fard et al., NeurIPS 2016 — recalled by the AI, **not verified**) |
| **G4** | §3.5 and §4: Chen et al.'s hypothesis that instruction-following decline explains several drifts | **Reported as a hypothesis** by Chen et al. (verify wording); not tested by me | Does separating format compliance from content correctness change the measured drift? | Score a small open-weight LLM pair (e.g. base vs instruction-tuned) with two graders: strict-format and content-only | Open-weight models and a data source reachable offline — **not available in this sandbox** (UNKNOWN) |
| **G5** | §7.5: whether backward compatibility and alignment objectives can be optimized jointly | **Inferred** (the review poses it as an open question) | Does adding a regression-aware term worsen alignment-tax-style capability loss? | RLHF-scale fine-tuning with a compatibility term | Needs RLHF infrastructure; **infeasible** here |

## Ranking (significance × resources × feasibility)

| Rank | Gap | Why |
|---|---|---|
| 1 | **G1** | Directly supported by §5.3/§6.4; fully runnable on CPU; yields a trade-off analysis, not just a number. **This is the gap I selected and ran as Paper 2.** |
| 2 | **G2** | Cheap, reuses the same code, and has a clear falsifiable prediction (NFR not predicted by accuracy change) |
| 3 | **G3** | Cheap and gives a baseline that makes any NFR claim interpretable; the LLM version is an extension, not the core test |
| — | G4, G5 | Valuable, but need open-weight LLMs or RLHF compute that this environment lacks |

## Cautions
- **Novelty is not established** for any gap. Do a literature search before claiming it (prediction churn, regression-free updates, backward-compatible training, model merging).
- Relevance to commercial LLMs is **inferred**, not shown, for G1–G3 because they run on a classical testbed.
- Verify the exact wording of every quoted limitation against the original publication before using it in a paper.
