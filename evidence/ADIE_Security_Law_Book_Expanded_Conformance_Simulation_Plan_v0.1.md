# ADIE Security Law Book — Expanded Conformance Plan and Scenario Simulation

**Investigation ID:** `ADIE-SEC-INV-001-P1`  
**Companion source:** `ADIE_Security_Law_Book_Final_Research_Proposal.md`  
**Document version:** 0.1.0  
**Date:** 26 October 2026  
**Status:** Draft for review; not approved  
**Implementation authorization:** None  
**Execution type:** Contract-level tabletop simulation and test-oracle analysis; no production engine, deployed ADIE component, physical device, or external provider was executed.

---

## 1. Purpose and decision boundary

This document expands the complete T01–T20 conformance suite from the Path 1 Security Law Book proposal and applies it to dynamic authority changes, permission revocation, task cancellation, actor suspension, policy-version changes, queued and in-flight operations, external providers, physical effects, and historical reconstruction.

The purpose is to determine whether the proposed Security Law Book contract adequately specifies expected behavior before architectural approval. This document does not approve the proposal, select a policy engine, finalize a schema, or authorize implementation.

### 1.1 Simulation method and evidence status

The scenarios below were evaluated against the normative principles in the source proposal and the frozen ADIE security reference model. The simulation checks whether each scenario has a defined expected result, whether the proposal currently supports that result, and what contract is still missing.

This is **not** an executable integration test. No engine was run, no policy bundle was compiled, no revocation endpoint was invoked, and no real actuator/provider was controlled. Accordingly:

- `CONTRACT-COVERED` means the proposal already states a sufficiently clear principle for the case at this level, not that implementation passed.
- `CONTRACT-GAP` means the expected behavior needs an explicit specification rule or lifecycle contract.
- `CONDITIONALLY-COVERED` means the proposal establishes the principle but leaves a material parameter or transition undefined.
- `NOT-TESTED-RUNTIME` means actual implementation behavior remains unverified.

No runtime PASS is claimed in this document.

## 2. Governing reference model

The fixed ADIE security model is:

**Security Law Book → Dynamic Authority Derivation → Portable Authority → Independent Enforcement at Every Execution Boundary → Verified Effect → Audit**

The Security Law Book is the authoritative normative source. It defines rule meaning, authority, applicability, constraint relationships, lifecycle, and approved semantics. It is not synonymous with a Rego/Cedar/XACML policy package, and an evaluator result alone does not establish authority legitimacy or prove that an effect occurred.

### 2.1 Explicitly separate five state dimensions

These dimensions must not be collapsed into a single `authorized` boolean or a single task status.

| Dimension | Question answered | Examples of state/evidence | Must not be confused with |
|---|---|---|---|
| **Policy-state consistency** | Which approved rule/policy state governs this evaluation, and is it internally consistent? | Canonical rule versions, package digest, approval provenance, activation/effective-time state, evaluator profile | Whether the actor is currently authorized; whether the task is active |
| **Authority validity** | Is this actor, delegation, capability, and requested operation authorized under the applicable constraints at the relevant enforcement point? | Actor identity/evidence, scope, operation, resource, delegation, constraints, revocation state, validity window | Task cancellation; provider acceptance; physical success |
| **Task lifecycle** | Is this task permitted to proceed, and what is its lifecycle state? | Created, accepted, running, cancellation-requested, cancelled, completed, failed, outcome-unknown, suspended/escalated | Actor-wide permission; proof that an external effect stopped |
| **Execution state** | Has an operation been admitted, dispatched, accepted, started, committed, stopped, or left uncertain? | Execution/command ID, queue state, idempotency key, provider acknowledgement, dispatch/commit evidence | Authorization decision; task state; verified effect |
| **Verified effect** | What actually happened in the target environment, and what evidence establishes it? | Independent device feedback, transaction status, read-back, provider evidence validated to the required assurance level | `PERMIT`, `SUCCESS` response, cancellation acknowledgement, or command dispatch |

A decision may be valid for the policy state used when it was evaluated and yet not be sufficient authority for a later operation. A task may be cancelled while an external operation is already committed. A provider may acknowledge cancellation without proving that the effect was prevented. The specification must preserve these distinctions.

### 2.2 Proposed lifecycle vocabulary (not yet canonical)

For simulation purposes, use the following task/execution states as distinct concepts, not as a finalized enum:

- **Task:** `CREATED`, `ACCEPTED`, `RUNNING`, `CANCELLATION_REQUESTED`, `CANCELLED`, `COMPLETED`, `FAILED`, `SUSPENDED`, `ESCALATED`, `OUTCOME_UNKNOWN`.
- **Operation:** `PLANNED`, `AUTHORIZATION_CHECKED`, `ADMITTED`, `QUEUED`, `DISPATCHED`, `PROVIDER_ACCEPTED`, `STARTED`, `COMMITTED`, `STOP_REQUESTED`, `STOPPED`, `EFFECT_VERIFIED`, `REJECTED`, `FAILED`, `OUTCOME_UNKNOWN`.
- **Policy state:** `DRAFT`, `VALIDATED`, `APPROVED`, `PUBLISHED`, `ACTIVE`, `SUSPENDED`, `WITHDRAWN`, plus effective-time/version identity as separately defined attributes.
- **Authority state:** `VALID`, `REVOKED`, `EXPIRED`, `SUSPENDED`, `OUT_OF_SCOPE`, `INDETERMINATE` (proposed vocabulary only).

A final specification may choose different names, but it must preserve the distinctions and define legal transitions.

## 3. Full proposed Security Law Book capability set

The following logical responsibilities are retained from the last proposal. They are logical capabilities, not a proposed count of services or deployables.

| Capability | Required responsibility | Revocation/cancellation relevance | Conformance evidence expected |
|---|---|---|---|
| **A. Rule Authority and Governance** | Establish who may create, approve, amend, suspend, or withdraw rules; preserve approval provenance. | Reject illegitimate revocations or policy changes; distinguish authorized governance action from an untrusted message. | Issuer authority, ownership, approval and transition evidence. |
| **B. Canonical Rule Model** | Represent rule identity/version, authority, ownership, applicability, scope, effect/constraint, relationships, combination semantics, lifecycle, validity interval, validation and integrity references, executable binding. | Express actor/operation/resource scope and the consequences of changed constraints. | Canonical rule fixtures and schema/semantic validation. |
| **C. Applicability and Authority Relationships** | Determine which rules apply and how legitimate authority relationships constrain one another. | Determine whether a revoking authority can govern the actor/task/operation and whether a lower authority can override it. | Applicable-rule set and authority relationship evidence. |
| **D. Conflict and Combination Semantics** | Define compatible constraints, prohibitions, conflicts, missing/unsupported information and non-resolution behavior. | Avoid last-writer-wins or arrival-order resolution; define restrictive/indeterminate outcomes. | Explicit combination results and negative tests. |
| **E. Rule Lifecycle and Provenance** | Maintain identity, approval history, version, effective status and authorized transitions. | Distinguish approval from activation; specify suspension, withdrawal, prospective/retroactive effects and history. | Version lineage, transition authorization, governance/effective times. |
| **F. Policy Compilation and Publication** | Transform approved rules into executable packages while binding the package to exact canonical source versions. | Ensure updated/revoked rules reach the policy state required at enforcement boundaries; prevent unapproved packages becoming active. | Deterministic transformation evidence, package/source binding, activation/rollback records. |
| **G. Conformance and Integrity Verification** | Verify package integrity and whether evaluator behavior preserves canonical semantics. | Test stale packages, unsupported constructs, ordering, revocation and cross-platform behavior. | T01–T20 evidence plus expanded scenarios; integrity and semantic-conformance records. |
| **H. Decision-Reconstruction Metadata** | Identify rules, versions, evaluator profile and relevant inputs required to reconstruct a decision. | Preserve why an operation was permitted/denied and what happened before/after revocation or cancellation. | Historical decision package, provenance, replay comparison, retention controls. |

### 3.1 Minimum proposed canonical rule information

A rule, directly or through authoritative references, should identify:

1. Stable rule identifier.
2. Rule version.
3. Authority reference.
4. Ownership.
5. Approval provenance.
6. Rule class.
7. Applicability conditions.
8. Subject and target scope (actors, resources, operations, capabilities).
9. Normative effect or constraint.
10. Authority relationships.
11. Combination semantics.
12. Effective lifecycle state.
13. Validity interval.
14. Validation evidence.
15. Source-integrity reference.
16. Executable-package reference, where compiled.

This remains a proposed minimum, not a finalized schema. Revocation, task lifecycle, execution and effect evidence must not be smuggled into a policy rule's meaning without an approved data contract; the Security Law Book must define the rule semantics that downstream stages consume.

### 3.2 Required lifecycle and publication safeguards

Proposed rule lifecycle:

`Author → Validate Authority and Ownership → Validate Structure and Semantics → Analyze Conflicts → Approve → Version and Seal → Publish → Activate → Monitor and Review → Amend, Suspend, or Withdraw`

A package must not activate merely because it compiles or loads. Proposed publication conditions:

1. Authorized approval of source rules.
2. Structural and semantic validation.
3. Conflict/consistency analysis.
4. Validated transformation.
5. Conformance testing against canonical semantics.
6. Integrity verification.
7. Binding to identifiable source version and publication event.
8. Defined activation and rollback procedure.

Governance time (when an event is recorded) and effective time (when a rule is intended to govern) must be distinguished.

### 3.3 Conflict and outcome contract from the proposal

- No applicable authorization path must not create implicit permission; proposed authorization default is `DENY`.
- An applicable binding prohibition cannot be overridden merely by a conflicting permit; its effect follows approved authority relationships and rule semantics.
- Compatible mandatory constraints must all be satisfied.
- The evaluator must not invent precedence based on incidental order or deployment configuration.
- Missing mandatory inputs, invalid attributes, evaluation errors, unsupported semantics, untrusted rule sources, and failed validation require explicit outcomes.
- Incompatible policy versions must not be mixed silently.

Proposed ADIE outcomes:

| Outcome | Meaning and proposed handling |
|---|---|
| `PERMIT` | Required authorization conditions satisfied; proceed only to the next approved stage, not straight to an assumed effect. |
| `DENY` | A binding constraint prohibits the action or no valid authorization path exists; reject. |
| `NOT_APPLICABLE` | No rule in the evaluated scope applies; apply enclosing-policy semantics and never treat this alone as permission. |
| `INDETERMINATE` | Trustworthy decision cannot be established; do not authorize the protected action. |
| `INVALID_POLICY_SET` | Structural, semantic, or governance validation fails; reject publication/activation. |
| `STALE_POLICY_STATE` | Required approved/effective state cannot be established; apply explicit stale-state failure behavior, never implicit permission. |

These are proposed ADIE-level outcomes; they do not assert that a particular engine has identical names or semantics.

## 4. Full T01–T20 suite retained from the proposal

The following test names and core oracles preserve the last proposal. Expanded tests in Section 5 add revocation/cancellation coverage without silently replacing T01–T20.

| Test | Scenario | Required ADIE result |
|---|---|---|
| **T01 — Explicit authorization** | A valid permit applies and no binding constraint prohibits the action. | `PERMIT`. |
| **T02 — Default denial** | No applicable rule establishes authorization. | `DENY`. |
| **T03 — Explicit prohibition** | A binding prohibition and a permit both apply. | `DENY`, under approved authority and combination semantics. |
| **T04 — Compatible constraints** | Multiple applicable mandatory constraints are satisfied. | Decision follows approved authorization semantics. |
| **T05 — Missing mandatory attribute** | A required attribute is unavailable. | No implicit permit; defined denial or indeterminate outcome. |
| **T06 — Invalid attribute type** | An input violates the declared schema. | Reject invalid request or return defined indeterminate outcome. |
| **T07 — Policy evaluation error** | A required policy encounters an evaluation error. | Do not silently ignore a mandatory constraint. |
| **T08 — Unsupported policy construct** | Evaluator cannot implement required semantics. | Reject package or return defined indeterminate outcome. |
| **T09 — Authority conflict** | Applicable rules conflict and no approved precedence relationship resolves them. | Do not invent precedence; produce defined conflict outcome. |
| **T10 — Policy-order permutation** | Same rules are supplied in different incidental file order. | Result unchanged unless an explicitly approved ordering rule applies. |
| **T11 — Policy version change** | New version is approved while an evaluation is in progress. | Evaluation uses an identified, internally consistent policy state. |
| **T12 — Stale policy package** | A package is no longer authorized for use. | Apply explicit stale-state failure behavior. |
| **T13 — Invalid package signature** | Package fails configured integrity/authenticity verification. | Do not activate package. |
| **T14 — Unapproved package** | Structurally valid package lacks valid approval provenance. | Do not activate package. |
| **T15 — Historical reconstruction** | Same request, inputs, policy versions, and evaluator profile are replayed. | Reproduce original result to defined determinism level. |
| **T16 — Semantic transformation** | Canonical rule is compiled into executable representation. | Conformance tests establish preservation of approved semantics. |
| **T17 — Schema evolution** | Schema change affects policies previously considered valid. | Detect and review affected policies before activation. |
| **T18 — Withdrawal** | Rule is withdrawn while older decisions remain relevant to reconstruction. | Preserve old version/history; apply new lifecycle state prospectively according to approved effective-time rules. |
| **T19 — Cross-platform representation** | Participating platform consumes a rule package through approved contract. | Platform must not reinterpret or broaden approved authority. |
| **T20 — Repeated evaluation** | Identical pinned inputs and versions evaluated repeatedly. | Equivalent outcomes and consistent decision metadata. |

### 4.1 T15 and T20 are separate requirements

- **T20 is repeatability:** under the same pinned inputs, policy state, evaluator profile and relevant configuration, repeated decision evaluations produce equivalent security outcomes and consistent substantive decision metadata.
- **T15 is historical reconstruction:** a past decision is reconstructed using the original approved policy state, evaluator profile and relevant historical evidence.

T20 does not prove that historical evidence was retained. T15 does not prove that a current implementation is consistently repeatable for all pinned inputs. Both need explicit comparison criteria. Security-significant outcome and decision basis must be compared; replay timestamps and newly generated correlation IDs may differ if the specification marks them as non-semantic metadata. Repeating an evaluation is not permission to repeat an external side effect.

## 5. Expanded scenario suite (S01–S30)

Each scenario states its initial conditions, event sequence, expected oracle, applicable capabilities/rules, and contract-level result. “Applicable Txx” is provided, but this suite is not reduced to the original four tests.

### S01 — All actor permissions revoked before execution

- **Initial state:** Task is accepted; actor has authority to read a sensor, evaluate it, and activate an actuator.
- **Events:** A legitimate governing authority revokes all relevant permissions before the command's execution boundary.
- **Expected oracle:** The protected command is rejected; the task is blocked/cancelled/suspended according to its defined lifecycle; no effect is reported without evidence. The rejection records the governing policy state and revocation evidence.
- **Capabilities/rules:** A, B, C, E, H; authority validity; execution-boundary enforcement; no stale authority reuse.
- **Txx:** T01, T02, T11, T12, T15, T20; expanded S01.
- **Status:** `CONDITIONALLY-COVERED`; runtime not tested. Revocation propagation and the exact validity-check contract need specification.

### S02 — Partial permission revocation

- **Initial state:** Actor may monitor, report, activate and deactivate a pump.
- **Events:** Activation is revoked; monitoring and reporting remain allowed.
- **Expected oracle:** Monitoring/reporting may continue only if independently authorized; activation is denied. The system must not broaden or erase remaining authority by assumption.
- **Capabilities/rules:** B, C, D, H; action-specific scope.
- **Txx:** T01–T05, T09, T11, T19, T20; expanded S02.
- **Status:** `CONDITIONALLY-COVERED`; per-operation capability semantics and task replanning rules need specification.

### S03 — Safety-relevant permission revoked but a task depends on it

- **Initial state:** Task requires both activation and deactivation; actor retains activation but loses deactivation authority.
- **Events:** Task plan is evaluated after partial revocation.
- **Expected oracle:** Do not assume task remains valid just because one operation remains permitted. Apply the approved task-feasibility/safety contract: reject, constrain, suspend, or escalate as specified. A separate safety controller may act only under its own authority.
- **Capabilities/rules:** B, C, D; authority validity; task lifecycle; safety authority boundary.
- **Txx:** T03–T05, T09, T11; expanded S03.
- **Status:** `CONTRACT-GAP`; proposal does not define whole-task feasibility or safety-specific authority.

### S04 — Revocation after decision but before execution

- **Initial state:** An evaluation returns `PERMIT` under an identified policy state.
- **Events:** Authority is revoked before command execution.
- **Expected oracle:** The execution boundary applies the approved authority-validity semantics and must not blindly treat the earlier permit as perpetual authority. If the revocation is effective for this operation, reject it; record the prior decision and later execution rejection as separate facts.
- **Capabilities/rules:** C, E, F, G, H; portable authority and execution boundary are downstream interfaces, not fully defined by Path 1.
- **Txx:** T11, T12, T15, T20; expanded S04.
- **Status:** `CONTRACT-GAP`; execution-time validity, revocation visibility and consistency guarantees must be specified across the six-stage model.

### S05 — Task cancelled before any operation starts

- **Initial state:** Task is accepted; no external operation has been dispatched.
- **Events:** Authorized requester cancels the task.
- **Expected oracle:** Task enters the defined cancelled state; no new operation is admitted; no effect is claimed. Actor permissions may remain intact for unrelated tasks.
- **Capabilities/rules:** A, C, E, H; task lifecycle separate from actor authority.
- **Txx:** T01, T02, T15, T20; expanded S05.
- **Status:** `CONTRACT-GAP`; task lifecycle is outside the detailed lifecycle specified in the proposal, which primarily defines rule lifecycle.

### S06 — Cancellation during preparation

- **Initial state:** Report or command has been prepared, but the effectful operation has not been admitted/dispatched.
- **Events:** Task cancellation becomes effective.
- **Expected oracle:** No subsequent effectful operation proceeds unless a separately approved rule permits it. Prepared data is retained/discarded/quarantined under data-handling policy; cancellation does not decide this automatically.
- **Capabilities/rules:** C, E, H; task lifecycle; execution boundary.
- **Txx:** T11, T15, T20; expanded S06.
- **Status:** `CONTRACT-GAP`; task-to-operation transition and cancellation boundary need specification.

### S07 — Cancellation races with dispatch

- **Initial state:** Command is about to be dispatched.
- **Events:** Cancellation and dispatch occur concurrently.
- **Expected oracle:** A defined serialization/commit boundary determines whether cancellation prevented dispatch or dispatch occurred first. If order cannot be established, report uncertainty; do not infer that no effect occurred from the local task status.
- **Capabilities/rules:** E, F, G, H; execution lifecycle and verified effect.
- **Txx:** T11, T15, T20; expanded S07.
- **Status:** `CONTRACT-GAP`; concurrency ordering and uncertainty semantics are not defined in the Path 1 proposal.

### S08 — External provider cannot cancel an in-flight operation

- **Initial state:** External provider has accepted and started an operation.
- **Events:** ADIE requests cancellation; provider reports unsupported/too-late cancellation.
- **Expected oracle:** Record cancellation requested and provider response distinctly; do not claim stopped or prevented. Apply approved compensation/safe-completion rules if available and authorized.
- **Capabilities/rules:** A, C, H; task lifecycle; execution state; verified effect.
- **Txx:** T15, T19, T20; expanded S08.
- **Status:** `CONTRACT-GAP`; provider cancellation contract and state mapping need specification.

### S09 — Cancellation after irreversible effect

- **Initial state:** Payment/report/physical action has committed and effect is verified.
- **Events:** Cancellation arrives after commitment.
- **Expected oracle:** Preserve the fact that the effect occurred. Mark cancellation as late or as applying only to remaining work. Any compensation is a new authorized task/operation with its own evidence.
- **Capabilities/rules:** E, H; immutable historical facts; verified effect.
- **Txx:** T15, T18, T20; expanded S09.
- **Status:** `CONDITIONALLY-COVERED`; historical preservation is present, but task/effect state semantics need specification.

### S10 — Queued command after revocation

- **Initial state:** Command was queued while authority was valid.
- **Events:** Permission is revoked before queue worker execution.
- **Expected oracle:** Queue acceptance does not confer indefinite authority. At the execution boundary, apply the defined validity/revocation contract; reject if no longer authorized. Preserve queue, revocation and rejection evidence.
- **Capabilities/rules:** C, E, F, G, H.
- **Txx:** T11, T12, T15, T20; expanded S10.
- **Status:** `CONTRACT-GAP`; queued-work authorization lifetime and execution-boundary contract need specification.

### S11 — Policy V2 activates during V1 evaluation

- **Initial state:** V1 is the identified state for an evaluation.
- **Events:** V2 is approved/activated while the evaluation is running.
- **Expected oracle:** Evaluation uses a coherent identified state, not an accidental mixture. Which version governs later operations follows explicit activation/effective-time semantics.
- **Capabilities/rules:** B, E, F, G, H.
- **Txx:** T11, T12, T15, T20; expanded S11.
- **Status:** `CONDITIONALLY-COVERED`; T11 covers consistency, but activation boundary and effect on in-flight execution remain open.

### S12 — Approved but not yet effective policy

- **Initial state:** V2 is approved at governance time but has a future effective time.
- **Events:** Operation is evaluated before V2's effective time.
- **Expected oracle:** Select governing version according to the approved effective-time contract, not approval timestamp or publication accident. Record governance time and effective time separately.
- **Capabilities/rules:** A, E, F, H.
- **Txx:** T11, T12, T18, T20; expanded S12.
- **Status:** `CONDITIONALLY-COVERED`; effective-time semantics are expressly outstanding in the proposal.

### S13 — Stale package remains available at one enforcement point

- **Initial state:** One platform has updated to the current package; another still has a superseded package.
- **Events:** The stale platform receives an operation requiring the current state.
- **Expected oracle:** Apply explicit stale-state handling; no implicit permit. Do not assume a package is valid because it is signed if it is no longer authorized for use.
- **Capabilities/rules:** E, F, G; package integrity and freshness are distinct.
- **Txx:** T12, T13, T14, T19, T20; expanded S13.
- **Status:** `CONDITIONALLY-COVERED`; stale-state detection and platform synchronization contract need specification.

### S14 — Actor suspended while tasks and delegations exist

- **Initial state:** Actor has multiple running/pending tasks and delegated authority.
- **Events:** Legitimate governance suspends actor.
- **Expected oracle:** Apply explicitly defined suspension scope to new work, pending tasks, queued commands, sessions and delegations. Do not assume all consequences from a single boolean without a transition contract.
- **Capabilities/rules:** A, C, E, H.
- **Txx:** T11, T12, T15, T19, T20; expanded S14.
- **Status:** `CONTRACT-GAP`; actor suspension propagation and task/delegation effects are not specified by the Security Law Book proposal.

### S15 — Delegation revoked while delegate holds old portable authority

- **Initial state:** A principal delegated scoped authority to an agent; the agent holds a previously issued authority artifact.
- **Events:** Delegation is legitimately revoked before a downstream operation.
- **Expected oracle:** Apply approved delegation-revocation semantics; possession of the old artifact alone must not exceed the authority still valid under the contract. Enforcement points need a defined way to establish validity or report indeterminate state.
- **Capabilities/rules:** A, C, E, H; portable authority stage dependency.
- **Txx:** T09, T11, T12, T15, T19, T20; expanded S15.
- **Status:** `CONTRACT-GAP`; portable-authority validity/revocation contract is outside the detailed Path 1 definition.

### S16 — Emergency action after ordinary authority revocation

- **Initial state:** Normal pump-control authority is revoked while the pump is running; a hazard condition arises.
- **Events:** System requires a stop or other safe-state action.
- **Expected oracle:** Apply separately approved safety authority and hazard-specific behavior. Do not automatically permit the agent or automatically disable all safety action. Verify actual safe state to the required assurance level.
- **Capabilities/rules:** A, B, C, D; verified effect; safety-domain contract.
- **Txx:** T03, T04, T09, T11, T19; expanded S16.
- **Status:** `CONTRACT-GAP`; safety authority and safe-state semantics are not defined by the Path 1 proposal and must not be invented here.

### S17 — Authority service unavailable at enforcement time

- **Initial state:** Enforcement point cannot establish the required current authority/revocation state.
- **Events:** A protected operation arrives during the outage.
- **Expected oracle:** Apply the explicit failure contract. For a protected operation where authority cannot be established, do not silently permit. Any safe-mode behavior must be explicitly approved for the operation class; it must not be a generic bypass.
- **Capabilities/rules:** C, D, G; stale/indeterminate outcome; enforcement boundary.
- **Txx:** T05, T07, T09, T12, T19; expanded S17.
- **Status:** `CONDITIONALLY-COVERED`; fail-closed disposition is a known project constraint, but exact availability/safety modes remain domain-specific and need specification.

### S18 — Forged or unauthorized revocation event

- **Initial state:** Actor is legitimately authorized; an untrusted source sends a purported revocation.
- **Events:** Enforcement system receives the message.
- **Expected oracle:** Verify issuer authority, integrity, scope and applicability before changing governance/authority state. Do not let an illegitimate actor disable protected work; record rejection.
- **Capabilities/rules:** A, B, C, E, G, H.
- **Txx:** T03, T09, T13, T14, T19; expanded S18.
- **Status:** `CONDITIONALLY-COVERED`; governance legitimacy and package integrity are present as principles, but a revocation-event contract needs specification.

### S19 — Conflicting authorities issue simultaneous changes

- **Initial state:** Two applicable authorities issue apparently conflicting rules or revocations.
- **Events:** Messages arrive in different orders at different enforcement points.
- **Expected oracle:** Apply approved authority relationships and combination semantics; arrival order and last-writer-wins must not invent precedence. If no legitimate resolution exists, return the defined conflict/indeterminate outcome.
- **Capabilities/rules:** A, C, D, E.
- **Txx:** T09, T10, T11, T19, T20; expanded S19.
- **Status:** `CONDITIONALLY-COVERED`; authority precedence versus evaluation precedence is recognized, but concrete relationships and conflict outcomes remain to be specified.

### S20 — Provider claims success but effect did not occur

- **Initial state:** Provider accepts a command and returns `SUCCESS`.
- **Events:** Independent read-back shows the actuator did not change state.
- **Expected oracle:** Record provider claim and verified effect separately; do not claim successful effect. Follow defined retry/escalation/compensation rules without blindly repeating a potentially non-idempotent action.
- **Capabilities/rules:** G, H; Verified Effect stage.
- **Txx:** T15, T19, T20; expanded S20.
- **Status:** `CONTRACT-GAP` for Path 1; verified-effect semantics belong to a later stage but must have an explicit interface contract.

### S21 — Provider acknowledges cancellation but action executes anyway

- **Initial state:** External operation is running; provider acknowledges cancellation.
- **Events:** Device or external system later confirms the effect occurred.
- **Expected oracle:** Do not infer prevention from acknowledgement. Preserve cancellation response and actual effect evidence; reconcile task state without rewriting history.
- **Capabilities/rules:** E, G, H; execution state and verified effect.
- **Txx:** T15, T19, T20; expanded S21.
- **Status:** `CONTRACT-GAP`; provider state and effect verification contracts need specification.

### S22 — Old command is replayed

- **Initial state:** Command was previously authorized for one task/execution instance.
- **Events:** Provider resubmits it after cancellation/revocation or after completion.
- **Expected oracle:** Bind authority/command to the applicable actor, task, operation, resource and execution instance; reject stale/replayed operations where the contract requires one-time or bounded use. Do not assume every command is inherently idempotent.
- **Capabilities/rules:** B, C, F, G, H.
- **Txx:** T11, T12, T15, T19, T20; expanded S22.
- **Status:** `CONTRACT-GAP`; replay prevention/idempotency semantics are not defined in Path 1.

### S23 — Multi-provider chain with revocation after analysis

- **Initial state:** Agent reads sensor; AI provider interprets it; connector queues an actuator command.
- **Events:** Authority is revoked after analysis but before connector execution.
- **Expected oracle:** Each execution boundary independently applies the approved authority contract. Queued work cannot inherit indefinite authority from the upstream evaluation. Correlate decision, revocation, queue item, command and effect evidence.
- **Capabilities/rules:** A–H as applicable; full six-stage model.
- **Txx:** T11, T12, T15, T19, T20; expanded S23.
- **Status:** `CONTRACT-GAP`; end-to-end correlation and cross-boundary freshness contract are not defined by Path 1 alone.

### S24 — Policy files reordered without semantic change

- **Initial state:** One canonical rule set and pinned evaluator profile.
- **Events:** Policy artifacts are reordered or loaded in a different incidental order.
- **Expected oracle:** Same security outcome and same substantive decision basis unless an explicitly approved ordering semantic applies.
- **Capabilities/rules:** B, D, F, G.
- **Txx:** T09, T10, T16, T20; expanded S24.
- **Status:** `CONTRACT-COVERED` at requirement level; runtime not tested.

### S25 — Same pinned decision evaluated repeatedly

- **Initial state:** Request, canonical rule versions, package digest, evaluator profile/configuration and relevant input evidence are pinned.
- **Events:** Repeat evaluation multiple times without changing any decision-relevant input.
- **Expected oracle:** Equivalent security outcome and consistent substantive decision metadata. Timestamps and unique replay IDs may differ only if designated non-semantic.
- **Capabilities/rules:** F, G, H.
- **Txx:** T15, T16, T20; expanded S25.
- **Status:** `CONDITIONALLY-COVERED`; equivalence relation and metadata comparison fields need specification; runtime not tested.

### S26 — Historical replay after policy withdrawal

- **Initial state:** Original decision used rule R-v1, later withdrawn and replaced by R-v2.
- **Events:** Historical decision is reconstructed after withdrawal.
- **Expected oracle:** Use original approved state and relevant evidence for reproduction; separately identify any current-policy reevaluation as a new evaluation, not historical reconstruction.
- **Capabilities/rules:** E, G, H.
- **Txx:** T15, T18, T20; expanded S26.
- **Status:** `CONDITIONALLY-COVERED`; historical state principle exists, but evidence-retention and replay contract need specification.

### S27 — Historical inputs include dynamic/external facts

- **Initial state:** Decision used risk score, device state, location, or external-provider data that can change.
- **Events:** Reconstruct later when live values differ.
- **Expected oracle:** Use retained/versioned original values or evidence references where lawful and necessary; do not silently query current values as substitutes. If evidence is missing, report reconstruction limitation.
- **Capabilities/rules:** B, G, H; data minimization and retention constraints.
- **Txx:** T05, T15, T17, T20; expanded S27.
- **Status:** `CONDITIONALLY-COVERED`; source proposal identifies relevant evidence, but retention, minimization and missing-evidence outcomes require specification.

### S28 — Canonical rule translation changes revocation meaning

- **Initial state:** Canonical rule states a restriction/revocation condition; compiled package is generated.
- **Events:** Translation accidentally weakens or drops the condition while package still passes structural compilation.
- **Expected oracle:** Package must fail semantic conformance and must not activate. Signature/integrity success cannot substitute for semantic preservation.
- **Capabilities/rules:** B, F, G.
- **Txx:** T08, T13, T14, T16, T17; expanded S28.
- **Status:** `CONTRACT-COVERED` at principle level; test corpus and semantic-equivalence method remain to be implemented/specifed.

### S29 — Safety action remains available through a separately authorized controller

- **Initial state:** Agent's ordinary authority is revoked; a separate safety controller has approved, scoped authority for a defined stop action.
- **Events:** Hazard condition occurs.
- **Expected oracle:** The agent's revoked authority is not restored. The safety controller acts only within its own approved authority and scope; actual safe state is verified.
- **Capabilities/rules:** A, B, C, D, G, H; safety boundary.
- **Txx:** T03, T04, T09, T19; expanded S29.
- **Status:** `CONTRACT-GAP` at Path 1 level; the scenario needs a separate approved safety-domain contract.

### S30 — Outcome unknown after network loss

- **Initial state:** Command is dispatched to provider; connection fails before acknowledgement or read-back.
- **Events:** Task cancellation/revocation occurs while operation status is unknown.
- **Expected oracle:** Record `OUTCOME_UNKNOWN` or the approved equivalent; do not report success, failure, or cancellation completion without evidence. Reconcile through safe, authorized status queries or compensation, without blind duplicate execution.
- **Capabilities/rules:** C, D, E, G, H; execution and verified effect.
- **Txx:** T05, T07, T12, T15, T19, T20; expanded S30.
- **Status:** `CONTRACT-GAP`; uncertainty and reconciliation semantics need specification.

## 6. Cross-scenario findings

### 6.1 Requirements sufficiently established at proposal level

The proposal already establishes these principles:

1. Incidental rule order must not invent semantics (T10).
2. Evaluations must use an identified, internally consistent policy state (T11).
3. Stale policy state must not result in implicit permission (T12).
4. Invalid or unapproved packages must not activate (T13–T14).
5. Canonical-to-executable transformation must preserve approved semantics (T16).
6. Withdrawal must preserve historical versions and use approved effective-time semantics (T18).
7. Participating platforms must not broaden or reinterpret authority (T19).
8. Historical reconstruction and repeated evaluation are distinct requirements (T15/T20).
9. Missing mandatory information, evaluation errors, unsupported semantics, and unresolved authority conflicts require explicit outcomes.
10. A valid package signature proves integrity relative to signed content, not correctness of rule translation.

These are design-level requirements only; none is runtime-verified here.

### 6.2 Material contract gaps revealed by simulation

The last proposal focuses on the Security Law Book. It explicitly says that the full authority-derivation service, portable-authority format, enforcement topology and audit architecture are outside the current investigation. The scenarios nevertheless show that Path 1 must define enough interfaces to prevent downstream components from changing its semantics.

The following must be specified before implementation of the relevant behavior:

1. **Revocation semantics:** scope, issuer authority, effective time, propagation expectations, impact on already issued authority, and behavior when freshness cannot be established.
2. **Policy activation contract:** approval versus publication versus activation versus effective time; what happens to evaluations already in progress and commands already admitted.
3. **Task lifecycle contract:** task cancellation, suspension, escalation, completion, cancellation acknowledgement, late cancellation, and transition rules.
4. **Execution-boundary contract:** what evidence must be checked when a command is admitted, queued, dispatched and executed; what makes an authority artifact usable at each point.
5. **Delegation and portable authority contract:** binding to actor/task/operation/resource/scope, validity, revocation, replay, and stale-state behavior.
6. **Concurrency/ordering contract:** how the system determines whether revocation/cancellation took effect before or after a particular execution commit point; do not rely on unqualified timestamps alone.
7. **Outcome-unknown contract:** what to record and how to reconcile when external execution cannot be confirmed.
8. **Verified-effect contract:** acceptable evidence for physical/external effects; provider claims versus independent read-back; assurance levels by operation risk.
9. **Compensation/idempotency contract:** when retries or compensating operations are permitted, how they are separately authorized, and how duplicate side effects are prevented.
10. **Safety-domain contract:** emergency-stop/safe-state authority and behavior must be approved for each relevant domain; no generic override or generic disable-all behavior is inferred.
11. **Decision evidence and privacy contract:** which input/context facts are retained or referenced, integrity and access controls, retention periods, and how missing evidence limits reconstruction.
12. **Cross-platform revocation propagation:** participating platforms and enforcement points must have defined freshness/availability behavior; one component's correct revocation handling is insufficient if another executes stale authority.

### 6.3 Fail-closed must not be overgeneralized

For a protected operation where authority cannot be established, the proposed default is no implicit permission and the required disposition provider remains fail-closed. However, this does not specify the physical safe-state behavior for every device or domain. A safety controller may need separately authorized actions. The architecture must not bypass a fail-closed disposition provider, and must not invent a universal emergency exception.

## 7. Revised conformance organization

Keep T01–T20 as the baseline Security Law Book suite. Add the expanded S01–S30 scenarios as a **cross-stage conformance layer**. These labels are scenario identifiers, not a replacement T-number series and not a claim that every requirement belongs exclusively to Path 1.

For each test run, the evidence record should include at least:

- Test and scenario ID/version.
- Canonical rule set and versions.
- Authority/governance state and provenance.
- Policy package identity/digest and activation/effective-time state.
- Evaluator and transformation versions/configuration.
- Normalized request and relevant input/context evidence references.
- Task ID, operation ID, execution ID, actor, resource and scope.
- Event sequence and ordering/commit evidence where applicable.
- Expected ADIE outcome and actual raw engine result.
- ADIE-mapped result and diagnostics.
- Execution/provider result separately from authorization result.
- Verified-effect result and evidence source separately from provider claim.
- Historical reconstruction comparison where applicable.
- Status classification: pass/fail/blocked/not-run; never infer pass from successful compilation alone.
- Known limitations, missing evidence, and approval reference.

Data retention and logging must follow approved privacy, security, and retention rules; this list is a schema proposal for test evidence, not authorization to collect all raw data indefinitely.

## 8. Execution results of this simulation

### 8.1 Results summary

| Result class | Count | Interpretation |
|---|---:|---|
| `CONTRACT-COVERED` | 2 | T10/order invariance and semantic transformation principle are clear at the proposal level. Runtime still untested. |
| `CONDITIONALLY-COVERED` | 12 | A principle exists, but a parameter, lifecycle transition, evidence contract or downstream interface remains open. |
| `CONTRACT-GAP` | 16 | The proposal does not yet define the necessary behavior sufficiently. |
| **Runtime tests executed** | **0** | This was a contract-level tabletop simulation only. |
| **Runtime tests passed** | **0** | No runtime result is claimed. |

Counts classify the 30 scenarios once each according to the status assigned in Section 5. They are not an implementation quality score.

### 8.2 Simulated case conclusions

- A valid earlier `PERMIT` cannot be treated as perpetual authority; the relevant execution boundary must apply the approved validity contract.
- Permission revocation does not itself cancel a task, and task cancellation does not itself revoke the actor's other permissions. The consequences must be defined independently.
- Cancellation requested is not the same as cancellation accepted, execution stopped, or effect prevented.
- Queue acceptance and provider `SUCCESS` are not proof that a protected effect is currently authorized or physically occurred.
- A new policy version must not be mixed into an in-progress evaluation accidentally; activation and enforcement-time behavior remain specification decisions.
- Historical reconstruction must use the original approved policy state and relevant historical evidence; current-policy reevaluation is a different operation.
- Repeated evaluation must compare decisions, not blindly repeat side effects.
- An unavailable or untrusted authority state must not be converted into permission. Domain-specific safety actions require separately approved authority and behavior.
- Each participating execution boundary must preserve the ADIE rule meaning and cannot broaden authority.

## 9. Required specification work before approval

The simulation does not justify approving the proposal as a complete implementation specification. It supports keeping the engine-independent Security Law Book direction, while requiring a revision/addendum that explicitly records the cross-stage interfaces and gaps.

Recommended next specification package:

1. **Security Law Book Semantics:** canonical rule fields, rule classes, authority relationships, applicability, conflict/combination semantics, ADIE outcomes.
2. **Governance and Effective-Time Contract:** who may approve/activate/suspend/withdraw, lifecycle transitions, governance time versus effective time, rollback and historical semantics.
3. **Revocation and Staleness Contract:** scope, event legitimacy, propagation/freshness, existing authority artifacts, queued/in-flight work and unavailable-state handling.
4. **Task/Execution Lifecycle Contract:** task versus operation states, cancellation and suspension, commit points, concurrency, provider status and outcome-unknown behavior.
5. **Execution-Boundary and Portable-Authority Interface:** what each boundary must validate and what it may trust; binding, expiry, revocation, replay and delegation rules.
6. **Verified-Effect and Compensation Contract:** evidence grades, provider claims versus independent verification, idempotency/retry/compensation and safety-specific actions.
7. **Decision Reconstruction and Evidence Contract:** T15/T20 comparison criteria, historical evidence, integrity, minimization, retention, and limits.
8. **Conformance harness:** implement T01–T20 plus S01–S30 against the canonical test fixtures; then test candidate engines/adapters under identical fixtures.

These are proposed specification work items, not an approved change to the frozen roadmap. The responsible architectural authority must decide whether and how they are incorporated into the approved Path 1 process.

## 10. Decision recommendation

**Do not approve the current proposal as complete yet.** The proposal's direction remains coherent, but the simulation exposed material specification gaps around revocation, task lifecycle, execution-time validity, cancellation races, external effects and safety boundaries.

The appropriate current disposition is:

- Preserve the existing engine-independent Security Law Book proposal as the research baseline.
- Accept T01–T20 as the baseline proposed conformance suite, subject to final specification.
- Add S01–S30 as a cross-stage scenario suite and preserve T15/T20 as separate tests.
- Specify the gaps in Section 6.2 before implementation authorization.
- Do not select a production engine or claim conformance until executable tests and evidence exist.

**Status after this document:** Simulation completed at contract-analysis level; proposal approval remains pending; implementation is not authorized.
