# RedoudAm Intelligence Ecosystem --- ADIE Architecture Baseline

**Document ID:** ADIE-ARCH-BASELINE-DRAFT-001\
**Version:** 0.1.0-draft\
**Status:** Proposed architecture baseline --- **NOT APPROVED**\
**Prepared:** 2026-10-19\
**Session role:** Ecosystem Governor\
**Architecture approval authority:** Chief Platform Architect\
**Scope:** Logical architecture baseline for ADIE, including ecosystem
platform boundaries and the architectural questions required before
deployment\
**Excluded:** Production implementation, deployment configuration, code
changes, technology selection, and changes to frozen roadmaps

------------------------------------------------------------------------

## 1. Purpose

The RedoudAm Intelligence Ecosystem has an approved ecosystem definition
and platform-boundary model, but the reviewed source set does not
establish a complete, approved technical architecture for ADIE. This
document starts the architecture work without pretending that a
definition or vision is already an implementation-ready architecture.

This is a **proposal for review by the Chief Platform Architect**. It is
not an Architecture Freeze, approved ADR, implementation order, or
authorization to deploy.

The objective is to define a coherent ADIE structure that can
interoperate with existing ecosystem platforms while preserving their
independent responsibilities and implementation continuity.

## 2. Status and evidence classification

Statements in this document use the following classifications:

-   **Established:** directly supported by an identified project source
    or previously recorded implementation finding.
-   **Proposed:** a design option introduced here for architecture
    review; not yet approved.
-   **Open:** a decision requiring evidence, an explicit decision owner,
    or further analysis.
-   **Excluded:** outside this baseline's scope.

### 2.1 Established baseline

1.  The RedoudAm Intelligence Ecosystem is defined as a federation of
    independent yet interoperable platforms. The ecosystem definition
    intentionally does not prescribe implementation methods, engineering
    practices, software architecture, or specific technologies.

2.  ADIE is defined as the common digital infrastructure platform for
    foundational capabilities used across the ecosystem. It is not a
    business application and does not replace specialized platforms.

3.  RedoudAm Core has a functional baseline covering identity,
    authentication, authorization, sessions, identity security,
    tenant/branch foundations, licensing, users, roles, and permissions.

4.  NoredAm Intelligence Platform (historically implemented as RedTap
    DevTools) is an independent engineering platform. Its existing
    repository and implementation identity must remain unchanged unless
    separately authorized.

5.  The frozen security/governance reference model is:

    **Security Law Book → Dynamic Authority Derivation → Portable
    Authority → Independent Enforcement at Every Execution Boundary →
    Verified Effect → Audit**

6.  Existing findings do not establish a complete generic
    authority-decision contract between Core and NoredAm. Existing Core
    identity/session/permission artifacts must not silently be treated
    as the Security Law Book or as the complete dynamic-authority
    decision source.

7.  The existing NoredAm unavailable security-disposition provider is
    fail-closed and must not be weakened or bypassed as part of this
    architecture exercise.

8.  Architecture approval belongs to the Chief Platform Architect. The
    Ecosystem Governor owns ecosystem vision, platform portfolio,
    platform ownership, and lifecycle; this session can contribute the
    ecosystem requirements but cannot approve the technical
    architecture.

### 2.2 Source basis

This draft is grounded in the following project sources and recorded
findings:

-   **RedTap Ecosystem.md** --- ecosystem definition and federation
    principle.
-   **RedTap Platform Portfolio.md** --- platform identities, ownership
    boundaries, and non-duplication principle.
-   **RedTap ADIE(1).md** --- ADIE product definition and foundational
    purpose.
-   **RedTap Core V1.md** --- Core V1 functional baseline.
-   **RedTap AI Operating Environment (AIOE) v1.md** --- role authority,
    architecture playbook, session rules, and approval gates.
-   **RedoudAm-NoredAm-Public-Naming-Authority.md** --- approved public
    names and protection of implementation identity.
-   **RedoudAm_Ecosystem_Current_Session_Reference_Summary.md** ---
    investigation hypotheses and frozen security/governance reference
    model; this is a session investigation record, not an architecture
    approval.
-   **A3.1 existing Core authority source and contract findings** ---
    targeted inspection finding that the required generic
    authority-decision contract was not established by the inspected
    sources. This is not proof that no such contract could exist
    elsewhere.

A complete Architecture Freeze, approved ADIE ADR set, and authoritative
ADIE roadmap were not established from the sources available for this
draft. Their existence and current status must be confirmed before
baseline approval.

------------------------------------------------------------------------

## 3. Architectural objective

**Proposed objective:** ADIE provides the shared infrastructure and
governance integration layer that allows independently owned ecosystem
platforms and capabilities to interoperate under explicit contracts,
while leaving each platform's internal responsibilities and execution
authority intact.

ADIE should not become a replacement for Core, a rebranded NoredAm, or
an all-purpose application that absorbs every ecosystem capability.

The design must support gradual deployment: establish a minimal ADIE
foundation first, integrate existing platforms through explicit
contracts, and add authority/evidence capabilities only after their
semantics and trust boundaries have been approved.

## 4. Platform responsibility boundaries

The following is a proposed logical responsibility model, not a service
or repository decomposition.

  -------------------------------------------------------------------------
  Platform / layer        Established or proposed   Boundary that must be
                          responsibility            preserved
  ----------------------- ------------------------- -----------------------
  RedoudAm Intelligence   Ecosystem-wide vision,    Does not prescribe an
  Ecosystem               constitutional rules,     unapproved
                          platform portfolio, and   implementation topology
                          federation principles     

  ADIE                    **Proposed:** shared      Must not absorb
                          cross-platform            platform-specific
                          infrastructure, platform  implementation or
                          integration contracts,    silently duplicate
                          ecosystem capability      platform capabilities
                          discovery, governance     
                          integration, and shared   
                          operational               
                          administration            

  RedoudAm Core           Existing identity,        Core identity and Core
                          tenant/branch, session,   API authorization
                          user/role/permission, and remain Core-owned
                          related Core V1           unless a formally
                          capabilities              approved architecture
                                                    changes that boundary

  NoredAm Intelligence    Engineering lifecycle,    Must remain an
  Platform                engineering               independent platform;
                          knowledge/intelligence,   no assumption that it
                          automation, repository    is the general
                          and engineering workflows ecosystem execution
                                                    runtime

  Other ecosystem         Their own domain          Participate through
  platforms and external  behavior, data, and       explicit contracts;
  capabilities            execution functions       integration does not
                                                    transfer ownership

  Digital or physical     Systems/devices/APIs      Enforcement remains
  execution environments  where an operation may    required at every
                          produce an effect         applicable execution
                                                    boundary; ADIE
                                                    integration alone is
                                                    not enforcement
  -------------------------------------------------------------------------

**Open decision:** whether ADIE is a single deployable platform, a set
of deployable components, or a logical platform initially hosted in a
limited number of deployables. This must be decided by the Chief
Platform Architect using operational, security, and lifecycle
requirements---not assumed from the logical domains below.

## 5. Proposed logical architecture

The following domains are **responsibility groupings only**. They do not
imply microservices, separate databases, separate repositories,
processes, or deployment units.

### 5.1 Platform Integration and Contract Registry

Responsibilities proposed for review:

-   Maintain discoverable descriptions of participating platforms and
    their published integration contracts.
-   Record contract identity, version, owner, compatibility
    expectations, and lifecycle status.
-   Support explicit onboarding and contract-version negotiation.
-   Avoid treating a platform's mere registration as proof that its
    capabilities are trusted or authorized.

**Open:** whether registry data is authoritative, replicated, or
consumed from platform-owned manifests; how contract withdrawal and
incompatible versions are handled.

### 5.2 Identity and Trust Integration

Responsibilities proposed for review:

-   Define how ADIE recognizes identities and trust assertions issued by
    Core or other approved identity authorities.
-   Preserve the identity issuer and provenance of assertions.
-   Define trust establishment, key/credential lifecycle, expiry,
    revocation, and failure behavior.
-   Avoid reimplementing Core identity/session behavior by default.

**Open:** the precise identity federation protocol, issuer model, token
exchange rules, and whether ADIE itself requires a distinct service
identity. No protocol is selected in this draft.

### 5.3 Policy and Governance Knowledge

Responsibilities proposed for review:

-   Represent approved governance sources and their provenance/version.
-   Provide a controlled way to identify which policy and governance
    artifacts apply to a request.
-   Preserve the distinction between source policy, interpretation,
    derived decisions, and evidence of enforcement.

**Open:** the machine-readable Security Law Book format, policy conflict
semantics, deterministic interpretation rules, version pinning, and
amendment lifecycle. The current investigation source identifies these
as unresolved; this draft does not invent their semantics.

### 5.4 Authority Decision and Portable Authority

Responsibilities proposed for review:

-   Establish a formally specified decision-producing responsibility for
    dynamic authority derivation, if approved by architecture.
-   Bind any decision or portable authority artifact to its subject,
    intended action, resource/capability, context, policy provenance,
    execution correlation, validity window, and applicable constraints.
-   Define how decisions are authenticated, validated, expired, revoked,
    and rejected when stale, incomplete, or unverifiable.
-   Make outcomes explicit, including at least allow/deny and any
    separately approved escalation or indeterminate states.

**Critical open decision:** the authoritative producer and verifier of
dynamic authority decisions, including whether these responsibilities
belong within ADIE or another formally designated platform boundary. No
authority service is authorized by this draft. No existing Core
JWT/session/permission claim or NoredAm engineering authorization
contract may be presumed to satisfy this requirement.

### 5.5 Execution Governance and Enforcement Integration

Responsibilities proposed for review:

-   Define the contract by which an execution boundary obtains and
    validates the applicable authorization/authority decision.
-   Require each executor or execution boundary to enforce the decision
    independently.
-   Specify fail-closed behavior when required authority evidence is
    absent, invalid, expired, revoked, or unverifiable, except where a
    separately approved safety policy defines a different explicit
    behavior.
-   Preserve executor ownership of the final enforcement point.

ADIE orchestration must not be mistaken for independent enforcement. A
central decision does not remove the need for local enforcement at each
execution boundary.

**Open:** the minimum enforcement contract shared by API, software,
infrastructure, and physical-device executors; the treatment of
emergency/safety operations; and the relationship between ADIE
governance and executor-local policies.

### 5.6 Effect Verification and Evidence

Responsibilities proposed for review:

-   Define evidence contracts for intended operation, observed outcome,
    verification method, time, execution correlation, and provenance.
-   Distinguish a request being accepted, an operation being attempted,
    and an effect being verified.
-   Correlate authority decision, execution attempt, observed effect,
    verification result, and audit record.
-   Represent verification failure or inconclusive evidence explicitly.

**Open:** evidence integrity requirements, independent-verifier
criteria, evidence retention, replay resistance, and whether evidence is
stored centrally or by the owning platform.

### 5.7 Audit and Governance Records

Responsibilities proposed for review:

-   Preserve a traceable record of decision provenance, enforcement
    result, execution, verification, and subsequent governance actions.
-   Support integrity checks and controlled access to audit evidence.
-   Record failures and rejected operations as well as successful
    operations.

**Open:** canonical audit-record schema, signing/integrity model,
retention/deletion policy, privacy minimization, and audit-storage
ownership.

### 5.8 Platform Administration and Shared Infrastructure

Responsibilities proposed for review:

-   Provide shared administration for ADIE-owned configuration, platform
    onboarding, lifecycle status, and operational health.
-   Establish the operational boundaries for configuration, secrets,
    observability, backup/recovery, upgrades, and service-to-service
    communication.
-   Reuse approved ecosystem capabilities where contracts and ownership
    are clear.

**Open:** which existing Core capabilities can be reused directly, what
ADIE-specific administration is required, and what shared infrastructure
is already available. Do not create duplicate tenant, identity,
licensing, or administration implementations by assumption.

------------------------------------------------------------------------

## 6. Proposed interaction model

The following flow is conceptual and intentionally omits protocols and
deployment topology:

``` text
Human / Platform / AI-originated request
                  |
                  v
       Identify actor and provenance
                  |
                  v
       Resolve target capability/contract
                  |
                  v
       Load applicable governance context
                  |
                  v
       Derive or obtain authority decision
                  |
                  v
       Validate decision at execution boundary
                  |
                  v
       Execute within the owning platform
                  |
                  v
       Observe and verify resulting effect
                  |
                  v
       Correlate evidence and audit record
```

The flow is a proposed interpretation of the frozen security/governance
reference model. It does not yet define a complete request protocol,
state machine, API, message format, or implementation.

The sequence must not imply that every step is a separate service. Nor
does a single diagram establish that one component can safely perform
all steps.

### 6.1 Mandatory negative paths

The later approved architecture must define deterministic behavior for
at least these cases:

-   Actor or issuer cannot be authenticated.
-   Capability or contract is unknown, disabled, or incompatible.
-   Applicable governance sources cannot be resolved.
-   Authority decision is denied, missing, stale, expired, revoked,
    malformed, or unverifiable.
-   Executor rejects the decision or cannot enforce it.
-   Operation times out or returns an ambiguous result.
-   Effect cannot be verified, or observed state contradicts the
    intended effect.
-   Audit/evidence persistence fails.
-   Duplicate or replayed execution request is received.

**Proposed safety posture:** missing required authority evidence must
not be interpreted as permission. Exact recovery, retry, compensation,
and safety exceptions remain open until specified.

------------------------------------------------------------------------

## 7. Core V1 and NoredAm integration boundaries

### 7.1 RedoudAm Core

**Established:** Core V1 has identity, session, tenant/branch,
user/role/permission, and related security foundations.

**Proposed integration rule:** ADIE consumes Core capabilities only
through explicit, versioned, authenticated contracts. Core remains the
owner of its identity and Core API authorization behavior.

**Not established:** a generic cross-platform authority decision
contract that implements the full Security Law Book → Dynamic Authority
Derivation → Portable Authority flow.

**Required before integration approval:**

1.  Inventory the actual Core public contracts and their owners.
2.  Distinguish identity assertions, session state, Core permission
    decisions, policy evaluation, and portable authority artifacts.
3.  Specify issuer, subject, tenant/resource scope, audience,
    expiry/revocation, correlation, provenance, and failure semantics
    where relevant.
4.  Define compatibility and contract-version policy.
5.  Verify tenant isolation and cross-tenant denial behavior.

No assumption is made that Core must be rewritten or that it should
become the owner of all ADIE governance logic.

### 7.2 NoredAm Intelligence Platform

**Established:** NoredAm is an independent engineering platform; current
public naming does not authorize implementation identity migration.
Existing NoredAm execution enforcement has a fail-closed
unavailable-provider behavior in the inspected implementation.

**Proposed integration rule:** ADIE integration must use a separately
approved contract and preserve NoredAm's internal ownership. It must not
weaken the fail-closed provider, bypass execution enforcement, or
repurpose an existing engineering-specific authority evaluator as an
ecosystem authority service without approved architecture evidence.

**Required before integration approval:**

1.  Verify current NoredAm extension/execution contracts and provider
    behavior.
2.  Identify the exact contract required from an external authority
    decision producer.
3.  Specify the decision binding, trust/provenance, expiry/revocation,
    correlation, and failure semantics.
4.  Demonstrate enforcement at the NoredAm execution boundary.
5.  Demonstrate the relationship between execution result, verified
    effect, and correlated audit evidence.

The previously recorded A3.1 findings are targeted inspection results,
not exhaustive proof that no additional contract exists in any source or
branch. The current source/branch/commit must be verified before
implementation.

### 7.3 Other platforms and executors

No other platform, provider, connector, or physical executor is assumed
to be integrated. Each requires a named owner, published contract, trust
model, enforcement boundary, effect-verification strategy, and
compatibility policy.

------------------------------------------------------------------------

## 8. Trust boundaries

The Chief Platform Architect should approve an explicit trust-boundary
diagram. At minimum, the architecture must distinguish:

1.  Ecosystem governance and policy sources.
2.  ADIE-owned control-plane responsibilities.
3.  Core identity and tenant boundary.
4.  Each participating platform's internal trust domain.
5.  Platform-to-platform contract boundary.
6.  Authority-decision issuer and verifier.
7.  Executor-local enforcement boundary.
8.  Digital or physical environment being controlled.
9.  Independent effect-verification source.
10. Evidence and audit storage boundary.
11. Administrative and operational access boundary.

No trust should be inferred solely from network location, registration,
a shared tenant identifier, a valid identity token, or successful
orchestration.

**Open:** trust bootstrap, cryptographic identity, key management,
service-to-service authentication, credential rotation, revocation
propagation, and compromise containment.

------------------------------------------------------------------------

## 9. Data and tenancy boundaries

The architecture must define data ownership before selecting databases
or storage topology.

  -----------------------------------------------------------------------
  Data category           Proposed ownership      Unresolved decisions
                          principle               
  ----------------------- ----------------------- -----------------------
  Core identity,          Remain Core-owned       ADIE read/query
  sessions, tenants,      unless an approved      contract; consistency
  branches, users, roles, architecture decision   and revocation
  permissions             states otherwise        propagation

  Platform and contract   Owned by the platform   Registry ownership,
  metadata                designated as registry  availability,
                          authority               versioning, and
                                                  replication

  Governance/policy       Owned by their          Canonical source,
  artifacts               designated governance   version pinning,
                          source                  approval, amendment and
                                                  withdrawal

  Authority               Owned by the formally   Issuer, artifact
  decisions/portable      designated decision     format, storage,
  artifacts               issuer, with validation expiry, revocation, and
                          at each execution       portability
                          boundary                

  Execution records and   Evidence producer       Storage owner,
  effect evidence         remains identifiable;   integrity, retention,
                          canonical correlation   privacy and
                          must be preserved       verification
                                                  independence

  Audit records           Governed by an approved Retention,
                          audit/evidence policy   immutability, access
                                                  control, legal deletion
                                                  and export

  ADIE operational        ADIE-owned              Configuration
  configuration           configuration remains   authority, secret
                          distinct from           management and change
                          platform-owned          approval
                          configuration           
  -----------------------------------------------------------------------

Mandatory constraints:

-   Tenant isolation must be enforced at each relevant data-access and
    execution boundary.
-   Cross-tenant operations must be explicitly authorized and auditable.
-   No central data store should silently become the authoritative owner
    of every platform's data.
-   Minimize copied identity and business data; define the authoritative
    source for each field.
-   Define retention, deletion, export, residency, and backup/recovery
    rules before production data is introduced.

These are proposed architecture requirements; the detailed controls and
implementation mechanisms remain open.

------------------------------------------------------------------------

## 10. Deployment and operational architecture

This baseline intentionally does **not** select microservices, a modular
monolith, containers, Kubernetes, a specific message broker, a database,
cloud provider, or on-premises topology.

Before a deployment topology can be approved, the architecture must
resolve:

-   Required deployment modes: self-hosted, private infrastructure,
    public cloud, hybrid, or other approved modes.
-   Availability and recovery objectives.
-   Expected scale, latency, throughput, and geographic distribution.
-   Network trust zones and ingress/egress policy.
-   Service identity, secret storage, key rotation, and certificate
    lifecycle.
-   Configuration ownership and change control.
-   Observability: logs, metrics, traces, correlation identifiers, and
    sensitive-data redaction.
-   Backup, restore, disaster recovery, and upgrade compatibility.
-   Isolation requirements for tenants and high-risk executors.
-   Offline/partition behavior and revocation propagation.
-   Operational ownership and incident response.
-   Integrity and availability requirements for authority, verification,
    and audit evidence.

**Deployment gate:** no production deployment design should be finalized
until the logical ownership, trust boundaries, authority semantics, and
minimum integration contracts are approved.

------------------------------------------------------------------------

## 11. Architecture decisions register

The following register deliberately records open decisions rather than
disguising them as settled design.

  ---------------------------------------------------------------------------
  ID                Decision required     Proposed owner    Current status
  ----------------- --------------------- ----------------- -----------------
  ADIE-ADR-001      ADIE logical          Chief Platform    Open
                    component and         Architect         
                    deployable-boundary                     
                    model                                   

  ADIE-ADR-002      ADIE/Core identity    Chief Platform    Open
                    and trust integration Architect         
                    contract                                

  ADIE-ADR-003      Authoritative owner   Chief Platform    Open / blocking
                    and semantics for     Architect;        
                    dynamic authority     escalate if       
                    decisions             constitutional    
                                          authority is      
                                          implicated        

  ADIE-ADR-004      Portable authority    Chief Platform    Open / blocking
                    artifact and          Architect         
                    validation contract                     

  ADIE-ADR-005      Independent           Chief Platform    Open / blocking
                    enforcement contract  Architect         
                    at execution                            
                    boundaries                              

  ADIE-ADR-006      Effect verification   Chief Platform    Open
                    and evidence contract Architect         

  ADIE-ADR-007      Correlation,          Chief Platform    Open
                    integrity, and        Architect         
                    audit-record contract                   

  ADIE-ADR-008      Platform/contract     Chief Platform    Open
                    registry ownership    Architect         
                    and lifecycle                           

  ADIE-ADR-009      Tenant, data          Chief Platform    Open
                    ownership, and        Architect         
                    cross-platform data                     
                    boundaries                              

  ADIE-ADR-010      Deployment modes and  Chief Platform    Open
                    operational           Architect         
                    requirements                            
  ---------------------------------------------------------------------------

These identifiers are **draft tracking identifiers only**, not assigned
or approved ADR numbers. They must be reconciled with the canonical ADR
library before formal registration.

------------------------------------------------------------------------

## 12. Verification criteria for the architecture baseline

The Chief Platform Architect should not approve the baseline until the
following are demonstrably satisfied:

### 12.1 Constitutional and portfolio compliance

-   ADIE's purpose is consistent with the approved product definition.
-   The independent-platform/federation model is preserved.
-   Core and NoredAm responsibilities are neither silently transferred
    nor duplicated.
-   Public naming is correct and implementation identities are
    unchanged.
-   No frozen roadmap or approved implementation order is modified by
    implication.

### 12.2 Architecture completeness

-   Logical responsibilities and ownership are explicit.
-   Logical domains are clearly distinguished from deployable
    components.
-   Cross-platform dependencies and contract owners are identified.
-   Trust boundaries and threat-relevant boundaries are documented.
-   Data and tenant ownership are explicit.
-   Failure and negative paths are deterministic.
-   Deployment decisions are supported by explicit operational
    requirements rather than assumption.

### 12.3 Security/governance completeness

-   Security Law Book, dynamic authority derivation, portable authority,
    independent enforcement, verified effect, and audit are all
    accounted for.
-   The authoritative decision producer is explicitly assigned or
    remains a documented blocking decision.
-   Identity claims are not conflated with policy-derived authority
    decisions.
-   Enforcement is required at each execution boundary.
-   Missing or invalid authority evidence does not become implicit
    permission.
-   Decision, execution, effect, and audit evidence can be correlated.
-   Verification and audit failures have explicit outcomes.

### 12.4 Compatibility and implementation readiness

-   Existing Core and NoredAm contracts are inventoried from verified
    current source state.
-   Existing fail-closed behavior is preserved.
-   Versioning and backward-compatibility rules are defined.
-   No implementation begins while blocking architecture decisions
    remain unresolved.
-   The architecture has an explicit path to a first atomic
    specification without prematurely selecting implementation details.

------------------------------------------------------------------------

## 13. Proposed next sequence --- not an approved roadmap

The following is a suggested sequence for review, not a modification to
any frozen roadmap:

1.  **Complete architecture source reconciliation.** Locate and verify
    the canonical Architecture Freeze, ADR library, current roadmap, and
    current platform states.
2.  **Chief Platform Architect review.** Resolve or explicitly retain
    the decisions in Section 11; approve, reject, or return this draft.
3.  **Publish the approved architecture baseline and ADRs.** Register
    decisions using canonical identifiers and versioning.
4.  **Chief Engineering Architect planning gate.** Reconcile the
    approved architecture with the existing roadmap and define the next
    atomic milestone.
5.  **Produce one implementation specification.** Define scope,
    interfaces, acceptance criteria, security tests, compatibility
    requirements, and rollback/recovery behavior.
6.  **Implement only after approval.** Compilation, verification, and
    approval remain separate gates.

No implementation or deployment is authorized by this document.

------------------------------------------------------------------------

## 14. Decision request

The required next decision is **Chief Platform Architect review of this
proposed ADIE Architecture Baseline**.

The reviewer should return one of:

-   **Approve:** architecture is sufficiently defined for baseline
    publication, with explicit approved decisions and ADRs.
-   **Approve with recorded conditions:** only if conditions are
    precise, owned, and do not leave safety-critical semantics
    ambiguous.
-   **Return for revision:** identify specific conflicts, missing
    decisions, or unsupported assumptions.
-   **Escalate:** if a decision requires constitutional authority or
    conflicts cannot be resolved under the established hierarchy.

Until that review is complete, this document remains **Draft 0.1.0 ---
not approved for implementation**.
