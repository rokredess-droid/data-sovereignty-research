# Focused Related-Work Assessment — Version 0.2

**Research:** Technically Enforceable Data Sovereignty Across Untrusted Infrastructure  
**Date:** 9 October 2026  
**Status:** Initial focused assessment; not a systematic literature or patent review.

## 1. Executive finding

The broad question overlaps substantially with existing work. Three especially relevant areas are: privacy-preserving computation with verifiability, continuous remote attestation, and technical mechanisms for controlling remote data through confidential computing. This overlap means novelty must not be asserted without a structured comparison.

## 2. Relevant prior work

### Privacy-preserving computation and verifiability
A 2025 survey compares 41 schemes related to privacy-preserving computing on distributed data and verifiability. This establishes that verifiability in privacy-preserving computation is an active, populated research area.

Source: https://link.springer.com/article/10.1007/s10207-025-01047-7

### Confidential computing
A 2023 critical review surveys confidential computing and related technologies, including trust assumptions and limitations. Confidential computing can reduce infrastructure-operator access to workload plaintext, but its guarantees depend on the hardware, firmware, attestation, workload, interfaces, and threat model. It does not automatically prove safe application semantics, absence of output leakage, or complete lifecycle control.

Source: https://link.springer.com/article/10.1186/s42400-023-00144-1

### Continuous remote attestation
MITRE published a public-review framework for continuous remote attestation in 2026. This is directly adjacent to proposals that require ongoing workload-state evidence rather than a one-time trust decision.

Source: https://www.mitre.org/news-insights/publication/framework-continuous-remote-attestation

### Remote data control using confidential computing
U.S. Patent 12,483,393 B2, issued 25 November 2025, is titled “Method of controlling remote data based on confidential computing and system thereof.” Its claim set describes controlling remote data through confidential-computing mechanisms. This is a preliminary prior-art indicator, not a legal opinion or exhaustive patent analysis.

Source: https://patents.google.com/patent/US12483393B2/en

### Inference-time leakage
A 2023 survey in IEEE Transactions on Pattern Analysis and Machine Intelligence reviews privacy leakage and attacks at inference time. This supports treating outputs and derived information as part of the threat model, rather than protecting raw inputs alone.

Source: https://pubmed.ncbi.nlm.nih.gov/37015684/

### Regulatory supplementary measures and pseudonymisation
EDPB Recommendations 01/2020 discuss the context-specific limits of supplementary technical measures for data transfers. EDPB Guidelines 01/2025 address pseudonymisation. These sources are authoritative for their relevant legal context but do not prove a universal technical impossibility.

Sources:
- https://www.edpb.europa.eu/documents/recommendation/recommendations-012020-on-measures-that-supplement-transfer-tools-to_en
- https://www.edpb.europa.eu/public-consultations/guidelines-012025-on-pseudonymisation_de

## 3. Implications for the research claim

Do not claim that confidential computing, privacy-enhancing cryptography, attestation, verifiability, pseudonymisation, or remote data-control mechanisms do not exist. They do.

The potentially useful question is narrower:

> Can an organisation state, enforce, and independently verify declared purpose and access constraints across the observable lifecycle of a computation—including authorization, workload identity, inputs, outputs, persistence, onward transfer, and future access—while explicitly identifying residual risks after information has been observed or copied?

This question is a research direction, not a finding of novelty.

## 4. Comparison dimensions required

Any proposed architecture or claim should be compared against prior work using at least these dimensions:

1. Adversary and trust model.
2. Plaintext visibility at provider, workload, and endpoint boundaries.
3. Protection in transit, at rest, and in use.
4. Workload identity and attestation freshness.
5. Output leakage and derived-data disclosure.
6. Persistence through logs, caches, backups, and telemetry.
7. Purpose limitation and authorization enforcement.
8. Key ownership, release conditions, and revocation semantics.
9. Portability across providers and heterogeneous execution environments.
10. Independent verification and reproducibility.
11. Legal compulsion and residual risks outside the technical boundary.
12. Experimental evidence and demonstrated limitations.

## 5. Recommended next work

1. Complete a structured literature review with search terms, inclusion criteria, dates, and a reproducible source register.
2. Conduct a claim-by-claim patent review before making novelty statements.
3. Define measurable experiments for key-release denial, workload substitution, purpose replay, output inference, retention-path inspection, revocation latency, result integrity, audit integrity, and fail-closed behaviour.
4. Report only observed results and preserve the configuration, evidence artifacts, and limitations for each test.

## 6. Conclusion

The initial review indicates meaningful prior art in each major technical area. It does not determine whether a carefully bounded end-to-end composition is novel. The next defensible milestone is a structured comparison and experimental validation, not a broad claim that no existing solution exists.
