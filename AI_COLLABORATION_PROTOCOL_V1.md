# AI COLLABORATION PROTOCOL V1

## 1. Roles

| Role | Who | Can do | Cannot do |
|------|-----|--------|-----------|
| Human | User | Lock scope, approve gates, final decision | — |
| Architect | ChatGPT | Propose hypothesis, design, synthesize | Execute, verify own work |
| Critic | Claude | Adversarial review, find failure modes | Propose final design |
| Executor | opencode | Run experiments, emit artifacts, manage state | Write conclusions, grade own work |
| Verifier | Script/3rd party | Recompute headline metrics independently | — |

## 2. State Model

```
FRAME → PREREG → EXECUTE → VERIFY → REVIEW → DECIDE → {CLOSE | AMEND → PREReg}
```

- **FRAME**: Human defines scope and objective
- **PREREG**: Specs, gates, golden sets hashed and frozen
- **EXECUTE**: opencode runs, emits raw artifacts + manifest.lock
- **VERIFY**: Independent recomputation of headline metric
- **REVIEW**: Blind first-pass → cross critique
- **DECIDE**: Consensus or BLOCKED → human gate

## 3. Artifact Model

```
EVIDENCE/round-NNN/
  manifest.lock.json    # sha256 of all inputs
  command.sh            # exact command run
  environment.json      # versions, config
  raw/                  # raw output rows
  report.json           # script-generated (not LLM-written)
  samples/              # reviewer-seeded samples
```

**Rule**: LLM never writes the report. Script generates it.

## 4. Message Protocol

Each round message contains:
- `ROUND_ID`: unique identifier
- `DELTA`: new claims, evidence, disagreements only
- `ARTIFACT_REFS`: hash-pinned references (not full content)
- `QUESTION`: specific ask

**No full transcript relay.** History lives in repo, not in messages.

## 5. Blind-Analysis Protocol

1. Executor emits artifact bundle with hash
2. opencode sends SAME artifact to both AIs (symmetry)
3. Both AIs submit first-pass independently (deadline barrier)
4. Only after both submitted → exchange for cross critique
5. Cross critique: AGREE / DISAGREE / NEW_INSIGHT / TEST_NEEDED

## 6. Evidence Protocol

- Every claim cites `artifact_id + sha256`
- Summaries are navigation, never truth
- Reviewer must list which artifacts were opened
- Headline number recomputed by reviewer's own script

## 7. Decision Protocol

```yaml
decision_id:
spec_version:
spec_hash:
question:
chatgpt_verdict:
claude_verdict:
agreements: [...]
disagreements: [...]
evidence: [{id, sha256}]
tests_required: [...]
human_gate: {status: PENDING|APPROVED|REJECTED}
status: OPEN|TESTING|APPROVED|REJECTED|BLOCKED
```

## 8. Human Gate Protocol

Only 3 gates:
- **GATE 1**: Approve spec/golden/thresholds before execution
- **GATE 2**: Approve human validation results (if user-facing)
- **GATE 3**: Approve transition to implementation

## 9. Disagreement → Experiment Protocol

1. Classify: factual / spec / interpretation / preference
2. Factual → name falsifying experiment → run → evidence decides
3. Spec → pre-registered rule or human arbiter
4. Interpretation → discriminating test
5. Preference → conservative status quo
6. Max 2 exchange rounds per dispute
7. Unresolved → BLOCKED (never recorded as consensus)

## 10. Verification Protocol

- Primary: main implementation (e.g., Python)
- Verifier: independent implementation (e.g., JS) OR human spot-check
- Both must agree on headline metric within tolerance
- Mismatch → investigate, don't average

## 11. Stop Rules

- Max 2 review rounds + 1 micro-round per decision
- Disagreement after limit → BLOCKED + new experiment
- No infinite debate
- "Disagreement → test", not "disagreement → more text"

## 12. Minimal Implementation for opencode

```
workflow-lab/
  SPEC/
    SPEC.yaml          # locked, hashed
    SPEC.sha256
  EVIDENCE/
    round-001/
      manifest.lock.json
      command.sh
      raw/
      report.json
  REVIEWS/
    chatgpt/
      pass1.md
      pass2.md
    claude/
      pass1.md
      pass2.md
  DECISIONS/
    decisions.yaml
    decisions.md
  PREMORTEM/
```

---

## WHY_CURRENT_WORKFLOW_FAILS

1. **Correlated reasoning**: Both AIs read opencode's summary → one data point read twice
2. **Self-grading**: opencode executes AND summarizes → grades own homework
3. **State decay**: SPEC/EVIDENCE/PROPOSAL/DECISION mixed in conversation → proposals become facts
4. **Post-hoc contamination**: Golden edited after seeing output (5/63), no POST_HOC flag
5. **Late human validation**: 12+ rounds before human screen
6. **Text substitution**: Disagreement → more prose instead of experiments

## WHAT_CHANGED

- Raw artifacts with hash > summaries
- Pre-registration + freeze before execution
- Blind first-pass before cross critique
- Executor emits artifacts, not conclusions
- Independent verifier (different code path)
- Bounded rounds + BLOCKED state
- Human gates at 3 points only

## WHAT_WAS_MEASURED

- PASS 1 blind analysis: both AIs independently identified same top failure mode (correlated reasoning)
- PASS 2 cross critique: 9/10 agreements, 3 residual disagreements
- Convergence: 2 rounds to reach consensus on core protocol

## WHAT_REMAINS_UNPROVEN

- W1 vs W2 vs W3 objective comparison (not yet run)
- Optimal stop rule (2 rounds vs other)
- Blind-first cost/benefit ratio
- Whether "versioned amendments" beats "immutable" for exploratory work
- Scalability beyond 2 AIs + 1 executor