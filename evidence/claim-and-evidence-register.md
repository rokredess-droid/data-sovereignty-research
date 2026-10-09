# Claim-and-Evidence Register
## Technically Enforceable Data Sovereignty Across Untrusted Infrastructure

**Status:** Initial external-source review; not a completed systematic literature review.  
**Purpose:** Separate supported claims, known mechanisms, limitations, and questions requiring further investigation. This register does not establish novelty.

## Executive finding

Existing work addresses privacy-enhancing cryptography, confidential computing, attestation, pseudonymisation, output leakage, and controlled key release. The defensible research question is whether existing mechanisms can be composed and independently verified to enforce purpose-constrained data custody across heterogeneous external services and the observable data lifecycle, including retention, disclosure, derived information, and revocation.

## Claim register

### C-01 — Transport and storage encryption do not by themselves protect plaintext during conventional processing
**Assessment:** Supported with qualification. If a service must process plaintext outside a separately protected mechanism, encryption in transit and at rest alone does not prevent that service from accessing it. This is not a claim that all external processing requires plaintext.

**Sources**
- EDPB Recommendations 01/2020: https://www.edpb.europa.eu/documents/recommendation/recommendations-012020-on-measures-that-supplement-transfer-tools-to_en
- NIST Privacy-Enhancing Cryptography: https://csrc.nist.gov/Projects/pec

### C-02 — Existing cryptographic methods can limit direct disclosure of inputs for selected computations
**Assessment:** Supported. Secure multiparty computation and fully homomorphic encryption support defined computation models with reduced disclosure of private inputs. Feasibility depends on workload, cost, threat model, correctness, and output leakage.

**Sources**
- NIST PEC tools: https://csrc.nist.gov/Projects/pec/pec-tools
- NIST FHE: https://csrc.nist.gov/Projects/pec/fhe
- IEEE Access SMPC survey (2024): https://ieeexplore.ieee.org/document/10498135/

### C-03 — Confidential computing and attestation can constrain access under a defined trust model
**Assessment:** Supported, implementation-dependent. Guarantees depend on hardware, firmware, measurements, attestation chain, policy, workload, interfaces, and configuration. Attestation alone does not prove application semantics are safe, outputs reveal nothing, or all retention paths are covered.

**Sources**
- Confidential Space security overview: https://docs.cloud.google.com/docs/security/confidential-space
- Confidential Space overview: https://docs.cloud.google.com/confidential-computing/confidential-space/docs/confidential-space-overview
- Attestation: https://docs.cloud.google.com/confidential-computing/docs/attestation
- NIST IR 8320E initial public draft: https://csrc.nist.gov/pubs/ir/8320/e/ipd

### C-04 — Supplementary technical measures may be insufficient when a recipient must access plaintext
**Assessment:** Supported within the EDPB guidance's specific legal and technical context. Do not generalize it into a universal claim that protected computation is impossible.

**Source**
- EDPB Recommendations 01/2020: https://www.edpb.europa.eu/documents/recommendation/recommendations-012020-on-measures-that-supplement-transfer-tools-to_en

### C-05 — Pseudonymisation is not equivalent to anonymisation or complete sovereignty
**Assessment:** Supported for personal data under the relevant GDPR framework. Pseudonymisation can reduce direct exposure but does not by itself establish that data is anonymous or unusable for other purposes.

**Sources**
- EDPB announcement: https://www.edpb.europa.eu/news/edpb-adopts-pseudonymisation-guidelines-and-paves-way-improve-cooperation-with_en
- EDPB Guidelines 01/2025: https://www.edpb.europa.eu/public-consultations/guidelines-012025-on-pseudonymisation_de

### C-06 — Protecting raw inputs does not automatically prevent sensitive disclosure through outputs or derived artifacts
**Assessment:** Material threat-model requirement; focused review is still needed. Outputs, metadata, logs, statistical results, embeddings, and derived artifacts may expose information. Evaluate channels per workload rather than making a universal categorical claim.

**Sources for follow-up**
- Leakage and Privacy at Inference Time survey: https://pubmed.ncbi.nlm.nih.gov/37015684/
- Survey of Privacy Attacks in Machine Learning: https://doi.org/10.1145/3624010

### C-07 — Owner-controlled keys and revocation do not alone control provider behaviour or recall disclosed information
**Assessment:** Supported as a design limitation. Policy-gated key release can constrain decryption, but cannot recall plaintext or derived information already observed, copied, exported, or learned. It does not alone prevent authorized-output leakage, endpoint compromise, or key exposure.

**Sources**
- Confidential Space security overview: https://docs.cloud.google.com/docs/security/confidential-space
- NIST PEC: https://csrc.nist.gov/Projects/pec

### C-08 — Novelty is undetermined
**Claim to avoid:** “No one has solved this problem before.”

**Assessment:** Existing systems and literature address substantial parts of the problem. A structured review must compare adversary model, plaintext visibility, protection in transit/at rest/in use, workload verification, output disclosure, retention, purpose limitation, revocation, portability, independent verification, and residual risk.

**Recommended wording:** “This investigation examines whether existing controls can be composed into a verifiable, purpose-constrained architecture for data custody across heterogeneous external capabilities. Novelty and feasibility require further comparative research and experimental validation.”

## Initial source inventory

| ID | Source | Supports | Does not prove |
|---|---|---|---|
| S-01 | EDPB Recommendations 01/2020 | Context-specific limits of supplementary measures | That all external processing is impossible to protect |
| S-02 | NIST Privacy-Enhancing Cryptography | Computation with reduced input disclosure | That every workload can avoid plaintext efficiently |
| S-03 | NIST FHE overview | Computation on encrypted data without the secret key | A drop-in solution for arbitrary computing |
| S-04 | Confidential Space documentation | Attested workload and role-separation model | Universal protection from every implementation, output, or hardware risk |
| S-05 | EDPB pseudonymisation guidance | Pseudonymised personal data may remain personal data | Universal re-identification outcomes |
| S-06 | Inference-time leakage survey | Need to consider inference-time leakage | That every output leaks sensitive data |

## Required follow-up

1. Complete structured related-work and patent review.
2. Examine model inversion, membership inference, data extraction, embedding leakage, differential privacy, logs, caches, backups, and telemetry.
3. Define distinct threat cases: privileged-but-protocol-constrained provider, malicious provider, compromised workload, and legal compulsion.
4. Specify measurable properties for key-release denial, workload substitution, purpose replay, output inference, retention-path inspection, revocation latency, result integrity, audit integrity, and fail-closed behaviour.
5. Record experimental results only after reproducible execution and evidence capture.

**Status:** Initial claim review complete; full literature review incomplete. The evidence supports continued investigation but does not establish a universal solution or novelty.
