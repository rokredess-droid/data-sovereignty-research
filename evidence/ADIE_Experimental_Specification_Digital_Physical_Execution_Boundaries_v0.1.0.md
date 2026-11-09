# ADIE Experimental Specification — Digital and Physical Execution Boundaries

**Document ID:** ADIE-EXP-SPEC-001  
**Version:** 0.1.0  
**Date:** 4 November 2026  
**Status:** Draft specification; milestone in progress  
**Authority:** Subordinate to constitutional Sources, approved ADRs and current roadmap  
**Implementation authorization:** None

## 1. Purpose and decision boundary

Define a bounded, reproducible experiment to test the frozen ADIE reference across one digital and one simulated physical execution boundary:

**Security Law Book → Dynamic Authority Derivation → Portable Authority → Independent Enforcement at Every Execution Boundary → Verified Effect → Audit**

This specification does not approve architecture, finalize the Security Law Book schema, select a policy engine, authorize production deployment, or claim universal enforcement. Identity/session/permission is not complete dynamic authority. A decision is not execution; an execution receipt is not proof of effect; a log is not proof of complete observation.

## 2. Objectives and hypotheses

The experiment shall:
- preserve actor, agent, delegation, task, resource, action, audience and scope across handoffs;
- derive deterministic authority from a versioned input/rule snapshot;
- enforce independently at each tested executor boundary;
- reject malformed, mismatched, expired, revoked, stale or replayed authority;
- distinguish decision, admission, execution, observed effect and verification;
- use independent read-back/observation for effect verification;
- reconstruct the operation timeline and expose evidence gaps.

| ID | Hypothesis | Falsification |
|---|---|---|
| H1 | Authority remains bounded across handoff | Boundary accepts altered subject/audience/action/resource/scope |
| H2 | Tested boundary enforces locally | Protected mutation occurs without valid admission |
| H3 | Revocation/expiry follows declared commit rule | Admission after cutoff without approved exception |
| H4 | Execution and effect are separate | Provider success alone becomes `VERIFIED` |
| H5 | Evidence reconstructs the operation | Reviewer cannot correlate request→decision→enforcement→execution→observation |
| H6 | Unknown evidence is not permission/success | Missing/contradictory evidence yields `ALLOW` or `VERIFIED` |

## 3. Experiment scope

**Included:** contract-level simulation; one controlled digital mutation; one simulated sensor→controller→actuator/process path; authority derivation; portable test artifact; local enforcement; negative/race/bypass/fault scenarios; independent observation; evidence reconstruction.

**Excluded:** production/customer data; real PLC or safety-critical device; changes to Core or NoredAm/DevTools; policy-engine selection; final authority schema; universal integration coverage; generic emergency override or safety-state behavior.

## 4. Conceptual topology

```text
Untrusted intent → Request normalization → Identity/context references
      → Security Law Book test rules → Dynamic authority derivation
      → ALLOW / DENY / ESCALATE / INDETERMINATE
      → (ALLOW only) portable authority test artifact
          ├─ Digital boundary validator → Digital executor → independent read-back
          └─ Physical boundary validator → simulated controller/actuator
                                            → independent process observation
      → Effect predicates → Evidence correlation/reconstruction
```
This is a test arrangement, not an approved deployment topology.

## 5. Stable correlation identifiers

Every run uses unique identifiers: `test_run_id`, `scenario_id/version`, `request_id`, `task_id`, `decision_id`, `authority_artifact_id`, `operation_id`, `execution_id`, `boundary_id`, `resource_id`, `observation_id`, `verification_id`, `evidence_record_id`. IDs are correlation keys, not trust proof; issuer and integrity must be recorded.

## 6. Authority decision test contract

Required inputs:
- accountable principal and authenticated identity reference;
- agent/workload and delegation chain, when applicable;
- tenant/organization/resource ownership context;
- normalized action/resource, executor/audience and task/operation;
- authoritative rule-set ID/version, evaluator version/digest;
- context/risk/runtime facts with provenance and freshness;
- validity and intended effect/postcondition.

Decision is exactly one of: `ALLOW`, `DENY`, `ESCALATE`, `INDETERMINATE`. No implicit allow. `INDETERMINATE` is non-executable for protected operations; `ESCALATE` is not a permit.

Derivation is deterministic for an immutable input/rule/evaluator snapshot. Applicable constraints compose restrictively; explicit prohibition cannot be overridden by a lower-authority grant; missing/conflicting/stale required facts cannot manufacture permission; delegated authority cannot exceed the delegator. Record evaluated constraints, source/version and disposition.

## 7. Portable authority test artifact

The integrity-protected artifact contains:
- issuer ID, key/version reference and issuer standing;
- principal, agent/workload and delegation chain;
- request/task/operation binding;
- audience/boundary, tenant, resource and action;
- bounded scope and obligations;
- issue/not-before/expiry times and rule/policy version;
- nonce/replay control and revocation reference;
- decision ID, result and reason/constraint references;
- integrity algorithm/key ID and test-run linkage.

It is not self-authorizing. The executor validates integrity, issuer trust, binding, scope, validity, freshness/revocation and local constraints before admission.

## 8. Boundary enforcement contract

Both boundaries independently:
1. authenticate caller and boundary identity;
2. verify artifact integrity and issuer trust;
3. bind exact request/action/resource/audience/tenant/operation;
4. reject invalid, expired, revoked, replayed or mismatched artifacts;
5. check local capability and domain constraints;
6. record enforcement result/reason;
7. execute only on valid `ALLOW`;
8. distinguish admission from execution;
9. expose operation correlation/state transitions;
10. preserve local domain/safety controls.

Direct/alternate paths are tested. A bypass must be blocked or detected and reported as coverage failure—not counted as enforced.

## 9. Digital executor profile

Perform one bounded mutation on an isolated test resource. `VERIFIED` requires independent read-back of resource identity, expected version/change, exact value/content digest, no unauthorized collateral mutation and correlation/integrity. Executor response alone is `EXECUTION_REPORTED`, not `VERIFIED`.

## 10. Simulated physical executor profile

Deterministic simulator fields: sensor value/time/quality; threshold/hysteresis; controller state; actuator command/state; process state (e.g. tank level); independent observation; injected delay/fault.

Example: pump activation is permitted only below a test low-level threshold and with no configured interlock/fault. Thresholds, hysteresis, timing and interlocks are explicit simulator parameters—not ADIE defaults. No physical safety behavior is inferred.

`VERIFIED` requires fresh/quality-valid sensor evidence, satisfied preconditions/interlocks, valid admission, expected actuator state, process predicate within declared time window, independent correlated observation and no contradictory fault evidence. Command, output and process outcome remain distinct. Failure/unknown yields `FAILED`, `PARTIAL`, `NOT_VERIFIED` or `INDETERMINATE`, never automatic success.

## 11. Separate state dimensions

**Authority:** `NOT_EVALUATED → ALLOW | DENY | ESCALATE | INDETERMINATE`  
**Execution:** `NOT_ADMITTED → ADMITTED → QUEUED (optional) → DISPATCHED → RUNNING → COMPLETED | FAILED | PARTIAL | OUTCOME_UNKNOWN`  
**Effect:** `NOT_STARTED → OBSERVING → VERIFIED | NOT_VERIFIED | CONTRADICTORY | INDETERMINATE`  
**Cancellation:** `NOT_REQUESTED → REQUESTED → ACKNOWLEDGED | REJECTED | TOO_LATE`

Cancellation requested does not prove execution stopped or effect reversed. Retry/compensation is a separately authorized operation with idempotency controls.

## 12. Revocation and commit ordering

Define one boundary admission/commit point. Compare revocation/cancellation against controlled sequence/commit evidence, not timestamps alone:
- revocation before admission commit: reject;
- revocation after commit: follow explicitly declared in-flight rule and record whether execution continues/stops/becomes unknown;
- expiry before commit: reject;
- cancellation acknowledgement does not prove physical reversal;
- late revocation cannot be represented as retroactively preventing committed effect.

Report the chosen rule and observed behavior; do not generalize to every subsystem.

## 13. Evidence record

Each scenario records:
- scenario/run/configuration; rule source/version and evaluator digest;
- context facts/provenance references;
- actor, agent, delegation, tenant, resource, action, audience;
- request/task/decision/artifact/operation/execution IDs;
- decision, diagnostics and evaluated constraints;
- boundary enforcement result; event sequence and commit ordering;
- executor response/raw state transitions;
- observation source, freshness, quality and result;
- postcondition and verification result;
- integrity references, missing/contradictory events and coverage gaps;
- status `PASS`, `FAIL`, `BLOCKED`, `NOT_RUN`, reviewer/approval references.

Minimize sensitive raw data and follow approved privacy/retention rules.

## 14. Scenario suite

| ID | Scenario | Expected result |
|---|---|---|
| D01 | Valid digital mutation | Admission + exact independent read-back |
| D02 | Wrong resource/action/audience | Reject; no mutation |
| D03 | Invalid integrity/issuer | Reject; no mutation |
| D04 | Expired/not-yet-valid | Reject |
| D05 | Revoked before commit | Reject |
| D06 | Replay/duplicate | Prevent or detect duplicate side effect |
| D07 | Delegation expansion | Reject; child cannot exceed parent |
| D08 | Missing/conflicting context | Indeterminate/deny; no protected execution |
| D09 | Success response but mismatched read-back | Not verified/contradictory |
| D10 | Direct bypass | Block or report boundary coverage failure |
| P01 | Valid simulated pump | Actuator and process predicates independently verified |
| P02 | Stale/invalid sensor | Reject/indeterminate; no unsafe command |
| P03 | Interlock/fault active | Local rejection; no effect claimed |
| P04 | Authority mismatch/revocation | Reject before commit |
| P05 | Command accepted, actuator does not move | Execution reported; effect not verified |
| P06 | Actuator moves, process predicate fails | Partial/not verified |
| P07 | Observer unavailable | Indeterminate |
| P08 | Contradictory observations | Contradictory; simulator disposition applies |
| X01 | Missing audit event | Gap detected; completeness assertion fails |
| X02 | Clock skew/out-of-order | Sequence/commit reconstructs or flags ambiguity |
| X03 | Revocation/cancellation race | Declared commit semantics observed |
| X04 | Evidence tampering | Integrity verification fails |
| X05 | Partial execution/timeout | Partial/outcome unknown; no false success |
| X06 | Cross-boundary correlation | Actor/constraints/operation linkage retained |

These identifiers supplement—not replace—Security Law Book T01–T20 and expanded S01–S30.

## 15. Test oracle and status

Compare independently: authority decision; boundary admission; execution; observed effect; verification; evidence integrity/completeness.

- `PASS`: all assertions and evidence requirements pass.
- `FAIL`: assertion violated or unauthorized effect occurs.
- `BLOCKED`: precondition/environment prevents meaningful run.
- `NOT_RUN`: not executed.
- `INDETERMINATE`: evidence/outcome cannot support conclusion; never a pass.

No aggregate score may hide unauthorized effect, bypass, false verification or evidence-integrity failure.

## 16. Metrics

Report boundary paths identified/tested; blocked/detected bypasses; unauthorized effects (target zero); invalid/revoked/replayed artifacts accepted (target zero); false `VERIFIED` (target zero); effect-verification coverage/independence; revocation ordering; evidence completeness/reconstruction; indeterminate outcomes; stage latency; simulator fault coverage. These are experiment criteria, not production SLAs/certification.

## 17. Exit criteria

Ready for review only when:
- both boundaries/trust boundaries documented;
- scenario inputs, outcomes and oracles deterministic;
- decision/enforcement/execution/effect states separate;
- independent digital read-back and physical observation defined;
- negative, race, bypass and evidence-fault cases included;
- reconstruction exposes all evidence gaps;
- unexecuted cases and limitations explicit;
- no production or architecture adoption claim.

## 18. Limitations

Simulation is not physical safety/field reliability evidence. Test adapters cannot establish universal coverage. Observation independence may be imperfect. Revocation semantics are boundary/commit-specific. Policy lifecycle, authority hierarchy, key lifecycle and canonical artifact remain unresolved. Post-transfer sovereignty is out of scope. No runtime tests are claimed by this specification.

## 19. Deliverables when implementation is separately authorized

1. Versioned topology/configuration.
2. Deterministic digital and physical simulator.
3. Authority fixtures/rule set.
4. Boundary validators/adapters.
5. Scenario runner/test oracle.
6. Evidence records/integrity checks.
7. Reconstruction report and gap register.
8. Verification summary and approval record.

## 20. Next gate

This specification is subordinate to constitutional Sources, approved ADRs and roadmap. Any authority conflict blocks progression.

**Next gate:** explicit authorization for bounded experiment implementation and test environment. Then:

**Specification approval → Implementation → Compilation (where applicable) → Verification → Approval → Next milestone**

No production deployment, architecture freeze or broad subsystem integration follows automatically.
