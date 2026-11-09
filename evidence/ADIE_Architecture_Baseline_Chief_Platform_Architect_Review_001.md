# ADIE Architecture Baseline --- Chief Platform Architect Review

**Review ID:** ADIE-ARCH-REVIEW-001\
**Reviewed document:** ADIE Architecture Baseline Draft 0.1.0\
**Review date:** 2026-10-19\
**Active role:** Chief Platform Architect\
**Review type:** Architecture review\
**Decision:** **RETURN FOR REVISION --- NOT APPROVED**\
**Implementation authorization:** None

------------------------------------------------------------------------

## 1. Executive decision

The draft is a useful starting inventory of proposed ADIE
responsibilities, cross-platform boundaries, trust questions, and
unresolved security semantics. It is not yet an approvable architecture
baseline.

The draft correctly avoids selecting technologies or turning every
logical responsibility into a separate service. It also correctly
preserves the need for independent enforcement and recognizes that the
inspected Core contracts do not yet establish the complete
authority-decision contract required by the security reference model.

However, four blockers prevent approval:

1.  **The authoritative architecture baseline and ADR/roadmap status
    have not been reconciled.** The draft itself says the canonical
    Architecture Freeze, approved ADIE ADR set, and authoritative ADIE
    roadmap were not established from the available sources.
2.  **The relationship between ADIE and Core is not sufficiently
    resolved.** The ADIE product definition describes ADIE as the
    ecosystem's foundational digital infrastructure, while the supplied
    Core V1 document says Core V1 defines the functional baseline of the
    RedTap ADIE platform. The draft treats ADIE and Core as separate
    platform boundaries. This relationship must be resolved against the
    canonical platform hierarchy and authoritative platform definitions
    before it is fixed in architecture.
3.  **The proposed domains are not yet a sufficiently decisive component
    architecture.** They are responsibility categories, and the draft
    explicitly leaves component ownership, deployable boundaries, and
    key control-plane decisions open. That is acceptable for an initial
    exploration, but not for an architecture baseline intended to lead
    directly to implementation.
4.  **The milestone context must be reconciled.** Earlier project
    context records authorization for A3.3 --- ADIE Authority Foundation
    and Cross-Platform Dependency Mapping. The current draft labels
    itself as the next A3.4 baseline. The canonical roadmap and status
    must establish whether A3.3 is complete, superseded, or still
    blocking progression. This review does not silently reorder the
    roadmap.

The decision is therefore **Return for Revision**. No production
implementation or deployment is authorized.

------------------------------------------------------------------------

## 2. What passes review

### 2.1 Ecosystem federation principle --- Pass, with one clarification required

The draft preserves the principle that ecosystem platforms are
independently owned and interoperable, rather than merging their
responsibilities into a monolith.

This is consistent with the Ecosystem Definition and Platform Portfolio.
ADIE may provide a shared infrastructure foundation, but "foundational"
must not be interpreted as organizational authority over every
independent platform.

**Required clarification:** explicitly distinguish *foundational
infrastructure role* from *platform hierarchy or ownership*. The
architecture must not infer that ADIE can take over Core, NoredAm, or
other platform responsibilities.

### 2.2 No premature technology selection --- Pass

The draft does not prescribe a database, message broker, cloud provider,
container orchestrator, or microservice topology without evidence. It
correctly treats logical responsibility domains as distinct from
deployment units.

### 2.3 Core and NoredAm boundary caution --- Pass, subject to resolution of the ADIE/Core relationship

The draft correctly warns against treating Core
identity/session/permission claims as the full Security Law Book or
dynamic authority decision, and it preserves NoredAm's fail-closed
unavailable-provider behavior.

The existing A3.1 result was a targeted inspection, not exhaustive proof
that no suitable contract exists anywhere. The draft appropriately
preserves that qualification.

### 2.4 Frozen security reference model --- Pass as a requirement; architecture remains incomplete

The draft carries forward:

**Security Law Book → Dynamic Authority Derivation → Portable Authority
→ Independent Enforcement at Every Execution Boundary → Verified Effect
→ Audit**

It does not claim that the end-to-end chain is implemented. This is
correct.

------------------------------------------------------------------------

## 3. Blocking findings

### B-01 --- Canonical architecture and governance sources are not reconciled

**Severity:** Blocking\
**Finding:** The draft states that the canonical Architecture Freeze,
approved ADIE ADR set, and authoritative ADIE roadmap were not
established from the sources available.

**Why this blocks approval:** An architecture baseline cannot be
approved against an incomplete record of the current approved
architecture and roadmap. Without this reconciliation, the review cannot
establish whether a proposal contradicts a frozen decision, duplicates
an existing ADR, or skips an already-authorized milestone.

**Required correction:** 1. Identify the canonical Architecture Freeze
and its current version/status. 2. Identify the canonical ADR library
and search for decisions affecting ADIE, Core, identity, authority,
platform boundaries, and integration. 3. Identify the current canonical
roadmap and implementation order. 4. Reconcile the current operational
state, including A3.3 authorization and completion status. 5. Add a
traceability matrix linking each proposed decision to its governing
source or marking it as a genuinely new decision.

**Disposition:** Must be resolved before the baseline can be approved.

### B-02 --- ADIE/Core platform relationship is unresolved

**Severity:** Blocking\
**Finding:** The draft describes ADIE and Core as distinct platform
boundaries. The supplied `RedTap Core V1.md`, however, states: "RedTap
Core V1 defines the functional baseline of the RedTap ADIE platform."
The ADIE product definition also describes ADIE as the foundational
platform, while the Platform Portfolio emphasizes independent platform
responsibilities.

These statements may reflect different document vintages or a
distinction between product/platform identity and functional
implementation, but the available evidence does not authorize the
reviewer to assume that explanation.

**Why this blocks approval:** The relationship affects ownership of
identity, tenant boundaries, administration, shared infrastructure, and
which system ADIE integrates with versus contains.

**Required correction:** 1. Establish the controlling source and version
for ADIE's platform identity and Core's relationship to it. 2. Determine
whether Core is (a) an independent platform, (b) an implementation/core
subsystem within ADIE, or (c) another explicitly defined relationship.
3. Reconcile the wording in the architecture baseline with the
controlling sources. 4. Do not change the constitutional documents as
part of this architecture review. If the conflict cannot be resolved by
the established hierarchy, escalate to the authority that owns the
conflict.

**Disposition:** Blocking until resolved by authoritative source
reconciliation.

### B-03 --- The document does not yet define the component architecture

**Severity:** Blocking\
**Finding:** The eight domains are explicitly described as
responsibility groupings, not components. The document leaves open
whether ADIE is one deployable, multiple deployables, or a logical
platform hosted in a limited number of deployables. It also leaves
ownership of key control-plane responsibilities open.

**Why this blocks approval:** A baseline may defer low-level
implementation details, but it must define enough architectural
structure and responsibility ownership to constrain the first
implementation specification. This draft is primarily a domain inventory
and decision backlog.

**Required correction:** Add a component-and-boundary view that
identifies, at minimum: - ADIE-owned responsibilities versus
platform-owned responsibilities; - which responsibilities are logical
capabilities only and which require an explicit component owner; - trust
and data boundaries between those owners; - contract direction and
dependencies; - which architectural decisions must be settled now versus
deferred to a later ADR.

This correction does **not** require choosing a microservice topology or
technology stack. It requires a clear logical component model and
ownership boundaries.

**Disposition:** Blocking for approval as an architecture baseline.

### B-04 --- A3.3/A3.4 sequencing is not verified

**Severity:** Blocking\
**Finding:** Prior project context records A3.3 --- ADIE Authority
Foundation and Cross-Platform Dependency Mapping as authorized. The
reviewed draft is presented as A3.4 --- ADIE Architecture Baseline, but
it does not establish A3.3 completion or an approved roadmap transition.

**Why this blocks approval:** The engineering workflow forbids silently
skipping, reordering, or modifying the approved roadmap.

**Required correction:** Reconcile A3.3 against the canonical roadmap
and current project state. If A3.3 is complete, cite its accepted
evidence. If incomplete, identify the exact dependency and determine
whether this baseline is an authorized input to that milestone or a
separate milestone. Do not rename, close, or supersede milestones by
assumption.

**Disposition:** Blocking for progression to specification or
implementation.

------------------------------------------------------------------------

## 4. Important design findings

These items require explicit treatment in the next revision. Some may be
resolved as approved decisions; others may remain open if the baseline
clearly identifies the owner, consequence, and blocking status.

### D-01 --- Separate control plane from platform execution

The draft should make this boundary explicit in its component view:

-   ADIE may coordinate contracts, governance context, trust and
    decision exchange if those responsibilities are approved.
-   The owning platform remains responsible for its domain operations.
-   The execution boundary remains responsible for independent
    enforcement.
-   Verification must establish observed effect rather than merely
    accepting an execution response.

This is a proposed architectural constraint consistent with the frozen
security reference model; the exact component allocation remains to be
decided.

### D-02 --- Dynamic authority decision ownership must be explicit

The draft correctly leaves the authority decision producer unresolved.
It must not be treated as a non-blocking implementation detail.

Before any authority-dependent execution is implemented, the
architecture must define the authoritative decision producer, decision
semantics, policy provenance, subject/action/resource binding, execution
correlation, validity and revocation behavior, and failure semantics.

If the owner or semantics cannot be determined within existing
constitutional authority, escalate rather than inventing a new service
or assigning ownership by convenience.

### D-03 --- Portable authority is not yet a contract

The draft lists relevant concerns but does not yet define a contract.
Before implementation, the approved design must specify the artifact's
intended purpose, issuer, audience, subject, permitted
operation/resource scope, policy/provenance reference, validity,
correlation, integrity protection, revocation/staleness handling, and
rejection semantics. This is a requirement checklist, not a prescription
for a particular token format.

### D-04 --- Verification and audit need a shared correlation model

The architecture should identify the minimum relationship among: -
request/intent; - authority decision; - execution attempt; - executor
identity and enforcement result; - observed effect; - verification
outcome; - audit/evidence record.

The architecture need not prescribe the final storage technology now,
but it must establish whether correlation identifiers and provenance
cross the platform boundaries and who is responsible for producing each
record.

### D-05 --- Tenant boundaries need explicit treatment in cross-platform calls

Core V1 makes tenant isolation mandatory. The baseline mentions it, but
does not yet define how tenant/resource context is conveyed, validated,
and denied across platform contracts. Cross-tenant access must not be
inferred from an authenticated identity or a registered platform.

### D-06 --- Negative-path handling needs a bounded contract

The negative-path list is useful but too broad to serve as an
implementation contract. The next architecture revision should classify
failures into: - authentication/trust failure; - policy/authority
failure; - contract/capability failure; - executor failure; -
effect-verification failure; - evidence/audit failure.

For each class, define whether execution is denied, stopped, retried,
compensated, or escalated. Safety-critical exceptions must be explicit
and must not emerge from generic retry logic.

------------------------------------------------------------------------

## 5. Required revision set

Return the draft for a targeted revision. Do not expand the scope into
implementation.

1.  **Source reconciliation:** record canonical Architecture Freeze, ADR
    library, roadmap, implementation order, and current state with
    version/status.
2.  **ADIE/Core boundary resolution:** resolve the conflict between the
    Core V1 functional-baseline statement and the proposed
    separate-platform model.
3.  **Milestone reconciliation:** establish A3.3 status and the
    authority for the current A3.4 label.
4.  **Logical component architecture:** show responsibility ownership,
    dependency direction, contract boundaries, and trust/data boundaries
    without prematurely prescribing deployment technology.
5.  **Decision register refinement:** classify each item as:
    -   decision required for baseline approval;
    -   decision safely deferred to a later ADR;
    -   unresolved issue requiring escalation.
6.  **Traceability:** map each architectural constraint to an
    authoritative source, or mark it as a proposed decision awaiting
    approval.
7.  **Approval criteria:** make the acceptance criteria testable and
    ensure no blocking ambiguity remains in the first implementation
    boundary.

------------------------------------------------------------------------

## 6. Decision record

  -----------------------------------------------------------------------
  Review dimension                    Result
  ----------------------------------- -----------------------------------
  Ecosystem federation principle      Pass, with wording clarification

  Core/NoredAm ownership caution      Pass, subject to ADIE/Core
                                      resolution

  Security reference model            Pass
  represented                         

  Authority decision ownership        Open and blocking

  Canonical Architecture Freeze / ADR Not demonstrated; blocking
  reconciliation                      

  ADIE/Core platform relationship     Conflict unresolved; blocking

  Component architecture              Incomplete; blocking

  A3.3/A3.4 roadmap sequence          Not verified; blocking

  Technology/deployment selection     Correctly deferred

  Approval for implementation         **Denied / not authorized**
  -----------------------------------------------------------------------

### Final decision

**RETURN FOR REVISION --- ADIE Architecture Baseline Draft 0.1.0 is not
approved.**

The document is a sound exploratory starting point, but it is not yet a
sufficiently reconciled and decisive architecture baseline. The next
action is to resolve the blocking findings above and submit a revised
baseline for another Chief Platform Architect review.

No code, repository, deployment, roadmap, or constitutional source is
modified by this review.
