## Claude PASS 2 — Cross Critique (of ChatGPT's PASS 1)

**FM1 (correlated reasoning via summaries):** AGREE. Same conclusion. I add: the failure was visible in my own replies ("raw data not checked") and we kept issuing binding DECISIONs anyway. A rule should block DECIDE when a reviewer has declared "unverified."

**FM2 (state/discussion mixing):** AGREE, ranked differently. I folded this into my FM3 (status decay). I rank it lower than my FM2 (author grades own gates/golden).

**FM3 (debate substitutes for experiment):** AGREE, partly. Real, but less costly than they imply: key disputes (L6, oracle, golden fixes) did end in experiments. The sharper failure was what experiments could not catch.

**NEW_INSIGHT:** the author-grades-own-homework failure. They cover executor/summarizer overlap (FM1) but not thresholds, gates and golden sets authored and then judged by the same party. 5 of 63 golden entries were edited after seeing output. Their design has no POST_HOC flag or amendment rule. This is the most important gap.

**Principle 1 (evidence primary):** AGREE. Mine adds spec-author ≠ executor ≠ verifier.

**Principle 2 (immutable state):** AGREE, but DISAGREE on "immutable." Too rigid for exploratory benchmarks. Use versioned amendments with a POST_HOC flag, and post-hoc amendments cannot count toward PASS. Otherwise teams bypass the system.

**Principle 3 (disagreement ends in test):** AGREE. Add: a dispute needs a named falsifying experiment.

**CONSENSUS:**
1. Raw artifacts > summaries for reviewer input
2. Pre-registration + hash-freeze before execution
3. Disagreement → experiment (bounded rounds)
4. Executor emits artifacts, not conclusions
5. Independent verifier (different code path) for headline metrics
6. A/B test W1/W2/W3 with planted faults

**RESIDUAL_DISAGREEMENTS:**
1. "Immutable" vs "versioned amendments" — needs test
2. Max 2 rounds — should be measured, not declared

**EXPERIMENT_PLAN:**
- W1 (current relay) vs W2 (artifact-first) vs W3 (blind-first+artifact-first) on same meta-task with planted faults
- Measure: fault detection, false agreement, reproducibility, rounds, time

**HUMAN_GATES:**
- Gate definition before experiment
- Post-experiment decision approval
- Implementation transition approval