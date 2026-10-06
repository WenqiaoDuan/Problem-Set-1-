# Problem-Set-1-

*(The follow-up prompt about estimator choice used the results from Phase 2:
median 100.451455 / 101.115029; 10% trimmed 104.670674 / 104.810619;
B2B-excluded mean 108.649644 / 109.092254.)*

### 2. Raw AI reply

The B2B-excluded mean should be the primary estimator in this controlled
synthetic setting because it reproduces the known true mean. The exclusion
rule should be explicitly reported so the reader understands which
observations were removed.

The median is highly robust to the B2B tail and is useful as a robustness
check, but it is not a good estimator of the arithmetic mean in a strongly
right-skewed distribution. In this dataset it systematically understates
the level.

The 10% trimmed mean provides a compromise between the median and the
ordinary mean. It reduces the influence of extreme observations but also
removes observations from both tails, so it can underestimate the population
mean when the upper tail is a genuine part of the distribution.

The main limitation of the B2B-excluded mean is that the exclusion threshold
is based on knowledge of the data-generating process. In real data, a
threshold such as 10,000 may remove legitimate high-value transactions
rather than contamination, so the rule must be validated against domain
knowledge before operational use.

For the synthetic lab, the AI recommended the B2B-excluded mean as the
primary level estimate, with the median and trimmed mean as robustness
checks. For a real policy dashboard, it would not automatically adopt the
threshold.

**Strongest objection (from the AI):** reproducing the known true mean in a
synthetic experiment does not by itself justify using the B2B-excluded mean
for a real policy dashboard. The `<10,000` rule is constructed from knowledge
of the simulation; in real data the analyst does not
