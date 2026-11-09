# RedTap — Jurisdiction-Aware AI Governance Architecture Specification
**Version 1.0 · Research and Architecture Baseline · 5 November 2026**

> Security Law Book → Dynamic Authority Derivation → Portable Authority → Independent Enforcement → Verified Effect → Audit

## Status
Architecture specification baseline drafted following approval. This is an architectural proposal, not legal advice, certification, or a claim of implementation.

## Executive summary
Consolidates:
1. Regulatory sandbox integration with the Security Law Book.
2. Jurisdiction-aware governance portability.
3. Technical governance from AI request to verified effect and audit.

Regulatory approvals and legal obligations are inputs to governance—not unrestricted runtime permissions. AI models propose actions; independent enforcement points decide whether actions may execute. Acknowledgement is not proof of effect.

## Architecture invariants
- Security Law Book remains the authoritative internal governance constraint system.
- Effective authority is the intersection of applicable constraints.
- Regulatory approval, data permission, organisational delegation and runtime authority are distinct.
- AI output is untrusted until independently validated and authorised.
- Every consequential execution boundary independently enforces authority.
- Portable authority is bounded, authenticated, contextual, time-limited and revocable/expiry-controlled.
- Dispatch success is not verified effect.
- Audit correlates policy, authority, execution and verification while minimising data.
- Destination jurisdiction and resource controls are re-evaluated.
- Fail closed for unsafe operations; safe-state procedures must be explicitly engineered.

## Logical planes
External Governance Sources → Regulatory Adapter → Jurisdiction Resolver → Security Law Book → Policy Resolver → Dynamic Authority Derivation → Portable Authority Service → AI Orchestrator → Execution Gateway → Execution Target → Verification Service → Evidence & Audit; Human Oversight spans the lifecycle.

## Regulatory integration
Source registry records official URI, issuer, provision, legal status, publication/effective dates, version, retrieval timestamp, digest, reviewer and interpretation. Candidate constraints are source-linked, reviewed, versioned and approved. Amendments, expiry, withdrawal and source changes trigger impact review.

Sandbox participation, regulatory approval, data-owner consent, lawful processing basis and production authorisation are separate legal/operational objects.

## Jurisdiction portability
Portable semantics do not make laws or permissions portable. Destination jurisdiction must independently resolve applicability and re-derive authority. UAE approval does not automatically authorise EU/Türkiye deployment; data transfer and processing require separate checks.

## Portable authority logical fields
Authority ID; issuer/key ID; principal/delegation chain; subject/audience; action/scope; constraints; jurisdiction context; policy references; risk/safeguards; validity; revocation; nonce/request binding; evidence reference; integrity protection.

Verifier checks issuer, integrity, audience, subject, action, resource, time, delegation, revocation/freshness and request binding. Downstream components cannot widen scope.

## Execution protocol
Authenticate → Resolve actor → Parse intent → Validate schema → Resolve context → Evaluate policy → Assess risk → Derive authority → Enforce boundary → Execute → Observe → Verify → Recover → Audit.

Outcomes: ALLOW, DENY, ESCALATE, DEFER, SUSPEND.

## Effect states
PROPOSED → AUTHORISED → DISPATCHED → ACKNOWLEDGED → EFFECT_OBSERVED → VERIFIED. Failure, indeterminate, compensation and suspension are explicit states. An API success response is only one signal; independent observation is required where effect matters.

## Audit
Correlated IDs across request, decision, authority, execution, verification and incident. Record source/policy/profile versions, reasons, scope, revocation checks, target, operation, idempotency, observed result, verification method, human approvals and recovery. Protect integrity and access; minimise personal data and enforce retention/deletion rules.

## Threat model
Prompt/tool injection; authority escalation; confused deputy; replay; stale/revoked policy; cross-jurisdiction laundering; data exfiltration; model/provider substitution; false success; audit tampering; approval spoofing/fatigue; adapter hallucination/drift; dependency outage; unsafe rollback.

## Minimum conformance tests
- Out-of-scope approval cannot authorise other resources/use cases.
- Expired, revoked, forged, wrong-audience or altered authority is rejected.
- Foreign approval cannot bypass destination requirements.
- Model-generated prohibited action is blocked at execution boundary.
- Policy changes invalidate or re-check authority.
- Data access/transfer checked separately.
- Required human approval cannot be spoofed.
- Acknowledgement without independent effect evidence is not VERIFIED.
- Missing/contradictory evidence becomes INDETERMINATE/FAILED.
- Replay, idempotency, revocation, suspension and provider substitution tested.
- Audit chain correlates events and source/policy versions.
- Sandbox exit/production transition triggers explicit scope review.

## RedTap integration
Reuse Identity, Authorization, Tenant/Branch isolation, Module Registry/Lifecycle, Infrastructure/connectors, Domain Events/Audit and AI service foundations. Do not create parallel identity or permission systems. Connectors, APIs and devices are enforcement boundaries.

## Open decisions
Policy schema; legal review ownership; applicability/conflict resolution; signing/key custody/revocation/offline behavior; gateway coverage; verification standards; audit integrity/retention; human oversight; risk taxonomy; safe-state behavior; sandbox-to-production gate; SLOs.

## Official references
- [Sandbox Dubai AI Sandbox](https://sandboxdubai.gov.ae/sandboxes/ai-sandbox)
- [Sandbox Dubai mandate/process](https://sandboxdubai.gov.ae/home)
- [EU AI Act Article 57 — Regulatory sandboxes](https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-57)
- [EU AI Act Article 60 — Real-world testing](https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-60)
- [NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)
- [OECD AI Principles](https://www.oecd.org/en/topics/ai-principles.html)
- [KVKK chatbot information note](https://www.kvkk.gov.tr/Icerik/8047/Sohbet-Robotlari-ChatGPT-Ornegi-Hakkinda-Bilgi-Notu)
- [EU AI Act consolidated text](https://eur-lex.europa.eu/eli/reg/2024/1689/2026-07-27/eng)

## Next engineering gate
Architecture review of open decisions. After approval, split into atomic packages: regulatory registry; jurisdiction profiles/resolver; authority object/verifier; execution gateway; effect verification/evidence; threat-driven conformance tests. No production implementation before trust boundaries, safe-state behavior, legal review ownership and acceptance criteria are approved.
