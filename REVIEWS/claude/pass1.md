VERDICT: CONDITIONAL

CLAIMS:

Failure modes, ranked by severity:
1. No independent verification. opencode executed, summarized, and chose which numbers to surface. Both reviewers saw only its reports. My own replies repeatedly said "based on opencode's reported figures, raw data not checked." That is the workflow admitting it cannot verify. Two LLMs agreeing on a summary is one data point read twice.
2. The author of specs, gates and golden sets also grades them. Thresholds (5%, 90%, fatal limits) were proposed by me, then I judged results against them. Golden anchors were edited after seeing output (5 of 63) despite the rule "never edit golden to match output." The "100 hand-checked chars" were checked by the party that produced them.
3. Typing and status decay. Proposals became facts without anyone deciding it. Examples: "600–1,200 effective cells," V2 hand-coded values like z1f, "K-group is useless." Human validation arrived only after 12+ rounds of text, so most rounds optimized an unvalidated target.

Mandatory principles:
1. No claim without an openable artifact. A claim cites a hash-pinned raw artifact, and prose summaries are not evidence. Spec-author, executor and verifier must be different parties.
2. Pre-register and freeze. Specs, gates and golden sets are hashed before execution. A change is a versioned amendment carrying a POST_HOC flag if the author saw output, and post-hoc amendments cannot count toward PASS.
3. Every round ends in an experiment or a decision, never more text. Items are typed (SPEC / PROPOSAL / EVIDENCE / DECISION), and only EVIDENCE and DECISION can be cited as fact. Human or external validation is a gate on the critical path, not a final step.

State machine: FRAME → PREREG → EXECUTE → VERIFY → REVIEW → DECIDE → {CLOSE | AMEND → PREREG}.
Roles: Architect (proposes only), Executor (runs and emits artifacts, writes no conclusions), Verifier (independently recomputes, via a different code path or a human), Critic (reads raw samples), Arbiter (human or a pre-registered rule).
Transitions: EXECUTE→VERIFY requires a hashed bundle. VERIFY→REVIEW requires the headline metric to be recomputed independently. DECIDE requires every blocking disagreement to be resolved or logged OPEN.

Evidence mechanism:
Executor outputs a bundle: manifest.lock (sha256), command, environment, raw rows, and a script-generated report. An LLM does not write the report.
Reviewers receive file access or reviewer-seeded row samples, not pasted summaries.
Each review lists which artifacts were opened, and the headline number is recomputed by the reviewer's own script or a fresh tool run.

Disagreement mechanism: Classify each dispute as factual, value/spec, or taste.
Factual: the disputing side must name a falsifying experiment, and the experiment decides.
Value/spec: a pre-registered rule applies, otherwise the human arbiter decides.
Taste: the default is the conservative status quo.
Limits: at most two exchange rounds per dispute. Restating a position does not count as new evidence. Unresolved items are logged OPEN and are never recorded as consensus.

Measurement: Run A/B workflows on tasks with known ground truth, using planted faults. Metrics:
- planted-fault detection rate
- false-accept rate
- count of post-hoc golden or gate edits
- fraction of claims with hash-pinned evidence
- rounds to decision
- human interventions
- reproducibility rate

EVIDENCE_REFS:
- Observed BAMSO workflow history: golden errors (5/63 edited post-hoc), hand-coded V2 values, threshold self-grading, 100-char spotcheck by producer
- The stated observation that both LLMs frequently reviewed summaries rather than raw artifacts

ASSUMPTIONS:
- opencode can persist raw artifacts and stable hashes
- ChatGPT and Claude can be given equivalent artifact access
- A human is available for gates

RISKS:
- Artifact access may be limited by current tooling (browser-based AI)
- Pre-registration adds overhead for small decisions
- Independent verification may be expensive

OPEN_QUESTIONS:
- How to handle decisions too small for full protocol?
- What if the verifier itself is wrong?

PREMORTEM:
1. My proposal could fail if artifact access is technically impossible for browser-based AIs — then we need a different mechanism (e.g., AI Studio, file upload)
2. Pre-registration could become bureaucratic overhead that slows down trivial decisions
3. The "different code path" verifier might share the same blind spots if written by the same person

RECOMMENDED_NEXT_TEST:
Run a planted-fault experiment: give both AIs a summary with a known error, see if they catch it. Then give them the raw artifact, see if detection rate improves.