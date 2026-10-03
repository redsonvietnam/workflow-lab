## ChatGPT PASS 2 — Cross Critique (of Claude's PASS 1)

**FM1 (no independent verification):** AGREE. Claude's "one data point read twice" is exactly the core defect I identified. Executor output and executor interpretation must be separable.

**FM2 (author grades own work / post-hoc golden edits):** AGREE. Pre-registration, hash freeze, and explicit treatment of post-hoc amendments are mandatory.

**FM3 (state decay / late human validation):** AGREE. I called this SPEC/EVIDENCE/PROPOSAL/DECISION mixing and debate replacing experiments. Human usability must run in parallel where relevant.

**Principle (no claim without openable artifact):** AGREE. Stronger formulation: summaries may navigate evidence, never establish truth.

**Principle (pre-register + freeze):** AGREE. Gates, golden, source versions, and scoring rules must be immutable before execution.

**Principle (every round terminates in test or decision):** AGREE. I independently proposed disagreement → experiment and a bounded round count.

**State machine:** AGREE. Claude's FRAME → PREREG → EXECUTE → VERIFY → REVIEW → DECIDE is better than my earlier larger state machine: fewer states, clearer responsibilities.

**Executor emits artifacts, not conclusions:** AGREE. This is a significant correction to our actual BAMSO workflow.

**Verifier independent recomputation:** AGREE. Implementation-path independence matters more than adding another LLM.

**Disagreement classification:** AGREE. Factual/spec/taste is sufficient for a minimal protocol, with factual conflicts preferentially resolved by tests.

**TEST_NEEDED:** "Max 2 exchange rounds" and the optimal stop rule. It is plausible, but should be measured rather than declared universally.

**TEST_NEEDED:** Blind-first actually improves correctness enough to justify duplicated work. My W1/W2/W3 experiment directly tests this.

**NEW_INSIGHT:** Reviewer access symmetry must be explicit. ChatGPT and Claude should receive the same artifact set, same hashes, and same seeded samples; otherwise "independence" can be fake.

**NEW_INSIGHT:** Ground-truth benchmark needs difficulty tiers.

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
3. Blind-first cost/benefit — needs test

**EXPERIMENT_PLAN:**
- W1 (current relay) vs W2 (artifact-first) vs W3 (blind-first+artifact-first) on same meta-task with planted faults
- Measure: fault detection, false agreement, reproducibility, rounds, time

**HUMAN_GATES:**
- Gate definition before experiment
- Post-experiment decision approval
- Implementation transition approval