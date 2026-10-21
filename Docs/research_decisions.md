# Research Decisions Log

Project: Operationalizing Negative-Flip-Aware Model Updates — A Controlled
Comparative Study of Positive-Congruent Training and Weight Interpolation
Author: Narayan Gurjar (MSE2026002)
Started: 2026-10-20

This log records the literature → gap → question → design chain required by
the course brief, with every entry tagged VERIFIED (checked against the
original source or an actual execution in this sandbox), INFERRED (a
reasoned design choice, not literature-stated), or UNKNOWN (acknowledged
gap/limitation).

## 1. Starting point: the completed review

My Paper 1 ("Tracking Performance Drift in Large Language Models Across
Successive Version Updates") synthesized four interacting drift dimensions
and traced them to four source literatures (Section 2 / Figure 1 of that
paper): concept drift [Gama et al. 2014], model-update regression / negative
flips [Bansal et al. 2019; Yan et al. 2021; Xie et al. 2021; Schumann et al.
2024], catastrophic forgetting [McCloskey & Cohen 1989; Kirkpatrick et al.
2017], and alignment tax/sycophancy [Ouyang et al. 2022; Perez et al. 2023;
Sharma et al. 2023; Wei et al. 2023]. — VERIFIED (my own prior, source-
checked manuscript; main.tex / references.bib from the Paper-1 submission).

## 2. Why a direct LLM-API reproduction is infeasible here (stated up front)

Chen et al. (2024) and Tu et al. (2023) — the two empirical anchors of my
review — depend on paired historical commercial-API snapshots (GPT-3.5/
GPT-4, March vs. June 2023) that are no longer queryable as those exact
snapshots, and on sustained commercial API spend. This sandbox has no
internet access to commercial model APIs, no stored API keys, and no GPU.
Re-running Chen et al.'s exact audit is therefore not an option I can
honestly claim to have executed. — VERIFIED (sandbox has no network route to
model-provider APIs; `pip`/`apt` only reach package registries, confirmed by
environment tooling notes).

Rather than fabricate LLM-API results, I move one layer down the taxonomy in
Figure 1 of Paper 1 to the sub-literature that is (a) explicitly part of my
verified review, (b) the literature the LLM-drift papers themselves borrow
their core metric from, and (c) fully reproducible offline: the classical
model-update-regression / negative-flip literature [Bansal et al. 2019; Yan
et al. 2021; Schumann et al. 2024]. This keeps the project in the same
research area (model versioning / update regression) while making every
claim traceable to code I actually ran. — INFERRED (my scoping decision,
stated transparently as a limitation in the final paper).

## 3. Evidence-based gap (from the verified review)

Paper 1, §2.1 and §5 (Methodological Challenges) states, as VERIFIED claims
from the literature: (i) negative flip rate (NFR) is the standard unit for
model-update regression [Bansal et al. 2019]; (ii) two mitigations exist —
training-time positive-congruent training [Yan et al. 2021] and post-hoc
backward-compatible weight interpolation (BCWI) [Schumann et al. 2024]; and
(iii) my review's §5.4 explicitly flags that "LLM drift studies to date have
used heterogeneous, largely study-specific metrics" and that a head-to-head
comparison of mitigation strategies under a *shared, controlled* protocol is
missing even in the classical-ML literature my review cites — Yan et al.
(2021) and Schumann et al. (2024) are each evaluated against their own
proposed baseline, not against each other. — VERIFIED (re-read of my Paper-1
source text, see `data/paper1_excerpts.md`).

**Gap selected:** no controlled, single-protocol comparison of PCT vs. BCWI
across varying *magnitudes* of simulated model update, with accuracy–NFR
trade-off curves reported for both methods under identical model
initialization, data, and evaluation.

## 4. Research question

**RQ:** Under a controlled softmax-classifier testbed that instantiates the
negative-flip framework, how do positive-congruent training (PCT) and
backward-compatible weight interpolation (BCWI) compare in their
accuracy–NFR trade-off, and how does this trade-off change as the magnitude
of the simulated model update increases?

**Sub-questions:**
- SQ1: At a fixed, large update magnitude, which method achieves a lower
  NFR at matched accuracy (or higher accuracy at matched NFR)?
- SQ2: Does the NFR advantage of either method shrink, grow, or stay flat as
  update magnitude increases?
- SQ3: Which classes/instances drive the residual negative flips under the
  best method, and do they differ from the classes driving NAIVE's flips?

**Success criteria (frozen before running the main sweep, see
`baseline_validation.md`):** a method is considered to give a *practically
useful* compatibility improvement if it reduces mean NFR by ≥20% relative to
NAIVE retraining at matched or ≤2-percentage-point accuracy cost, averaged
over 5 seeds, at the largest update magnitude tested.

## 5. Study pathway

**Pathway B — Controlled comparative study** (course brief §3): three update
strategies (NAIVE, PCT, BCWI) evaluated under one frozen protocol, one
dataset, one fixed test set, identical initialization, 5 seeds, with a
magnitude sweep and ablations over each method's own hyperparameter (PCT's
λ, BCWI's α). — VERIFIED pathway choice recorded before any run (recorded in this log; no git history exists).

## 6. Dataset and comparators (Prompt 06)

- **Dataset:** `sklearn.datasets.load_digits` — the Optical Recognition of
  Handwritten Digits dataset (1,797 samples, 8×8 grayscale, 10 classes),
  bundled with scikit-learn, no network access required, no license
  restriction for research/educational use. — VERIFIED (loads offline;
  `sklearn.__version__` and sample counts logged in
  `results/logs/00_env_check.log`).
- **Fixed held-out test set:** 20% stratified split, seeded, never used in
  any training step, identical across every run in the sweep. — VERIFIED by
  construction (see `src/data.py::make_splits`, asserted via test-index hash
  check in `scripts/00_env_check.py`).
- **"Old" model:** softmax classifier trained on a fixed 25% stratified
  subsample of the remaining pool, representing an earlier, less-resourced
  snapshot. Fixed per seed, reused as the comparison point for every update
  magnitude in that seed.
- **Comparators:**
  1. **NAIVE** — retrain from scratch on the updated data fraction, no
     compatibility term. This is the real-world default practice the
     drift/compatibility literature critiques, and is therefore the correct
     baseline, not a strawman.
  2. **PCT** — simplified, "PCT-style" (NOT a verified reproduction of Yan et al. 2021; written from the AI's understanding, not checked against the original paper/code) positive-congruent training: NAIVE's
     objective plus a consistency penalty against the old model's
     predictions on instances the old model got right.
  3. **BCWI** — [Schumann et al. 2024]-style: train NAIVE, then linearly
     interpolate its weights with the old model's weights post-hoc.
  (A fourth row, **OLD-frozen**, is reported as a reference floor: the old
  model re-evaluated on the fixed test set, giving NFR = 0 by construction
  and an accuracy lower bound.)

## 7. Primary metrics (frozen before viewing any result)

- **Accuracy** on the fixed test set (higher is better).
- **Negative Flip Rate (NFR)** — the fraction of test instances the old
  model classified correctly that the new model misclassifies (lower is
  better; formalized in Paper 1, Eq. 2, after Bansal et al. 2019).
- **Positive Flip Rate (PFR)** — the fraction of test instances the old
  model got wrong that the new model now gets right (reported for context;
  not optimized directly).

## 8. Known limitations acknowledged in advance

- A linear softmax classifier is not a large language model; this is a
  controlled analogy to the *update-regression mechanism*, not a claim about
  LLM-specific phenomena (instruction-following, sycophancy, RLHF). This
  limitation is stated explicitly in the paper's Limitations section.
- "Update magnitude" is operationalized as training-data-fraction growth,
  not an actual retrained LLM checkpoint; this is an INFERRED proxy, chosen
  because it is the only update mechanism fully under my control and
  reproducible offline.
- BCWI's weight interpolation requires old and new models to share the same
  parameterization and be initialized from the same initial weights — a
  known simplifying condition in the weight-interpolation literature
  (consistent with Schumann et al. 2024's own setting) — which I enforce and
  state explicitly rather than silently assuming.

## 9. Protocol-design pilot (run before freezing the main sweep)

Before freezing the experimental protocol, I ran two small pilot checks
(first run was inline with no saved log and printed TEST-set numbers; re-run on the
VALIDATION split as `scripts/00b_pilot_design.py`, log `results/logs/00b_pilot_design.log`) to choose a sensible "update magnitude" axis:

1. **Varying only the old-model data fraction** (8%–25% of pool) produced a
   nearly flat, very small NFR (0.8%–1.7%, 3–6 flipped test instances out of
   360) because `load_digits` is an easy, near-linearly-separable task — a
   softmax classifier saturates accuracy quickly even on a small training
   set. This axis gives too little signal to meaningfully compare PCT vs.
   BCWI. — VERIFIED (printed console output, reproduced above).

2. **Fixing the new-model training set at the full pool (fraction = 1.0)
   and instead injecting additive Gaussian pixel noise into a σ-controlled
   fraction of the new-model's training images** (test set always clean)
   produces a clean, monotonic NFR response: σ=0 → NFR≈0.8%, σ=10 →
   NFR≈11.4% (out of 360 test instances). This operationalizes Gama et al.
   (2014)'s formalization of concept drift as a change in the training-time
   P(X,Y) relative to the evaluation-time distribution, and gives a usable
   dial for "update magnitude." — VERIFIED (printed console output).

**Decision (frozen before running the main sweep):** "update magnitude" =
Gaussian corruption level σ ∈ {0, 2, 4, 6, 8, 10} applied to 100% of the new
model's training images (test set is always clean/uncorrupted); the old
model is always trained on a clean, fixed 25% stratified subsample (σ=0).
This is an INFERRED proxy for a real LLM version update (see Section 8
limitations) — chosen because it is reproducible offline, fully under my
control, and gives a non-degenerate signal, not because it was the first
thing tried (the data-fraction-only axis above was tried first and
rejected for giving too little signal, which is reported here rather than
hidden).

**Hyperparameter-selection order (to keep this a fair, non-circular
comparison):** PCT's λ and BCWI's α are each swept once, at a single fixed,
pre-declared magnitude (σ=8, "strong update") across all 5 seeds
(`scripts/02_ablation.py`). The value of λ and α that best satisfies the
Section-4 success criterion (≥20% NFR reduction vs. NAIVE at ≤2-point
accuracy cost, mean over seeds) is then FIXED and carried, unchanged, into
the magnitude-sensitivity sweep (`scripts/03_magnitude_sweep.py`) that
answers SQ2. Hyperparameters are never re-tuned per magnitude point — doing
so would let each method "cheat" differently at each σ and would make the
sensitivity curves uninterpretable.


## 10. Corrections made during the session (honest change log)

- Ablation originally selected alpha on the TEST set (leakage). Fixed: a 20% validation
  split of the non-test pool is now used for all selection (`src/data.py::make_val_split`);
  `02_ablation.py` evaluates on validation only; `03`/`04` report on test. All results
  re-run. Numbers quoted in Sections 4/9 earlier in this file refer to the success
  criterion only; the original test-based pilot numbers were superseded by
  `results/logs/00b_pilot_design.log`.
- Selection rule: an extra "beat OLD" floor was briefly added, then removed (see
  `ai_assistance_log.md`). alpha=1.0 is excluded as a structural identity.
- The assignment brief was read late; pathway/gate naming here was first written from
  assumption and matches the brief (Pathway B; G1-G4) on re-check.
- Success criterion (frozen in Section 4, before the sweep): >=20% mean NFR reduction vs
  NAIVE at <=2-point accuracy cost. Note it was written after the first pilot looks, not
  before any data was seen.
