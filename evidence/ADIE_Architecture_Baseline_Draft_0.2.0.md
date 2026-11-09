# RedoudAm Intelligence Ecosystem --- ADIE Architecture Baseline

**Document ID:** ADIE-ARCH-BASELINE-DRAFT-002\
**Version:** 0.2.0-draft\
**Status:** Revised proposed architecture baseline --- pending Chief
Platform Architect approval\
**Revision date:** 2026-10-22\
**Scope:** ADIE logical architecture, the six-stage security structure,
subsystem boundaries, and decisions required before specification\
**Excluded:** Production implementation, deployment configuration,
technology selection, code changes, and changes to frozen roadmaps

## 1. Purpose and decision status

This revision incorporates the following explicit direction:

1.  **Core is a subsystem within ADIE**, not a peer platform outside
    ADIE.

2.  ADIE's responsibility model is organized around the six-stage
    security structure.

3.  Other subsystems and participating platforms operate under ADIE's
    control, security verification, and authorization while retaining
    their domain responsibilities and local execution obligations.

4.  The basic ADIE security/governance model is:

    **Security Law Book → Dynamic Authority Derivation → Portable
    Authority → Independent Enforcement at Every Execution Boundary →
    Verified Effect → Audit**

5.  A3.3 was a proposal, not an approved milestone sequence. It is not
    treated as an approved roadmap gate or required predecessor.

This is a proposed architecture baseline, not an approved Architecture
Freeze, approved ADR, implementation specification, or authorization to
deploy. Formal architecture approval remains required.

## 2. Evidence and decision classification

-   **Established source baseline:** directly stated by an available
    project source.
-   **User-directed architectural input:** explicit direction
    incorporated as a design constraint for this revision; it does not
    replace the formal architecture approval gate.
-   **Proposed architecture:** a design arrangement introduced here for
    approval.
-   **Open decision:** a question requiring formal decision or
    additional authoritative evidence.

### 2.1 Established source baseline

-   The RedoudAm Intelligence Ecosystem is a federation of platforms
    that preserve their identities and responsibilities while
    interoperating.
-   ADIE is defined as the ecosystem's foundational digital
    infrastructure environment.
-   Core V1 defines foundational capabilities including authentication,
    authorization, identity sessions, identity security, tenant/branch,
    licensing, user, role, permission, and tenant-configuration
    foundations.
-   NoredAm Intelligence Platform is an independent engineering
    platform. Its public naming does not authorize repository,
    namespace, or implementation-identity migration.
-   The six-stage security/governance model is the required baseline.
-   The previous Core-authority contract investigation was targeted. It
    did not establish a complete generic authority-decision contract in
    the inspected Core sources; it did not prove that no relevant
    contract exists elsewhere.
-   Existing NoredAm fail-closed behavior must not be weakened or
    bypassed by this architecture work.

### 2.2 Source limitation

Available sources include the Ecosystem Definition, Platform Portfolio,
ADIE Product Definition, Core V1 functional baseline, AIOE governance
source, public naming authority, and prior targeted Core/NoredAm
findings. The canonical Architecture Freeze, full canonical ADR library,
and current approved ADIE roadmap have not yet been reconciled in this
document. This remains a traceability task for approval; no claim is
made that those sources do not exist.

## 3. Architectural objective

**Proposed objective:** ADIE is the foundational digital infrastructure
platform that governs the shared identity, security, authorization,
policy, and cross-platform operating environment for the ecosystem. Core
is an internal ADIE subsystem providing foundational operational
capabilities. Other ADIE subsystems and participating platforms consume
ADIE-defined governance and security contracts while retaining their
domain functions and internal execution responsibilities.

ADIE enables interoperability without allowing platform registration,
identity, network position, or successful request submission to bypass
authorization or execution-boundary enforcement.

The six stages define the required lifecycle of authority and effect.
They **do not automatically imply six services, six subsystems, or six
deployment units**.

## 4. ADIE responsibility model: the six-stage security structure

The six-stage structure is the organizing architecture. Individual
capabilities may be assigned to one or more components only after
component boundaries are approved.

`<escape>`{=html} Security Law Book ↓ Dynamic Authority Derivation ↓
Portable Authority ↓ Independent Enforcement at Every Execution Boundary
↓ Verified Effect ↓ Audit `</escape>`{=html}

### 4.1 Stage 1 --- Security Law Book

**Responsibility:** Define authoritative security and governance rules
against which identities, actions, resources, contexts, and execution
boundaries are evaluated.

Proposed ADIE responsibilities: - Govern the authoritative source and
lifecycle of security rules. - Preserve policy ownership, approval
provenance, version, and effective status. - Identify rules and
constraints applicable to an action. - Define deterministic policy
precedence and conflict resolution before implementation. - Account for
applicable identity, delegation, tenant, organizational, jurisdictional,
resource, task, context, risk, and runtime constraints.

**Core relationship:** Core supplies its owned identity, session,
tenant, role, and permission facts through explicit contracts where
applicable. Core permission records are not automatically equivalent to
the complete Security Law Book.

**Open decisions:** machine-readable representation; conflict semantics;
amendment and withdrawal lifecycle; version pinning; ownership of
domain-specific policy sources.

### 4.2 Stage 2 --- Dynamic Authority Derivation

**Responsibility:** Evaluate applicable constraints and derive effective
authority for a specific actor, action, target resource/capability,
context, and execution purpose.

Proposed ADIE responsibilities: - Resolve applicable policy and
governance context. - Consume authenticated identity and session facts
from their authoritative owners. - Apply approved deterministic
derivation semantics. - Produce an explicit decision with traceable
provenance. - Handle denied, indeterminate, stale, missing, or
unverifiable inputs according to approved failure rules. - Prevent
dependency order or platform convenience from becoming an independent
source of authority.

**Core relationship:** Core remains responsible for its defined
identity, session, user, role, and permission capabilities. ADIE
consumes them through explicit contracts; authentication or ordinary
permission claims must not silently become the complete
derived-authority decision.

**Blocking decisions:** authoritative decision producer, evaluation
semantics, policy-combination rules, conflict handling, and failure
outcomes. This draft does not authorize a new parallel authority
service.

### 4.3 Stage 3 --- Portable Authority

**Responsibility:** Represent an approved authority decision so it can
be carried to and validated by an applicable execution boundary without
losing provenance, scope, or constraints.

Proposed requirements: - Bind the artifact to issuer, subject, intended
action, target resource/capability, audience or execution boundary,
relevant context, policy provenance, validity, and execution correlation
as required by the approved contract. - Define integrity protection and
issuer trust. - Define expiry, revocation, staleness, replay, audience
mismatch, and validation-failure behavior. - Prevent reuse outside
approved scope. - Distinguish identity credentials, Core permission
claims, derived decisions, and portable authority artifacts.

**Blocking decisions:** artifact semantics, representation, issuer,
validator, trust distribution, revocation, and compatibility. No token
format or protocol is selected here.

### 4.4 Stage 4 --- Independent Enforcement at Every Execution Boundary

**Responsibility:** Ensure each component or external platform that can
perform an operation independently validates and enforces applicable
authority before permitting the effect.

Mandatory boundary rule: - Every applicable execution boundary validates
portable authority or an approved equivalent decision through a formally
defined contract. - The executor verifies issuer trust, scope, target,
validity, and relevant constraints. - Missing, invalid, expired,
revoked, stale, or unverifiable authority evidence does not become
implicit permission. - The owning platform retains responsibility for
local operational and safety constraints. - ADIE orchestration does not
substitute for enforcement inside the receiving platform, API,
connector, infrastructure target, or physical executor. - Enforcement
outcomes are correlated to the decision and execution attempt.

**NoredAm relationship:** NoredAm remains an independent engineering
platform. Its execution boundary preserves existing fail-closed behavior
until an approved integration contract is implemented and verified. ADIE
must not bypass or weaken that boundary.

**Other platform relationship:** participating platforms and subsystems
operate under ADIE's approved governance, security verification, and
authorization requirements while preserving domain responsibilities and
local enforcement obligations.

**Open decisions:** common enforcement contract; executor trust
bootstrap; emergency/safety exception semantics; revocation propagation;
conformance tests.

### 4.5 Stage 5 --- Verified Effect

**Responsibility:** Establish whether the intended operation produced
the required observed effect, rather than treating request acceptance or
successful command submission as proof of completion.

Proposed requirements: - Define intended effect and verification
criteria before execution where practicable. - Distinguish request
acceptance, execution attempt, executor response, and verified effect. -
Identify evidence producer and verification method. - Correlate observed
state with original request, authority decision, and executor. -
Represent failure, partial completion, timeout, ambiguity, and
contradictory observations explicitly. - Require verification
independence appropriate to risk and execution environment.

**Open decisions:** verification independence, evidence authenticity and
freshness, acceptable uncertainty, compensation policy, storage
ownership.

### 4.6 Stage 6 --- Audit

**Responsibility:** Preserve a traceable record of governance inputs,
authority derivation, portable authority, enforcement, execution,
observed effect, verification, and relevant administrative or incident
actions.

Proposed requirements: - Correlate the stages through stable identifiers
and provenance. - Record accepted and rejected operations, including
relevant failures. - Protect audit integrity and restrict access to
sensitive evidence. - Minimize unnecessary personal, tenant, secret, and
operational data. - Define retention, export, deletion, and
legal/organizational obligations. - Preserve evidence of denied
operations and inconclusive verification.

**Open decisions:** canonical audit schema, integrity/signing model,
retention, data minimization, storage ownership, audit access controls.

## 5. Core subsystem within ADIE

### 5.1 Logical placement

`<escape>`{=html} RedoudAm Intelligence Ecosystem \| v ADIE \| +--
Security Law Book +-- Dynamic Authority Derivation +-- Portable
Authority +-- Enforcement governance and contracts +-- Verified Effect
+-- Audit \| +-- Core subsystem \| - Authentication \| - Identity and
sessions \| - Tenant / branch foundations \| - Users, roles, permissions
\| - Licensing and configuration foundations \| +-- Other ADIE
subsystems \| +-- Participating independent platforms - NoredAm
Intelligence Platform - Other approved ecosystem platforms - Connectors,
executors, external environments `</escape>`{=html}

This is a logical relationship, not a repository, process, database, or
deployment layout.

### 5.2 Core ownership

Core is an ADIE subsystem, not a peer platform to ADIE. Core retains the
capabilities assigned to it, including identity, authentication,
sessions, users, roles, permissions, tenant/branch foundations, and
applicable Core V1 functions.

ADIE's security architecture consumes Core-owned facts and capabilities
through explicit contracts. It must avoid duplicate identity/session
ownership and must not treat Core's current functional baseline as the
complete implementation of all six security stages.

### 5.3 Required Core contracts

Before implementation, identify and verify: - Identity and session
assertion contracts and authoritative source. - Tenant and
organizational context contracts. - User/role/permission data or
decision contracts and their intended semantics. - Authentication,
session revocation, and lifecycle propagation behavior. -
Service-to-service identity and trust rules. - Contract versioning and
compatibility. - Failure behavior when Core is unavailable or returns
stale/incomplete information. - Tenant-isolation behavior across ADIE
and external boundaries.

The precise contracts must be verified from current implementation and
authoritative sources. No undocumented API or behavior is assumed.

## 6. Other subsystems and participating platforms under ADIE governance

### 6.1 Governance rule

Other ADIE subsystems and participating platforms operate under ADIE's
approved control, security verification, and authorization framework.
This does not transfer ownership of their domain functions to ADIE and
does not remove local enforcement obligations.

Each participating platform must have: 1. A named owner and declared
responsibility. 2. An explicit integration contract and
compatibility/version policy. 3. A trusted platform/service identity. 4.
A defined way to supply identity and context. 5. A defined
authority-decision validation and enforcement boundary. 6. A way to
report execution outcomes and effect evidence. 7. A correlated
audit/evidence path. 8. Explicit failure and revocation behavior.

Registration or onboarding alone does not confer permission to execute
operations.

### 6.2 NoredAm integration

NoredAm remains an independent engineering platform. It operates under
ADIE's approved security and authorization framework through explicit
contracts.

The architecture must not absorb NoredAm's engineering responsibilities
into ADIE, treat an engineering-specific authority evaluator as the
ecosystem authority service without approval, bypass NoredAm execution
enforcement, weaken its fail-closed unavailable-provider behavior, or
authorize repository/namespace/implementation identity migration.

Before implementation, verify the current repository state and identify
the exact contract NoredAm requires to consume ADIE-authorized
decisions.

### 6.3 Other platforms, connectors, and executors

ADIE governs authorization and security verification; the owning
execution boundary independently enforces the applicable decision; the
resulting effect is verified and audited. Integration protocols and
safety requirements vary by environment and must be specified before
each integration. No production capability is implied by this baseline.

## 7. Cross-stage contracts and traceability

The six stages must form one traceable security lifecycle without
assuming one service per stage.

  -----------------------------------------------------------------------
  Handoff                 Minimum obligation      Blocking question
  ----------------------- ----------------------- -----------------------
  Security Law Book →     Identify applicable,    How are conflicts and
  Dynamic Authority       approved, versioned     unavailable sources
  Derivation              policy inputs and       handled
                          provenance              deterministically?

  Dynamic Authority       Bind decision to actor, Who issues the decision
  Derivation → Portable   action,                 and which states exist?
  Authority               resource/capability,    
                          context, constraints,   
                          validity, correlation   

  Portable Authority →    Verify issuer,          What is the canonical
  Independent Enforcement integrity, scope,       artifact/validation
                          audience, validity,     contract?
                          revocation/staleness    

  Enforcement → Verified  Record executor,        How are ambiguous,
  Effect                  enforcement outcome,    partial, and
                          operation attempt,      asynchronous outcomes
                          intended effect         represented?

  Verified Effect → Audit Attach verification     What integrity and
                          result, evidence        retention rules apply?
                          provenance,             
                          correlation, timestamps 

  Audit → Governance      Make relevant evidence  Which records may
  feedback                available to approved   change policy, and who
                          governance processes    approves changes?
  -----------------------------------------------------------------------

No stage may manufacture evidence belonging to another stage. A valid
authority artifact does not prove that an operation succeeded; a
successful executor response does not by itself prove the intended
effect.

## 8. Trust, data, and tenant boundaries

### 8.1 Trust boundaries

The approved architecture must distinguish: - ADIE governance/policy
authority; - Core subsystem identity and tenant context; - ADIE
authority decision and portable authority responsibilities; - each
subsystem's internal trust domain; - each participating platform's
integration boundary; - each executor-local enforcement boundary; - the
digital or physical environment being acted upon; - effect-verification
source; - audit/evidence storage; - administrative and operational
access.

Trust must not be inferred solely from network location, registration,
tenant identifiers, successful authentication, or a valid token whose
authority and scope have not been verified.

### 8.2 Tenant and data ownership

Core remains the source of truth for its owned tenant, branch, identity,
and related data. ADIE defines the approved way those facts are consumed
by other subsystems and platforms.

Mandatory requirements: - Tenant isolation is enforced at each relevant
data and execution boundary. - Cross-tenant access requires explicit
authorization and auditability. - Each data element has a declared
authoritative owner. - Identity and business data are not copied
unnecessarily. - Revocation and relevant tenant/security changes
propagate according to explicit contracts. - Retention, deletion,
export, residency, and backup rules are defined before production data
is introduced.

No database topology or centralized/distributed evidence store is
selected here.

## 9. Component and deployment boundaries

### 9.1 Logical component model

This draft organizes ADIE around the six-stage security structure and
identifies Core as a subsystem. It does not yet approve the number or
deployment shape of components.

The next architecture decision must allocate each responsibility to: -
an ADIE-owned logical component; - Core subsystem; - participating
platform; - executor-local enforcement point; or - independent
verification/audit responsibility where required.

For each allocation, define ownership, inputs, outputs, trust boundary,
data ownership, failure behavior, and dependencies.

### 9.2 Deployment topology remains open

No choice is made among a modular monolith, multiple services,
containers, orchestration platforms, cloud providers, or self-hosted
topology. Deployment choices follow the approved logical component model
and explicit availability, isolation, scale, recovery, latency,
offline-behavior, and operational requirements.

**Deployment is not authorized by this draft.**

## 10. Failure and negative-path requirements

The approved architecture/specification must define deterministic
behavior when: - Identity, issuer, or service trust cannot be
established. - Tenant or organizational context is missing or
inconsistent. - Security Law Book sources are unavailable, conflicting,
or not current. - Authority derivation is denied, indeterminate, or
cannot complete. - Portable authority is missing, malformed, expired,
revoked, stale, replayed, out of scope, or unverifiable. - An execution
boundary rejects or cannot enforce the decision. - An operation times
out, partially completes, or returns an ambiguous response. - The
intended effect cannot be verified or contradicts observations. -
Audit/evidence persistence fails. - A duplicate or replayed request is
received.

Missing authority evidence must not become permission. Safety-critical
exceptions, compensation, retry, or degraded modes must be explicit,
bounded, and approved.

## 11. Architecture decision register

These are draft tracking IDs only, not canonical or approved ADR
identifiers until reconciled with the authoritative ADR library.

  -----------------------------------------------------------------------
  Draft ID                Decision                Status
  ----------------------- ----------------------- -----------------------
  ADIE-ADR-D01            Six-stage security      User-directed; pending
                          structure as ADIE's     formal approval
                          organizing              
                          responsibility model    

  ADIE-ADR-D02            Core as an ADIE         User-directed; pending
                          subsystem and its       formal approval
                          responsibility boundary 

  ADIE-ADR-D03            ADIE governance and     Proposed;
                          integration             component/contract
                          requirements for other  decisions needed
                          subsystems/platforms    

  ADIE-ADR-D04            Security Law Book       Open / blocking
                          representation and      
                          lifecycle               

  ADIE-ADR-D05            Dynamic authority       Open / blocking
                          derivation ownership    
                          and deterministic       
                          semantics               

  ADIE-ADR-D06            Portable authority      Open / blocking
                          contract and            
                          trust/validation rules  

  ADIE-ADR-D07            Independent enforcement Open / blocking
                          contract and            
                          conformance             
                          requirements            

  ADIE-ADR-D08            Effect verification and Open
                          evidence contract       

  ADIE-ADR-D09            Correlated              Open
                          audit/evidence contract 

  ADIE-ADR-D10            Logical component       Open / blocking
                          boundaries and          
                          deployment decision     
                          criteria                

  ADIE-ADR-D11            Core-to-ADIE subsystem  Open / blocking
                          contracts and           
                          compatibility           

  ADIE-ADR-D12            Cross-platform          Open / blocking
                          integration and         
                          lifecycle contracts     

  ADIE-ADR-D13            Tenant/context          Open / blocking
                          propagation and         
                          cross-tenant denial     
                          requirements            
  -----------------------------------------------------------------------

A3.3 is not recorded as an approved milestone. Milestone labels and
sequencing remain governed by the canonical roadmap and authorized
planning role.

## 12. Architecture approval criteria

The Chief Platform Architect should approve the baseline only when:

### 12.1 Source and governance reconciliation

-   Canonical Architecture Freeze and relevant ADRs are identified and
    reconciled.
-   The approved roadmap and current milestone state are confirmed.
-   Proposed decisions are distinguished from approved constraints.
-   No constitutional conflict is resolved by assumption.

### 12.2 Responsibility and boundary clarity

-   The six-stage model governs the responsibility structure.
-   Core is explicitly modeled as an ADIE subsystem.
-   Other platforms operate under ADIE governance without losing domain
    ownership.
-   ADIE responsibilities, Core responsibilities, platform
    responsibilities, and executor-local enforcement are
    distinguishable.
-   Logical components and contracts constrain the first implementation
    specification.

### 12.3 Security completeness

-   Dynamic authority decision ownership and semantics are explicit or
    remain documented as blocking.
-   Portable authority scope, provenance, validity, and validation
    requirements are explicit.
-   Independent enforcement is required at every applicable execution
    boundary.
-   Verification distinguishes intended effect from request acceptance
    and execution response.
-   Audit/evidence records are correlated across the lifecycle.
-   Negative-path behavior is deterministic and fail-closed where
    required.

### 12.4 Compatibility and implementation readiness

-   Core and NoredAm contracts are verified against current source state
    before implementation.
-   Existing NoredAm fail-closed behavior is preserved.
-   Tenant isolation and data ownership are explicit.
-   No implementation or deployment begins while blocking architecture
    decisions remain unresolved.

## 13. Proposed next sequence

This is a workflow recommendation, not an approved roadmap amendment:

1.  Reconcile the canonical Architecture Freeze, ADR library, and
    current approved roadmap.
2.  Finalize the ADIE/Core subsystem boundary and six-stage
    responsibility model.
3.  Approve the logical component and contract-boundary architecture.
4.  Register approved decisions in the canonical ADR library.
5.  Have the Chief Engineering Architect determine the next milestone
    under the current approved roadmap.
6.  Produce one atomic implementation specification after architecture
    approval.
7.  Implement, compile, verify, and approve through separate gates.

A3.3 is not presumed approved or required. Use the existing authorized
roadmap; do not create or reorder milestones by assumption.

## 14. Decision requested

**Requested action:** Chief Platform Architect review of Draft 0.2.0.

Possible outcomes: - **Approve:** blocking architectural and
source-reconciliation questions are resolved. - **Approve with explicit
conditions:** only if no unresolved safety-critical semantics or
platform ownership conflicts remain. - **Return for revision:** specific
decisions or source conflicts remain unresolved. - **Escalate:**
constitutional authority is required or the issue cannot be resolved
under the established hierarchy.

Until formal approval, this remains a proposed architecture baseline and
does not authorize production implementation or deployment.
