# ADIE Security Law Book --- Final Research Proposal

**Investigation ID:** `ADIE-SEC-INV-001-P1`\
**Path:** 1 --- Security Law Book\
**Date:** 22 October 2026\
**Status:** Final research proposal submitted for architectural review\
**Approval status:** Proposed; not yet architecturally approved\
**Implementation status:** Not authorized by this report

## 1. Executive determination

The research supports a clear direction for ADIE: **establish an
authoritative, versioned Security Law Book whose meaning is independent
of the policy engine used to evaluate its rules.**

ADIE should not treat a collection of Rego files, Cedar policies, XACML
documents, or application-level permissions as its complete Security Law
Book. These technologies solve important parts of the problem, but none
independently establishes the complete authority hierarchy, governance
legitimacy, and semantic guarantees required by ADIE.

The proposed solution combines:

1.  **An ADIE-governed canonical rule model** defining the meaning,
    authority, applicability, and lifecycle of security rules.
2.  **An explicit authority-precedence model** defining which authority
    may govern a situation and how constraints interact.
3.  **A deterministic evaluation contract** defining how applicable
    rules produce an outcome.
4.  **A policy-engine adapter** that translates approved rules into an
    executable representation and returns results through the ADIE
    contract.
5.  **An approval and publication process** that binds each active
    policy package to an authorized, immutable version.
6.  **Semantic conformance verification** proving that the executable
    representation preserves the approved rule meaning.
7.  **Historical decision reconstruction** linking a decision to the
    exact rules, versions, inputs, and evaluator profile that produced
    it.

The recommendation is to **adopt the canonical ADIE rule model as the
architectural foundation, use existing standards and policy engines as
implementation candidates, and keep the final engine selection subject
to conformance testing.**

This is not a proposal to invent a new general-purpose policy language.
ADIE should reuse mature technology wherever it meets the requirements
and develop only the governance and semantic contracts that existing
technology does not supply.

## 2. Research objective and scope

The central question is:

> How can ADIE maintain an authoritative, machine-interpretable body of
> security rules whose meaning remains controlled by governance rather
> than by the software that evaluates those rules?

The Security Law Book is the first stage of the established ADIE
security model:

**Security Law Book → Dynamic Authority Derivation → Portable Authority
→ Independent Enforcement at Every Execution Boundary → Verified Effect
→ Audit**

This investigation addresses the first stage. It does not define the
complete authority-derivation service, portable-authority format,
enforcement topology, or audit architecture.

Nevertheless, the Security Law Book must provide sufficient contracts
for those later stages to consume its rules without redefining their
meaning.

### 2.1 Required properties

  -----------------------------------------------------------------------
  Property                            Required guarantee
  ----------------------------------- -----------------------------------
  Authority                           Every active rule has an
                                      identifiable, authorized source.

  Governance                          Creation, approval, amendment,
                                      suspension, and withdrawal follow
                                      controlled procedures.

  Applicability                       The conditions under which a rule
                                      applies are explicit.

  Precedence                          Authority conflicts are resolved by
                                      defined rules, not accidental file
                                      ordering.

  Determinism                         Equivalent inputs and identical
                                      approved policy versions produce
                                      equivalent decisions.

  Failure safety                      Missing, invalid, stale, or
                                      unverifiable policy information
                                      cannot silently become permission.

  Integrity                           Published rules and policy packages
                                      can be verified against their
                                      approved versions.

  Traceability                        Each decision can identify the
                                      rules and policy versions relevant
                                      to its result.

  Semantic preservation               Compilation or translation cannot
                                      silently change an approved rule's
                                      meaning.

  Compatibility                       Rule-model evolution is controlled,
                                      versioned, and tested against
                                      existing consumers.

  Interoperability                    Independent ADIE subsystems and
                                      participating platforms can consume
                                      explicit, stable contracts.

  Reproducibility                     Historical decisions can be
                                      reconstructed to the extent
                                      permitted by retained evidence.
  -----------------------------------------------------------------------

These are proposed architectural requirements, not claims that an
implementation has already satisfied them.

# 3. Technology comparison

The research compared Open Policy Agent (OPA) with Rego, Cedar, and
XACML against a common set of authorization and governance requirements.

The comparison distinguishes two questions:

-   **Can the technology evaluate policies?**
-   **Can ADIE rely on the technology to preserve the meaning and
    authority of its Security Law Book?**

A positive answer to the first question does not establish the second.

## 3.1 Open Policy Agent and Rego

Official references: [OPA Policy
Language](https://www.openpolicyagent.org/docs/policy-language), [OPA
Policy Testing](https://www.openpolicyagent.org/docs/policy-testing),
and [OPA Decision
Logs](https://www.openpolicyagent.org/docs/management-decision-logs).

OPA is a general-purpose policy engine. Rego expresses declarative rules
over structured data and is designed to evaluate policies separately
from application business logic.

OPA provides useful capabilities for ADIE:

-   Declarative policy evaluation over structured inputs.
-   Policy testing.
-   Policy bundles and revision metadata.
-   Decision logs containing decision identifiers and bundle revision
    information.
-   Digital-signature verification for appropriately configured policy
    bundles.

Its signed-bundle mechanism can help establish the integrity and
authenticity of executable policy packages. It does not, by itself,
prove that a policy was legitimately approved under ADIE governance.

### Critical limitation: undefined and error outcomes

Rego's undefined values and its handling of certain runtime errors
require particular attention. OPA documentation explains that built-in
errors normally result in undefined values rather than necessarily
terminating evaluation.

That behavior is useful in general policy evaluation, but ADIE must not
allow an undefined or erroneous expression to be interpreted as implicit
authorization.

A policy that evaluates to false, a policy that does not apply, a
missing required attribute, and an evaluation failure are not
necessarily equivalent states.

**Proposed disposition: retain OPA/Rego as the leading initial engine
candidate, subject to a strict ADIE decision contract and successful
conformance tests.**

OPA is not itself the Security Law Book. ADIE must define the semantics
that its OPA integration is required to preserve.

## 3.2 Cedar

Official references: [Cedar Policy Language
Reference](https://docs.cedarpolicy.com/), [Cedar Authorization
Semantics](https://docs.cedarpolicy.com/auth/authorization.html), and
[Cedar Policy
Validation](https://docs.cedarpolicy.com/policies/validation.html).

Cedar is purpose-built for authorization. Its policies express permit
and forbid rules against a principal, action, resource, and context. Its
schema-based validation can identify many policy errors, including
invalid entity types, unsupported actions, and incorrect attribute
references.

Cedar's documented authorization semantics include:

-   Default denial when no permit policy applies.
-   Forbid rules overriding matching permit rules.
-   Diagnostics identifying determining policies and policy evaluation
    errors.

However, Cedar's documented evaluator skips policies that encounter
errors when combining authorization results. An application must examine
the returned diagnostics if it needs a stricter response to such errors.

This distinction matters to ADIE. If an applicable mandatory constraint
cannot be evaluated, ADIE must not assume that the constraint is
irrelevant simply because the engine excludes the erroneous policy from
the final result.

### Proposed disposition

**Retain Cedar as a serious alternative**, especially for its typed
authorization model and schema validation.

Before selecting it, ADIE must verify that its handling of evaluation
errors, missing information, policy combinations, and governance
constraints can meet the proposed ADIE contract without unsafe
assumptions or an overly complex adaptation layer.

## 3.3 XACML 3.0

Official reference: [OASIS XACML 3.0 Core
Specification](https://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-cos01-en.html).

XACML provides a mature authorization policy model with explicit
policy-combining and rule-combining algorithms.

Its standard concepts include:

-   Permit and deny outcomes.
-   Not-applicable results.
-   Indeterminate results.
-   Defined policy-combination procedures.
-   Obligations and advice associated with applicable decisions.

The specification's treatment of missing attributes, unsupported
functionality, and evaluation errors offers useful material for ADIE's
failure semantics. It also makes clear that combining algorithms can
have materially different behavior.

For example, deny-overrides, permit-overrides, and first-applicable are
not interchangeable. Choosing one determines how a group of policy
results is combined.

However, an XACML combining algorithm is still an evaluation mechanism.
It does not establish whether the policy authors and approving
authorities have legitimate authority over the rules they publish.

XACML's expressiveness also comes with a more elaborate policy
representation and integration model than may be desirable for every
ADIE use case.

### Proposed disposition

**Use XACML 3.0 as a principal reference for explicit evaluation
outcomes and policy-combination semantics.**

Retain it as an executable-engine candidate if the common conformance
tests demonstrate a material advantage. Do not assume that ADIE must
adopt XACML as its canonical rule representation.

## 3.4 NIST ABAC

Reference: [NIST SP 800-162 --- Guide to Attribute Based Access
Control](https://csrc.nist.gov/pubs/sp/800/162/upd2/final).

NIST describes attribute-based access control as authorization based on
attributes associated with the subject, object, requested operation, and
relevant environmental conditions, evaluated against policies, rules, or
relationships.

This is a strong conceptual foundation for describing the conditions
under which an action is permitted.

It does not, on its own, define ADIE's constitutional authority
hierarchy, governance lifecycle, cross-authority conflict semantics, or
rule-publication approval process.

### Proposed disposition

**Adopt ABAC as a conceptual foundation for expressing applicability and
authorization conditions**, without treating it as a complete Security
Law Book architecture.

## 3.5 Comparison summary

  --------------------------------------------------------------------------------------------------------------
  Requirement      OPA/Rego          Cedar                               XACML 3.0            Proposed ADIE
                                                                                              responsibility
  ---------------- ----------------- ----------------------------------- -------------------- ------------------
  Express policy   Strong            Strong                              Strong               Define the
  rules                                                                                       canonical meaning
                                                                                              of rules

  Structured       Supported         Explicit                            Supported through    Define required
  authorization                      principal/action/resource/context   request context and  context and its
  context                            model                               attributes           provenance

  Schema or        Available through Strong schema-based policy          Defined              Define the
  structural       policy/data       validation                          policy/request model authoritative
  validation       tooling                                               and validation       validation
                                                                         requirements         contract

  Explicit policy  Implementable in  Built-in permit/forbid combination  Multiple             Define the
  combination      Rego; depends on  semantics                           standardized         permitted
                   policy design                                         algorithms           combination
                                                                                              semantics for each
                                                                                              rule class

  Error and        Requires explicit Error diagnostics exist; erroneous  Explicit             Define mandatory
  indeterminate    ADIE handling of  policies are skipped by default     Indeterminate and    failure outcomes
  handling         undefined/error                                       NotApplicable        
                   behavior                                              outcomes             

  Policy-package   Signed bundles    Depends on publication and          Depends on           Require verifiable
  integrity        supported when    distribution arrangement            implementation and   approved-package
                   configured                                            distribution         integrity
                                                                         arrangement          

  Decision         Decision logs and Determining-policy and error        Response context and Define minimum
  traceability     bundle revision   diagnostics                         obligations/advice   evidence and
                   metadata                                                                   correlation
                                                                                              requirements

  Authority        Not supplied by   Not supplied by the engine          Not supplied by the  Govern centrally
  legitimacy       the engine                                            standard             through ADIE's
                                                                                              approved authority
                                                                                              model

  Approval and     Requires external Requires external governance        Requires external    Define lifecycle
  amendment        governance                                            governance           and authorized
  lifecycle                                                                                   transitions

  Semantic         Must be proven    Must be proven                      Must be proven       Require
  preservation                                                                                conformance
  across                                                                                      evidence
  translation                                                                                 
  --------------------------------------------------------------------------------------------------------------

**Comparison conclusion:** None of the three technologies should be
designated as the complete Security Law Book. The strongest approach is
to separate ADIE's authoritative rule semantics from the evaluator and
select an evaluator through conformance testing.

# 4. The most important distinction: authority precedence versus evaluation precedence

This is the central architectural finding of Path 1.

## 4.1 Authority precedence

Authority precedence answers:

> Which authority is entitled to impose a rule or constraint in this
> situation, and how must that constraint relate to other applicable
> authorities?

Examples include:

-   Applicable law or regulation.
-   Ecosystem-level security requirements.
-   ADIE security and governance rules.
-   Tenant or organizational policy.
-   Resource-specific constraints.
-   Delegated authority and personal authorization conditions.

These categories are illustrative. Their exact hierarchy, legal effect,
applicability, and interaction must be established through the
appropriate governance and legal analysis. Their presence in a list does
not automatically establish a universal ranking.

ADIE must not assume that every category can be ordered by one simple
numeric priority.

A jurisdictional requirement, a tenant policy, and a safety constraint
can interact in ways that are not correctly represented by merely
choosing whichever rule has the highest integer.

## 4.2 Evaluation precedence

Evaluation precedence answers:

> Given a defined set of applicable rules and their established
> relationships, how should an evaluator combine their results?

This is the domain addressed by mechanisms such as XACML combining
algorithms and Cedar's permit/forbid semantics.

An evaluation engine can correctly execute an algorithm and still
produce an unacceptable result if ADIE has supplied the wrong authority
relationships or failed to identify a mandatory constraint.

## 4.3 Proposed architectural rule

**ADIE must resolve authority and applicability before relying on a
policy engine to combine rule outcomes.**

The evaluator may implement approved semantics, but its execution order
must not become an independent source of authority.

The canonical rule model must therefore represent, or unambiguously
reference:

-   The authority source.
-   The rule's governing scope.
-   Its applicable conditions.
-   Its effect and constraints.
-   The approved relationship to other applicable rules.
-   The combination semantics appropriate to its rule class.
-   The rule's authorized version and lifecycle state.

This is a proposed architectural requirement, not an approved final
schema.

# 5. Final proposed design for the Security Law Book

## 5.1 Architectural structure

The proposed logical structure is:

**A. Rule Authority and Governance**\
Establishes who may create, approve, amend, suspend, or withdraw a rule.

**B. Canonical Rule Model**\
Defines the machine-interpretable representation and semantics of rules
independently of any one engine.

**C. Applicability and Authority Relationships**\
Defines when rules apply and how the applicable authority relationships
are determined.

**D. Conflict and Combination Semantics**\
Defines how compatible constraints combine, how explicit prohibitions
operate, and what happens when rules conflict or cannot be evaluated.

**E. Rule Lifecycle and Provenance**\
Maintains rule identity, approval history, version, effective status,
and authorized transitions.

**F. Policy Compilation and Publication**\
Transforms approved rules into an executable package where required,
while preserving the approved meaning and binding the package to its
source version.

**G. Conformance and Integrity Verification**\
Verifies policy packages and tests whether the executable result matches
the canonical rule contract.

**H. Decision-Reconstruction Metadata**\
Identifies the rule versions and evaluator information required to
reconstruct the basis of a decision.

These are logical responsibilities. They do **not** imply eight
services, eight subsystems, or eight deployment units.

The allocation of responsibilities to ADIE components remains subject to
architecture approval.

## 5.2 Canonical rule representation

The proposed canonical model should be engine-independent.

At minimum, each rule should have the following information, either
directly or through authoritative references:

  -----------------------------------------------------------------------
  Field or property                   Purpose
  ----------------------------------- -----------------------------------
  Rule identifier                     Stable identity across versions and
                                      publication events

  Rule version                        Identifies the exact approved
                                      version

  Authority reference                 Identifies the source authorized to
                                      govern the rule

  Ownership                           Identifies the responsible policy
                                      owner

  Approval provenance                 Identifies the authorization under
                                      which the rule was approved

  Rule class                          Distinguishes categories with
                                      potentially different semantics

  Applicability                       Defines the conditions under which
                                      the rule applies

  Subject and target scope            Identifies the actors, resources,
                                      operations, or capabilities
                                      affected

  Effect or constraint                Expresses the rule's normative
                                      meaning

  Authority relationships             Identifies applicable precedence or
                                      constraint relationships

  Combination semantics               Defines how the rule interacts with
                                      other applicable rules

  Effective lifecycle state           Establishes whether the version is
                                      approved, active, suspended, or
                                      withdrawn

  Validity interval                   Defines when the version is
                                      intended to apply

  Validation evidence                 Records the results required by the
                                      approved validation process

  Source integrity reference          Connects the representation to the
                                      authoritative source content

  Executable-package reference        Links the approved source version
                                      to its executable representation,
                                      if compiled
  -----------------------------------------------------------------------

This is a **minimum design proposal**, not a finalized schema. Field
types, required/optional status, identity conventions, and compatibility
rules must be specified before implementation.

### Why the canonical model must be independent of the engine

If the authoritative rules exist only as OPA/Rego, Cedar, or XACML
artifacts, the chosen engine's semantics can gradually become ADIE's de
facto governance semantics.

That creates risks when:

-   The engine is replaced.
-   A policy is translated to another language.
-   A runtime update changes behavior.
-   A platform implements only part of the policy contract.
-   A rule must be reviewed independently of its executable
    implementation.

An engine-independent canonical model allows ADIE to govern the rule
meaning first and then select an appropriate execution mechanism.

It also creates an additional obligation: ADIE must prove that
translation preserves meaning. Merely having two representations of the
same rule does not prove equivalence.

## 5.3 Authoritative rules and executable policy packages

The proposal is to separate the **authoritative rule source** from the
**executable policy package**.

The authoritative source establishes the approved rule meaning,
provenance, lifecycle state, and applicable governance constraints.

The executable package provides an implementation of those semantics for
a selected evaluator.

The relationship should be explicit:

`Approved Canonical Rules → Validated Transformation → Executable Policy Package → Integrity and Conformance Verification → Controlled Publication`

Each package should identify the exact canonical rule versions from
which it was generated.

The transformation must be deterministic for a pinned toolchain and
configuration, or any permitted variation must be specified and
verified. A successful compilation is not proof of semantic equivalence.

### Required publication conditions

A package must not become active merely because it compiles or can be
loaded.

Publication requires, at minimum:

1.  Authorized approval of the source rules.
2.  Successful structural and semantic validation.
3.  Successful conflict and consistency analysis.
4.  Verification that the transformation completed as specified.
5.  Conformance tests against the approved rule semantics.
6.  Integrity verification.
7.  Binding to an identifiable version and publication event.
8.  A defined activation and rollback procedure.

These are proposed conditions for the specification and approval
process.

# 6. Proposed conflict-resolution model

ADIE should not adopt a single universal conflict algorithm without
considering the meaning of the rules involved.

Instead, the Security Law Book should define an explicit
**rule-class-specific combination contract**.

## 6.1 Required distinctions

### Case A --- No applicable rule

The absence of an applicable permit must not result in implicit
permission.

For authorization decisions, the proposed default is denial unless a
valid, applicable authorization path explicitly establishes permission.

### Case B --- Applicable prohibition

An applicable, valid prohibition must be preserved through evaluation
and cannot be overridden merely because another rule permits the same
operation.

Whether a prohibition is binding in a particular situation depends on
the approved authority relationships and the rule's semantics.

### Case C --- Compatible constraints

Where several rules impose compatible requirements, the resulting
decision must satisfy all applicable mandatory constraints.

This is consistent with the project's frozen reference model of
intersecting applicable constraints. The precise representation of that
intersection remains to be specified.

### Case D --- Conflicting rules

The system must distinguish:

-   A legitimate precedence relationship that resolves the conflict.
-   A conflict requiring a defined combination rule.
-   A conflict for which the available authority information is
    insufficient.
-   An invalid policy set that must not be published.

The evaluator must not invent a resolution based on incidental ordering
or deployment configuration.

### Case E --- Missing, invalid, or unsupported information

Missing mandatory inputs, malformed policies, unsupported semantics,
untrusted rule sources, and failed validation must produce explicit
outcomes.

A policy that cannot be evaluated must not silently disappear from
consideration when it represents a mandatory constraint.

### Case F --- Conflicting lifecycle versions

A decision must not combine incompatible policy versions accidentally.

The evaluation must use an identified, approved policy state. If the
required state cannot be established, the decision must follow the
applicable failure contract rather than silently using an arbitrary
mixture of versions.

## 6.2 Proposed evaluation outcomes

The ADIE contract should distinguish at least the following states:

  -----------------------------------------------------------------------
  Outcome                 Meaning                 Proposed handling
  ----------------------- ----------------------- -----------------------
  `PERMIT`                All required            Proceed to the next
                          authorization           approved stage
                          conditions are          
                          satisfied               

  `DENY`                  An applicable           Reject
                          constraint or           
                          authorization rule      
                          prohibits the action,   
                          or no valid             
                          authorization path      
                          exists                  

  `NOT_APPLICABLE`        No rule in the          Apply the approved
                          evaluated scope applies enclosing-policy
                                                  semantics; never treat
                                                  this alone as
                                                  permission

  `INDETERMINATE`         The system cannot       Do not authorize the
                          establish a trustworthy protected action
                          decision                

  `INVALID_POLICY_SET`    The policy set fails    Reject publication or
                          required structural,    activation
                          semantic, or governance 
                          validation              

  `STALE_POLICY_STATE`    The required approved   Follow the explicit
                          version or effective    stale-state failure
                          state cannot be         contract; no implicit
                          established             permission
  -----------------------------------------------------------------------

These are proposed ADIE-level outcomes. They are not a claim that each
candidate engine uses identical outcome names or semantics.

The adapter must map engine-specific outcomes to ADIE outcomes without
losing meaningful distinctions.

# 7. Rule lifecycle and governance

The Security Law Book needs an explicit lifecycle. A file existing in a
repository is not sufficient evidence that its rules are authorized or
active.

## 7.1 Proposed lifecycle

`Author → Validate Authority and Ownership → Validate Structure and Semantics → Analyze Conflicts → Approve → Version and Seal → Publish → Activate → Monitor and Review → Amend, Suspend, or Withdraw`

The transition rules must specify who may perform each action and what
evidence is required.

### Important safeguards

**Approval must be distinct from authorship.** A rule's author cannot
establish its legitimacy merely by creating it.

**Amendments must create identifiable versions.** Changing an approved
rule must not silently rewrite the history of previous decisions.

**Suspension and withdrawal must have defined effects.** The system must
distinguish a rule that was never approved from a rule that was
previously approved and subsequently withdrawn.

**Activation must be controlled.** The fact that a new version has been
approved does not automatically establish when all evaluators and
enforcement points must begin using it.

**Historical reconstruction must respect effective time.** A later
policy version must not be substituted for the version governing a
historical decision without explicitly recording that the reconstruction
is a simulation rather than a reproduction of the original decision.

## 7.2 Effective-time semantics

ADIE must distinguish at least two concepts:

-   **Governance time:** when an approval, amendment, suspension, or
    withdrawal was recorded.
-   **Effective time:** when the approved rule is intended to govern
    decisions.

These timestamps can differ.

The final specification must define how to handle delayed publication,
clock disagreement, retroactive legal requirements, and policies that
are approved but not yet active.

These matters should not be solved by assuming that the latest timestamp
or latest repository revision always wins.

# 8. Conformance testing: common test set

The technology comparison must be converted into repeatable tests before
a final engine selection.

The following is the proposed common conformance suite. It is a test
design, not a claim that the engines have already passed it.

  ------------------------------------------------------------------------
  Test                    Scenario                 Required ADIE result
  ----------------------- ------------------------ -----------------------
  T01 --- Explicit        A valid permit applies   `PERMIT`
  authorization           and no binding           
                          constraint prohibits the 
                          action                   

  T02 --- Default denial  No applicable rule       `DENY`
                          establishes              
                          authorization            

  T03 --- Explicit        A binding prohibition    `DENY`, under the
  prohibition             and a permit both apply  approved authority and
                                                   combination semantics

  T04 --- Compatible      Multiple applicable      Decision follows the
  constraints             mandatory constraints    approved authorization
                          are satisfied            semantics

  T05 --- Missing         A required attribute is  No implicit permit;
  mandatory attribute     unavailable              return the defined
                                                   denial or indeterminate
                                                   outcome

  T06 --- Invalid         An input violates the    Reject the invalid
  attribute type          declared schema          request or return the
                                                   defined indeterminate
                                                   outcome

  T07 --- Policy          A required policy        Do not silently ignore
  evaluation error        encounters an evaluation a mandatory constraint
                          error                    

  T08 --- Unsupported     The evaluator cannot     Reject the package or
  policy construct        implement a required     return the defined
                          semantic construct       indeterminate outcome

  T09 --- Authority       Applicable rules         Do not invent
  conflict                conflict and no approved precedence; produce the
                          precedence relationship  defined conflict
                          resolves them            outcome

  T10 --- Policy-order    The same rules are       Result remains
  permutation             supplied in a different  unchanged unless an
                          incidental file order    explicitly approved
                                                   ordering rule applies

  T11 --- Policy version  A new version is         The evaluation uses an
  change                  approved while an        identified, internally
                          evaluation is in         consistent policy state
                          progress                 

  T12 --- Stale policy    A package is no longer   Apply the explicit
  package                 authorized for use       stale-state failure
                                                   behavior

  T13 --- Invalid package A package fails the      Do not activate the
  signature               configured               package
                          integrity/authenticity   
                          verification             

  T14 --- Unapproved      A structurally valid     Do not activate the
  package                 package has no valid     package
                          approval provenance      

  T15 --- Historical      The same request,        Reproduce the original
  reconstruction          inputs, policy versions, result to the defined
                          and evaluator profile    level of determinism
                          are replayed             

  T16 --- Semantic        A canonical rule is      Conformance tests
  transformation          compiled into an         establish preservation
                          executable               of the approved
                          representation           semantics

  T17 --- Schema          A schema change affects  Detect and review
  evolution               policies previously      affected policies
                          considered valid         before activation

  T18 --- Withdrawal      A rule is withdrawn      Preserve the old
                          while older decisions    version and its
                          remain relevant to       history; apply the new
                          historical               lifecycle state
                          reconstruction           prospectively according
                                                   to approved
                                                   effective-time rules

  T19 --- Cross-platform  A participating platform The platform must not
  representation          consumes a rule package  reinterpret or broaden
                          through the approved     the approved authority
                          contract                 

  T20 --- Repeated        Identical pinned inputs  Produce equivalent
  evaluation              and versions are         outcomes and consistent
                          evaluated repeatedly     decision metadata
  ------------------------------------------------------------------------

## 8.1 How the engines should be tested

The same logical cases should be implemented in each candidate engine
using equivalent inputs and intended semantics.

For each test, record:

-   The canonical rule set and its version.
-   The engine and version.
-   The policy translation and configuration.
-   The request and context inputs.
-   The raw engine result.
-   The ADIE-mapped result.
-   The relevant diagnostics.
-   Whether the result matches the expected outcome.
-   Any engine-specific workaround or semantic limitation.

A candidate must not be credited with passing a test simply because its
adapter converts an unsafe result into an apparently correct one. The
test must establish that the adapter preserves the intended rule
semantics and does not hide a mandatory evaluation failure.

**Selection gate:** No candidate should be selected as the initial
production evaluator until the required conformance suite has passed,
its limitations are documented, and the responsible architectural
authority has approved the selection.

# 9. Policy integrity, semantic preservation, and historical evidence

Three related concerns must be kept separate.

## 9.1 Integrity

Integrity verification establishes that a policy package matches the
expected signed or otherwise authenticated content.

OPA's signed-bundle support demonstrates one available approach. It
verifies configured signatures and bundle content against the expected
hashes. The configuration must require verification; otherwise, a
missing signature may not prevent activation.

ADIE must define its trust and key-management requirements separately
from the engine's capabilities.

## 9.2 Semantic preservation

A valid signature proves that content has not been altered relative to
the signed content. It does **not** prove that the content implements
the approved rule correctly.

For example, an authorized compiler could generate a correctly signed
package that contains a translation error.

ADIE therefore needs a separate conformance process that verifies the
relationship between:

1.  The approved canonical rule version.
2.  The transformation implementation and configuration.
3.  The executable package.
4.  The observed evaluator behavior.

The required level of proof must be proportionate to the policy's risk.
Test suites can provide useful evidence, but passing finite tests does
not automatically constitute a formal proof of semantic equivalence.

## 9.3 Historical reproducibility

To reconstruct a decision, ADIE should retain or reference enough
information to establish:

-   The request and relevant context, subject to data-minimization
    requirements.
-   The identity and provenance of the input attributes.
-   The exact canonical rule versions considered.
-   The effective policy package revision.
-   The evaluator implementation and relevant configuration.
-   The decision result and meaningful diagnostics.
-   The correlation identifier connecting the decision to subsequent
    execution and evidence.

The exact retained data and privacy controls belong in the detailed
specification.

Where an input is not retained for legitimate privacy or security
reasons, ADIE must record that limitation. It must not claim complete
historical reproducibility when the required evidence no longer exists.

# 10. Recommended technology disposition

The research supports the following decisions for the final proposal.

  -----------------------------------------------------------------------
  Item                    Recommendation          Status
  ----------------------- ----------------------- -----------------------
  ADIE canonical rule     Establish an            Proposed
  model                   engine-independent      
                          model                   

  Rule authority and      Govern separately from  Proposed
  approval lifecycle      policy evaluation       

  ABAC                    Use as a conceptual     Research-supported
                          foundation for          direction
                          applicability and       
                          authorization           
                          conditions              

  XACML                   Use as a principal      Research-supported
                          reference for explicit  direction
                          combining and           
                          indeterminate semantics 

  OPA/Rego                Retain as the leading   Conditional;
                          initial evaluator       conformance testing
                          candidate               required

  Cedar                   Retain as a serious     Conditional;
                          alternative for typed   conformance testing
                          authorization and       required
                          schema validation       

  XACML evaluator         Retain as an            Conditional;
                          alternative where its   conformance testing
                          semantics and           required
                          implementation          
                          trade-offs justify it   

  Policy integrity        Require verifiable      Proposed requirement
                          package integrity and   
                          controlled activation   

  Semantic preservation   Require explicit        Proposed requirement
                          conformance evidence    

  Rule-version history    Require immutable       Proposed requirement
                          version identity and    
                          traceable lifecycle     
                          events                  

  Universal conflict      Do not adopt without    Rejected as an
  algorithm               rule-class analysis     unsupported assumption

  Engine as authority     Do not allow an         Rejected
  source                  evaluator to determine  
                          governance legitimacy   

  Immediate               Do not begin until the  Not authorized by this
  implementation          proposal is reviewed    report
                          and the next            
                          specification is        
                          authorized              
  -----------------------------------------------------------------------

## Why OPA is the leading initial candidate

OPA is a reasonable first candidate because it offers declarative
evaluation, structured input, policy testing, signed bundles, and
decision-log metadata. These capabilities provide a practical starting
point for evaluating ADIE's needs.

However, this is a **candidate recommendation, not a final engine
selection**.

The decisive issue is whether OPA can satisfy ADIE's explicit handling
of mandatory constraints, evaluation errors, approved versions, and
semantic conformance without unacceptable workarounds.

Cedar may prove better for some authorization models. XACML may provide
a more direct fit where standardized combining and indeterminate
semantics are decisive. The common test suite must determine whether
those advantages outweigh integration and operational complexity.

ADIE should not select an engine on familiarity, popularity, performance
benchmarks alone, or the assumption that one engine's built-in
authorization behavior automatically matches the Security Law Book.

# 11. What ADIE must define itself

The investigation identifies a set of responsibilities that must be
explicitly governed by ADIE, even if existing technologies implement
parts of them.

### 11.1 Authority semantics

ADIE must define which sources can legitimately govern rules, how
ownership and approval authority are established, and how applicable
authority relationships are represented.

### 11.2 Canonical rule semantics

ADIE must define what its rules mean independently of an evaluator's
syntax, default behavior, or implementation-specific conventions.

### 11.3 Conflict and failure semantics

ADIE must define how compatible constraints combine, how prohibitions
operate, and how unresolved conflicts, missing mandatory information,
and unsupported semantics affect decisions.

### 11.4 Publication and version semantics

ADIE must define the lifecycle that turns an authored rule into an
approved, active, versioned rule and an executable package.

### 11.5 Transformation assurance

ADIE must define how the relationship between the canonical rule and
executable package is verified.

### 11.6 Historical reconstruction

ADIE must define the minimum evidence needed to explain which rules
governed a decision and how that decision can be reproduced or
reconstructed.

These are ADIE governance and architecture responsibilities. They do not
automatically require new standalone services or a new policy language.

# 12. Risks and unresolved questions

The proposal is technically defensible as a direction, but the following
matters remain open before architectural approval.

  -----------------------------------------------------------------------
  Risk or unresolved issue            Required resolution
  ----------------------------------- -----------------------------------
  Authority hierarchy is              Define the authoritative sources,
  underspecified                      ownership, applicability, and
                                      legitimate relationships between
                                      rule classes

  Legal requirements differ by        Establish an approved process for
  jurisdiction                        interpreting and encoding
                                      applicable legal constraints; do
                                      not infer legal precedence from
                                      software configuration

  One universal conflict algorithm    Define and test rule-class-specific
  may be inadequate                   combination semantics

  Engine errors can conceal mandatory Require explicit error mapping and
  constraints                         tests proving that required
                                      constraints cannot be silently
                                      bypassed

  Canonical and executable            Establish semantic conformance
  representations may diverge         criteria and transformation
                                      verification

  Policy changes may affect active    Define version pinning, activation,
  evaluations                         effective-time, and stale-state
                                      behavior

  Signature verification may be       Require tests proving that
  misconfigured                       unauthorized or unsigned packages
                                      cannot activate when verification
                                      is mandatory

  Historical inputs may be            Define evidence retention,
  unavailable                         minimization, and explicit limits
                                      on reconstruction

  Cross-platform interpretations may  Establish a shared rule contract
  differ                              and platform conformance
                                      requirements

  Candidate selection may precede     Keep engine selection conditional
  evidence                            until the common test suite and
                                      architecture review are complete
  -----------------------------------------------------------------------

A further limitation concerns authority over rule content. The proposal
establishes the need for governance and provenance, but it does not
independently determine the final legal or constitutional hierarchy of
all ecosystem rules. That requires the appropriate constitutional and
legal validation rather than a technical assumption.

# 13. Final proposal

## 13.1 Recommended architecture direction

I recommend that ADIE proceed with the following direction for Path 1:

1.  **Establish an engine-independent canonical Security Law Book
    model.**
2.  Separate authoritative rule governance from policy evaluation.
3.  Define authority precedence independently of evaluation precedence.
4.  Use ABAC concepts to represent applicability and authorization
    conditions.
5.  Use XACML's explicit combination and indeterminate semantics as a
    principal reference when defining ADIE's evaluation contract.
6.  Retain OPA/Rego as the leading initial engine candidate, with Cedar
    and XACML as alternatives.
7.  Define explicit ADIE outcomes for permit, deny, not applicable,
    indeterminate, invalid policy set, and stale policy state.
8.  Require controlled approval, immutable version identity, and
    traceable publication.
9.  Require executable-package integrity verification and semantic
    conformance testing.
10. Require sufficient versioned evidence to reconstruct the rules and
    evaluator state underlying a decision.
11. Select the production evaluator only after it passes the common
    conformance suite and receives the required architectural approval.

## 13.2 What this proposal deliberately does not decide

This proposal does not:

-   Approve a final database schema.
-   Select a production policy engine conclusively.
-   Define the complete legal or constitutional hierarchy.
-   Authorize a new standalone service.
-   Determine the deployment topology.
-   Redefine Core's identity, session, role, or permission
    responsibilities.
-   Change NoredAm's existing fail-closed behavior.
-   Define the complete Dynamic Authority Derivation contract.
-   Authorize implementation.

These boundaries preserve the separation between research, architecture,
specification, and implementation.

## 13.3 Proposed acceptance criteria for closing Path 1

Path 1 should be considered architecturally determined only after the
responsible authority has reviewed and accepted the following:

-   The canonical rule model and its required semantics.
-   The source-authority, ownership, and approval model.
-   Applicability and authority-precedence rules.
-   Conflict and failure semantics.
-   The rule lifecycle and effective-time model.
-   The policy-package publication and integrity requirements.
-   The semantic-conformance approach.
-   The candidate-engine comparison and conformance results.
-   The historical reconstruction requirements.
-   The compatibility and cross-platform contract.
-   The unresolved legal, governance, and operational constraints.

The current research report does not claim these acceptance criteria
have already been met.

# 14. Conclusion and next milestone

**The Security Law Book should be an ADIE-governed, engine-independent
authority model---not merely a policy-engine configuration.**

OPA/Rego, Cedar, and XACML provide useful but different mechanisms for
policy representation, evaluation, validation, and combination. The
central ADIE responsibility is to define the authoritative semantics
that those mechanisms must preserve.

The most important design safeguard is to prevent the evaluator's
defaults, error handling, file ordering, or implementation conventions
from silently becoming ADIE's authority hierarchy.

The proposed separation between canonical rules, governance, executable
policy packages, and conformance verification provides a defensible
foundation for that objective while preserving the option to use mature
technologies instead of rebuilding them.

### Recommended next milestone

**`ADIE-SEC-INV-001-P1 — Security Law Book Architecture Decision Review`**

The next milestone should be a focused architectural review of this
proposal. If accepted, the next engineering artifact should be a
**Security Law Book semantic specification** defining the canonical rule
model, authority relationships, lifecycle, decision outcomes, and
conformance contract.

The common OPA/Rego, Cedar, and XACML test suite should then provide the
evidence needed for the final evaluator selection.

This is a proposed next step, not an alteration to the approved roadmap
or an authorization to begin implementation.

**Final research disposition:** A concrete architecture direction is
available. The research supports separating ADIE's authoritative rule
semantics from the policy evaluator and supports a conditional
OPA/Rego-first evaluation strategy. Final architectural approval and
production-engine selection remain outstanding.
