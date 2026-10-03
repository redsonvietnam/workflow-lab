VERDICT: CONDITIONAL

CLAIMS:

Failure mode #1 — correlated reasoning through summarized evidence (critical): ChatGPT and Claude were often reasoning over opencode's interpretation instead of independently inspecting raw artifacts. This can convert an executor's mistaken interpretation into shared "fact".

Failure mode #2 — no hard separation between state and discussion: SPEC, EVIDENCE, PROPOSAL, and DECISION were repeatedly mixed in conversational text. This creates semantic drift and allows obsolete proposals to become implicit requirements.

Failure mode #3 — debate can substitute for experiment: When disagreement persisted, the workflow sometimes produced more prose rather than a discriminating test. This increases latency without increasing certainty.

Mandatory principle 1: Evidence is primary; summaries are non-authoritative. Reviewers must receive immutable raw artifacts plus hashes/metadata sufficient to reproduce or inspect key claims.

Mandatory principle 2: State is explicit and immutable by phase. A locked SPEC cannot change silently; a PROPOSAL cannot become DECISION without an explicit transition and human gate where required.

Mandatory principle 3: Disagreement should terminate in a test whenever testable. Limit discussion rounds; unresolved conflicts become BLOCKED/NEW_TEST, not endless persuasion.

Minimal state machine:
OPEN → SPEC_FROZEN → BLIND_REVIEW → CROSS_REVIEW → TESTING → EVIDENCE_READY → DECISION_PENDING → HUMAN_GATE → CLOSED
Roles: Human=scope/gates; ChatGPT=architect; Claude=critic; opencode=executor/orchestrator; optional verifier=recompute.

Evidence mechanism: opencode stores raw outputs/artifacts with content hashes, source/version metadata, and stable IDs. Review packets contain references + selected raw excerpts, never summaries alone. ChatGPT and Claude complete first-pass reviews before seeing each other's answers.

Disagreement mechanism: classify each conflict as factual / specification / interpretation / preference. Factual conflicts trigger verification; specification conflicts trigger explicit gate review; interpretation conflicts trigger a discriminating test; preference conflicts are documented and resolved by human scope decision.

Objective comparison: run the same fixed meta-task set under W1=current relay, W2=artifact-first, W3=blind-first+artifact-first. Measure:
- error-detection rate
- false-agreement rate
- missed-critical-evidence rate
- reproducibility of decisions
- human interventions
- number of rounds
- message/token volume
- wall-clock execution time
- number of unnecessary experiments

The best workflow is not the one with fastest agreement; it is the one with the best correct-decision rate per unit of evidence/interaction cost.

EVIDENCE_REFS:
- Observed BAMSO workflow history supplied in this lab context: golden errors, incorrect hand-written codes, stroke/anchor ambiguities, repeated micro-rounds, and reliance on opencode summaries.
- The stated observation that both LLMs frequently reviewed summaries rather than raw artifacts.
- The current proposed roles and failures are derived from these observed workflow behaviors, not from an external benchmark.

ASSUMPTIONS:
- opencode can persist raw artifacts and stable hashes.
- ChatGPT and Claude can be given equivalent artifact access.
- A human is available for gates.

RISKS:
- Artifact access may be limited by current tooling (browser-based AI).
- Pre-registration adds overhead for small decisions.
- Independent verification may be expensive.

OPEN_QUESTIONS:
- How to handle decisions too small for full protocol?
- What if the verifier itself is wrong?

PREMORTEM:
1. My proposal could fail if artifact access is technically impossible for browser-based AIs — then we need a different mechanism.
2. Pre-registration could become bureaucratic overhead that slows down trivial decisions.
3. The "different code path" verifier might share the same blind spots if written by the same person.

RECOMMENDED_NEXT_TEST:
Run a planted-fault experiment: give both AIs a summary with a known error, see if they catch it. Then give them the raw artifact, see if detection rate improves.