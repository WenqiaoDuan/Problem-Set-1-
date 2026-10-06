# Econ 5200 PS1 — The Measurement Audit

**Course:** ECON 5200
**Notebook:** [`Econ_5200_PS1.ipynb`](./Econ_5200_PS1.ipynb)
**Module:** [`src/basket_metrics.py`](./src/basket_metrics.py)
**AI transcript:** [`ai-appendix.md`](./ai-appendix.md)

---

## Overview

This problem set audits a retailer's "average basket value" metric built on a
synthetic `basket_value` panel. It reproduces the wrong number the dashboard
shows (Phase 1), separates the two contamination mechanisms that cause the gap
(Phase 2), ships a small tested module (Phase 3), and runs an adversarial
review of the recommendation (Phase 4).

---

## Phase 4 — AI Expansion

### Task 4.1: The adversarial review

**What I found, including if it changes my Phase 2 recommendation:**

All three estimators increase from 2023 to 2024, but they tell different
stories: the median rises from 100.451455 to 101.115029 (+0.66%), the 10%
trimmed mean from 104.670674 to 104.810619 (+0.13%), and the mean after B2B
exclusion from 108.649644 to 109.092254 (+0.41%). The true mean is 108.649644
(2023) and 109.092254 (2024), and the key finding is that the B2B-excluded mean
reproduces the true mean exactly, while the median and trimmed mean sit below.

This partially changes my Phase 2 recommendation:

1. **The median is demoted** from primary recommendation to robustness check —
   it estimates a different quantity than the arithmetic mean and should not be
   the headline level when the target is the arithmetic mean on a right-skewed
   distribution.
2. **The trimmed mean is kept** but no longer as the sole recommendation.
3. **The B2B-excluded mean is promoted** to primary recommendation *for this
   lab*, because it is the only estimator that reproduces the known true mean
   when the rule matches the known contamination mechanism — provided it is
   reported together with the exclusion rule.

In short, if the goal is a robust read on the trend the median is acceptable,
but if the goal is an accurate level for a policy team, the estimator must be
chosen from the known distribution and the handling rule stated explicitly —
and in this lab that points to the B2B-excluded mean, not the median.

**The strongest objection, run down:** the `<10,000` exclusion rule could be
reverse-engineered from the known simulation. A threshold sensitivity analysis
(on `df_logged`, sweeping `[5_000, 10_000, 20_000, 50_000]` against the known
clean true mean) shows only `<10,000` reproduces the true mean exactly
(gap = 0 in both years); `<5,000` under-excludes legitimate retail and
`<20,000` and above let the B2B tail back in. The rule is validated *for this
controlled lab*, not discovered from the data — so the `<10,000` threshold must
not be transferred to real data without independent validation.

---

### Task 4.2: The board slide

**Headline number:**
Report **108.65 (2023) → 109.09 (2024)** (B2B-excluded mean), the only
estimator that reproduces the known true mean.

**The correction:**
The dashboard previously showed the median, **100.45 → 101.12**. That is not
the average basket — it sits low because the distribution is right-skewed. The
10% trimmed mean (**104.67 → 104.81**) sits low for the same reason. The three
estimators also disagree on growth from (+0.13%) to (+0.66%), so the choice of
estimator, not the data, drives the headline.

**The confidence:**
High *within this synthetic lab*, because ground truth is known and the
exclusion rule recovers it exactly. Low *as a transferable method*, because the
rule was built from knowledge of the simulation.

**What I'd want before betting on it:**
Validation that `basket_value < 10,000` removes genuine B2B contamination
rather than legitimate high-value transactions — tested against real domain
data, not the simulation.

---

## Repository contents

| Path | Description |
|---|---|
| `Econ_5200_PS1.ipynb` | Full notebook, Phases 1–4 |
| `src/basket_metrics.py` | Phase 3 module (`audit_report`, `robust_mean`) |
| `ai-appendix.md` | Full AI transcript: prompt as sent, raw reply, rejected parts, changes made |
| `README.md` | This file |

---

## Link to `ai-appendix.md`

[ai-appendix.md](./ai-appendix.md)
