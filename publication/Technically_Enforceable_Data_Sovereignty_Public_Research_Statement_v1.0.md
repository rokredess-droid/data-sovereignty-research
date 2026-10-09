# Technically Enforceable Data Sovereignty Across Untrusted Infrastructure
## Public Research Statement — Version 1.0

**Status:** Research problem statement and investigation framework. This document does not claim a demonstrated complete solution or established novelty.

## Abstract

Organisations increasingly rely on external digital services to store data, execute computation, process AI requests, and coordinate business activity. Encryption, access control, privacy-enhancing cryptography, confidential computing, and attestation can reduce specific forms of exposure, but each operates within a defined technical and trust boundary. Protecting stored inputs does not by itself establish control over processing, outputs, derived information, retention, onward transfer, or information already observed by a recipient.

This investigation asks whether an organisation can state, enforce, and independently verify purpose and access constraints across the observable lifecycle of a computation performed using external infrastructure, while explicitly documenting residual risks and assumptions.

## Research question

Can existing technical mechanisms be composed into a verifiable, purpose-constrained data-custody model that works across heterogeneous external services and execution environments without assuming that any one provider is fully trusted?

The investigation must distinguish enforceable properties from contractual promises, provider assertions, and properties that cannot be guaranteed after information has been disclosed.

## Scope

The investigation considers:

- authorization and declared-purpose enforcement;
- workload identity and attestation;
- input confidentiality and processing boundaries;
- output leakage and derived-data disclosure;
- persistence in storage, logs, caches, backups, and telemetry;
- onward transfer and secondary use;
- key ownership, conditional key release, and revocation;
- independent verification, evidence integrity, and reproducibility;
- provider portability and residual risks outside the observed boundary.

The scope does not assume that every workload can be executed without plaintext exposure, that every provider is malicious, or that all lifecycle behaviour can be observed or controlled.

## Threat model

The research should distinguish at least four cases:

1. **Privileged but protocol-constrained provider:** infrastructure administrators may control the host environment but cannot bypass specified technical controls under the assumed hardware and attestation model.
2. **Malicious provider or operator:** an operator may attempt unauthorized access, workload substitution, policy bypass, retention, or disclosure.
3. **Compromised workload or endpoint:** application code, dependencies, credentials, or endpoints may expose information despite infrastructure-level protections.
4. **Legal compulsion:** a provider may be compelled to disclose information or operate within legal constraints. Technical controls do not automatically eliminate this risk.

These cases require separate assumptions and success criteria; they must not be collapsed into a single generic adversary.

## Existing technical context

Relevant areas include privacy-enhancing cryptography, secure multiparty computation, fully homomorphic encryption, confidential computing, remote attestation, pseudonymisation, key-release policy, and research on inference-time leakage. These mechanisms address different problems and carry different performance, trust, implementation, and disclosure limitations.

The research must not claim that these mechanisms do not exist. Nor should it assume that they can be combined into a complete solution without evidence.

## Candidate security properties

The following are proposed properties to investigate, not results already demonstrated:

- unauthorized key release is denied;
- workload substitution is detected or rejected;
- purpose-bound authorization cannot be replayed outside its defined context;
- outputs are evaluated for sensitive inference and disclosure;
- observable persistence and transfer paths are documented;
- revocation blocks future authorized access within a measured time bound;
- result integrity and audit integrity can be independently checked;
- policy or attestation failures cause execution to fail closed;
- assumptions and residual risks are explicit and testable.

## Proposed experiments

Each experiment should record its identifier and version, hypothesis, threat model, system boundary, configuration, procedure, expected result, pass/fail criteria, observed result, evidence artifacts, integrity hashes, deviations, and residual risks.

Initial experiments should examine:

1. permission minimisation for a concrete outcome;
2. key-release denial under invalid or stale attestation;
3. workload substitution and identity mismatch;
4. purpose-policy replay and context changes;
5. output inference and derived-data leakage;
6. retention paths across storage, logs, backups, and telemetry;
7. revocation latency and behaviour after revocation;
8. result integrity, audit integrity, and fail-closed operation.

A successful experiment demonstrates only the property tested under the recorded conditions. It does not prove the behaviour of unobserved provider systems or recall information already copied or learned.

## Evaluation criteria

The investigation should report, at minimum:

- which authority was requested and actually granted;
- what data each execution boundary can observe;
- what component was measured and what evidence was verified;
- which policy decision was enforced and where;
- what outputs and derived artifacts may disclose;
- what persistence and transfer paths were observed;
- what revocation blocks and what it cannot recall;
- what evidence can be independently reproduced;
- what assumptions, failures, and residual risks remain.

## Limitations

No single mechanism should be treated as a universal guarantee. Confidential computing depends on its trusted computing base and attestation policy. Cryptographic techniques support particular computation models and may impose substantial costs. Output leakage, endpoint compromise, legal compulsion, and information already disclosed remain material concerns. The present review is selective rather than systematic, and no novelty conclusion is established.

## Related work and source anchors

- NIST Privacy-Enhancing Cryptography: https://csrc.nist.gov/Projects/pec
- NIST Fully Homomorphic Encryption: https://csrc.nist.gov/Projects/pec/fhe
- EDPB Recommendations 01/2020: https://www.edpb.europa.eu/documents/recommendation/recommendations-012020-on-measures-that-supplement-transfer-tools-to_en
- EDPB Guidelines 01/2025 on pseudonymisation: https://www.edpb.europa.eu/public-consultations/guidelines-012025-on-pseudonymisation_de
- Confidential Space security overview: https://docs.cloud.google.com/docs/security/confidential-space
- NIST IR 8320E initial public draft: https://csrc.nist.gov/pubs/ir/8320/e/ipd
- Leakage and Privacy at Inference Time survey: https://pubmed.ncbi.nlm.nih.gov/37015684/
- Verifiability for privacy-preserving computing on distributed data — survey: https://link.springer.com/article/10.1007/s10207-025-01047-7
- Confidential computing and related technologies: a critical review: https://link.springer.com/article/10.1186/s42400-023-00144-1
- MITRE Framework for Continuous Remote Attestation: https://www.mitre.org/news-insights/publication/framework-continuous-remote-attestation
- U.S. Patent 12,483,393 B2: https://patents.google.com/patent/US12483393B2/en

These are source anchors for further review, not an exhaustive bibliography or legal opinion. Each source must be assessed against the specific claim it is used to support.

## Conclusion

The investigation concerns whether purpose constraints can be technically enforced and independently verified across the observable lifecycle of computation involving external infrastructure. Existing work covers substantial parts of this space. The next defensible step is a structured comparison and reproducible experiments. A universal solution, complete provider control, and research novelty are not established by this statement.
