Investigation status :

| ADIE responsibility          | Existing approaches worth investigating                                                                            | Gap that remains to be evaluated                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| Security Law Book            | Policy-as-code, policy governance, zero-trust policy inputs, risk-based control frameworks                         | How to represent the full hierarchy and resolve conflicts deterministically.                                |
| Dynamic Authority Derivation | Policy engines, contextual authorization, zero-trust decision models                                               | How to derive the intersection of all applicable constraints and define exact failure semantics.            |
| Portable Authority           | Biscuit, OAuth authorization mechanisms, token exchange, capability-based security                                 | How to bind authority to scope, delegation, context, validity, and revocation across independent platforms. |
| Independent Enforcement      | Policy Enforcement Points, application gateways, workload identity, execution guards                               | How to enforce independently at every boundary, including devices and side-effecting operations.            |
| Verified Effect              | Remote attestation, signed receipts, transaction evidence, state reconciliation, application-specific verification | How to establish that the intended effect occurred, not merely that a request was accepted.                 |
| Audit                        | Protected event records, correlated decision and execution evidence, cryptographic integrity mechanisms            | How to establish trustworthy, complete, privacy-conscious evidence across platform boundaries.              |

Path 1 — Security Law Book
Research question: How should ADIE represent, govern, version, and evaluate the authoritative rules that constrain every other path?

The first objective is to define a logical architecture for the Security Law Book itself.

Its investigation must establish:

How security rules are represented.
How rules are classified by authority and applicability.
How rule ownership and approval are represented.
How versions are identified and made immutable.
How conflicting rules are detected and resolved.
How rules are evaluated against a particular request or situation.
How historical decisions can be related to the exact rules in force at the time.

We must distinguish the authoritative policy content from the software that evaluates it.

A policy engine can execute rules. That does not make it the authority that determines which rules are legitimate.

| Capability                                    | Determination                                                                                                                |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Executable policy evaluation                  | **USE or ADAPT:** investigate established policy engines.                                                                    |
| Structured policy representation              | **ADAPT:** determine the required ADIE policy model and semantics.                                                           |
| Rule ownership and version identity           | **ADAPT:** establish an authoritative, verifiable policy lifecycle.                                                          |
| Constitutional authority hierarchy            | **DEVELOP if existing mechanisms do not satisfy the requirements:** the ADIE-specific governance semantics must be explicit. |
| Deterministic conflict resolution             | **DEVELOP or formally specify:** must not depend on undocumented precedence or implementation accidents.                     |
| Historical reconstruction of applicable rules | **ADAPT:** combine immutable policy identity with decision evidence.                                                         |

Current result: Existing policy technology is a credible foundation for rule evaluation. It is not a substitute for ADIE's authoritative rule-governance design.

The next research task for this path is to compare policy languages, rule-composition models, versioning mechanisms, and formal approaches to conflict resolution.

Path 2 — Dynamic Authority Derivation
Research question: How can ADIE derive the effective authority applicable to a specific actor, operation, resource, and context?

This path must investigate the derivation of authority from multiple applicable constraints.

The required inputs may include:

Authenticated identity.
The identity's established authority.
Delegation constraints.
Security Law Book rules.
Organisation and tenant policy.
Resource-specific policy.
The requested operation and its parameters.
Contextual and runtime conditions.
Risk or trust evidence where applicable.

The architectural question is how these inputs are transformed into a precise authority decision.

The investigation must establish the decision semantics, not merely choose an authorization engine.

| Capability                                    | Determination                                                                                                   |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Evaluation of structured policy rules         | **USE or ADAPT:** evaluate established policy engines.                                                          |
| Structured description of requested authority | **ADAPT:** investigate established authorization-request formats.                                               |
| Combining applicable constraints              | **DEVELOP or formally specify:** ADIE needs explicit derivation semantics.                                      |
| Deterministic conflict and failure handling   | **DEVELOP or formally specify:** define the result when inputs conflict or are unavailable.                     |
| Decision reproducibility                      | **ADAPT:** use immutable policy and input references, plus recorded decision evidence.                          |
| Authority decision under uncertain conditions | **DEVELOP or formally specify:** define which uncertainty requires denial, escalation, or restricted operation. |

Current result: Existing engines may execute the derivation logic, but they cannot be assumed to supply ADIE's complete derivation model.

The key research task is to formalise the rules by which applicable authority is combined and restricted.

Path 3 — Portable Authority
Research question: How can authority be represented so that independent systems can verify and constrain it without requiring the original authority source to participate in every operation?

The architecture must establish:

How authority is represented.
How its origin is verified.
How its scope and restrictions are expressed.
How delegation is represented.
How authority can be narrowed without being expanded.
How expiration and revocation are handled.
How the receiver determines which issuer and authority claims to trust.
How the representation can be used across independently operated platforms.

The investigation must distinguish a token's cryptographic validity from the legitimacy and current applicability of the authority it carries.

| Capability                                       | Determination                                                                           |
| ------------------------------------------------ | --------------------------------------------------------------------------------------- |
| Cryptographic verification of portable authority | **USE:** established cryptographic mechanisms.                                          |
| Attenuation and delegated authority              | **ADAPT:** investigate Biscuit and related capability-token designs.                    |
| Structured authority scope                       | **ADAPT:** compare Biscuit's model with established authorization-request formats.      |
| Cross-platform trust semantics                   | **DEVELOP or formally specify:** define which issuers and claims each receiver accepts. |
| Revocation and freshness                         | **ADAPT:** combine token properties with an explicit state and validity strategy.       |
| Complete ADIE authority semantics                | **DEVELOP:** no candidate is assumed to represent the whole approved model.             |

Current result: Biscuit is a serious candidate for adaptation, not a selected ADIE standard.

The research should compare its security properties and limitations with other capability-based and delegated-authorization approaches before the path architecture is determined.

Path 4 — Independent Enforcement at Every Execution Boundary

Research question: How can each protected execution boundary independently enforce the applicable authority, regardless of whether the requester is a user, service, subsystem, or AI agent?

A. Architectural structure to investigate

This path requires an enforcement architecture, not merely an authorization API.

The investigation must identify how enforcement applies to:

HTTP and API operations.
Inter-service requests.
Background jobs and message consumers.
Database-changing operations.
File and document operations.
AI tools and external connectors.
Device commands and industrial control operations.
Privileged configuration changes.

The path must distinguish central policy decisions from the mechanism that actually permits or blocks the operation.

| Capability                                   | Determination                                                                                              |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Policy enforcement at protected resources    | **USE or ADAPT:** established enforcement-point patterns.                                                  |
| Local policy evaluation                      | **USE or ADAPT:** evaluate OPA and equivalent approaches.                                                  |
| Exact-operation binding                      | **DEVELOP or formally specify:** define which operation fields must be bound to the authorization.         |
| Protection against bypass paths              | **DEVELOP and verify:** every route to a protected side effect must be identified and tested.              |
| Enforcement across different execution types | **ADAPT:** use boundary-specific enforcement mechanisms under common contracts.                            |
| Safe behaviour when dependencies fail        | **DEVELOP or formally specify:** define denial, escalation, and any explicitly permitted restricted modes. |

Current result: Established enforcement-point patterns are reusable. ADIE still needs an explicit contract that determines what an enforcement boundary must verify before allowing an operation.

The investigation must also distinguish logical enforcement responsibilities from deployment topology. A common contract does not require every boundary to run the same software.

Path 5 — Verified Effect

Research question: How can ADIE establish whether the intended operation actually occurred and whether its resulting state satisfies the required conditions?

A valid authorization decision proves that an operation was permitted under specified conditions. It does not prove that the operation succeeded or produced the intended result.

The path must determine:

What constitutes an effect for different operation classes.
Which component is the authoritative source of the resulting state.
How execution attempts are correlated with observed outcomes.
How partial completion is represented.
How retries and duplicate requests are handled.
How conflicting or delayed observations are resolved.
What evidence is sufficient to support a verification result.
How verification uncertainty is represented.

The investigation must distinguish an execution acknowledgement from independently verified state.

| Capability                                        | Determination                                                            |
| ------------------------------------------------- | ------------------------------------------------------------------------ |
| Cryptographic integrity of effect evidence        | **USE:** established signature and integrity mechanisms.                 |
| Correlation of an execution attempt with a result | **ADAPT:** define a stable operation identity and evidence relationship. |
| Verification against authoritative state          | **ADAPT:** use domain-specific authoritative observations.               |
| Independent verification of physical effects      | **DEVELOP or ADAPT:** requirements vary by device and safety context.    |
| Partial, ambiguous, or contradictory outcomes     | **DEVELOP or formally specify:** define the verification result model.   |
| Proof that a claimed effect is true               | **Investigate by operation class:** no universal mechanism is assumed.   |

Current result: Existing integrity mechanisms can protect evidence, and established state-observation mechanisms can support verification. ADIE still needs a defined model for determining what counts as a verified effect.

This path is likely to require domain-specific verification strategies under a common ADIE contract.

Path 6 — Audit
Research question: What evidence must ADIE retain so that security decisions and their outcomes can be reconstructed and assessed reliably?

The audit path must determine how to represent and protect evidence associated with:

Policy versions.
Authority derivation inputs and decisions.
Issued and delegated authority.
Enforcement decisions.
Execution attempts.
Verified effects.
Denials, failures, exceptions, and escalation.
Changes to the rules and configuration governing the operation.

It must also define evidence provenance, integrity protection, retention, access controls, privacy boundaries, and reconstruction requirements.

| Capability                                         | Determination                                                                        |
| -------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Structured decision records                        | **USE or ADAPT:** established logging mechanisms.                                    |
| Cryptographic record integrity                     | **USE:** established cryptographic primitives.                                       |
| Correlation across decision and execution evidence | **ADAPT:** define stable identifiers and record relationships.                       |
| Evidence completeness                              | **DEVELOP and verify:** define how missing or bypassed events are detected.          |
| Independent assurance of evidence provenance       | **ADAPT or DEVELOP:** depends on trust in the evidence producer and collection path. |
| Privacy-conscious retention and access             | **ADAPT:** establish explicit data-minimisation and retention rules.                 |

Current result: Existing logging and integrity mechanisms can supply important building blocks. They do not, individually, establish a complete audit-assurance architecture.
-----------------------------------------

| Technology or approach                      | Relevant path    | Current disposition                         | What ADIE must determine                                                             |
| ------------------------------------------- | ---------------- | ------------------------------------------- | ------------------------------------------------------------------------------------ |
| Policy-as-code engines, including OPA       | 1, 2, 4, 6       | **Investigate for use/adaptation**          | Policy semantics, governance boundaries, decision inputs, and enforcement contracts. |
| NIST Zero Trust Architecture                | 1, 2, 4          | **Use as an architectural reference**       | How its logical roles map to ADIE's distinct security responsibilities.              |
| Biscuit authorization tokens                | 3, potentially 4 | **Investigate for adaptation**              | Scope, attenuation, issuer trust, revocation, and cross-platform requirements.       |
| OAuth Rich Authorization Requests           | 2, 3             | **Investigate for adaptation**              | Whether its structured authorization descriptions suit ADIE's operation model.       |
| OAuth Token Exchange                        | 3                | **Investigate for adaptation**              | How token exchange interacts with ADIE's authority derivation and delegation rules.  |
| Remote attestation                          | 1, 2, 5          | **Use where the threat model justifies it** | Which trust claims are required and how attestation evidence is evaluated.           |
| Digital signatures and authenticated hashes | 3, 5, 6          | **Use as appropriate**                      | What is signed, who is trusted to sign it, and what the evidence actually proves.    |
| Local enforcement points                    | 4                | **Use/adapt as a pattern**                  | How to protect every relevant execution boundary.                                    |
| Domain-specific state verification          | 5                | **Investigate per operation class**         | What constitutes authoritative evidence of the intended effect.                      |
| Correlated audit records                    | 6                | **Adapt**                                   | Completeness, provenance, privacy, retention, and reconstruction semantics.          |

-----------------------------------------------------
. What ADIE may need to invent

The research must actively look for existing solutions before concluding that a capability requires new development.

Nevertheless, several ADIE-specific capabilities are likely to require explicit design, even if existing technologies implement parts of them.

4.1 Authority-governance semantics

A precise definition of which rules are authoritative, how they apply, and how conflicts are resolved.

4.2 Effective-authority derivation semantics

A deterministic model describing how applicable constraints combine and how uncertainty or missing inputs affect the result.

4.3 Portable-authority contract

A definition of what authority must mean across participating platforms, independent of the selected token format.

4.4 Execution-boundary contract

A precise definition of what a protected boundary must verify and enforce before an operation can cause a side effect.

4.5 Effect-verification model

A definition of how operation-specific evidence supports a verified, failed, partial, or indeterminate outcome.

4.6 Audit-assurance contract

A definition of the evidence required to reconstruct and assess an operation, including how integrity, provenance, and completeness are addressed.

These are research targets, not claims that no existing technology addresses them. Each must be compared against the scientific and engineering evidence before ADIE commits to developing its own mechanism.