# Experimental Validation

## Objective

Evaluate whether an organisation can state, enforce, and independently verify purpose and access constraints across the observable lifecycle of data processed by external infrastructure.

## Status

No experiment in this directory is represented as completed. This file records the proposed validation approach; results must be added only after execution and evidence capture.

## Candidate test dimensions

1. **Authorization boundary** — record the requested outcome, requested permissions, granted authority, and whether a narrower path achieves the outcome.
2. **Workload identity and attestation** — define what component was measured, what evidence was verified, who verified it, and which trust assumptions remain.
3. **Input and output handling** — track data supplied to a service, outputs returned, and disclosures that may reveal sensitive information.
4. **Persistence and onward transfer** — identify observable storage, logs, backups, telemetry, and transfer paths within the test boundary.
5. **Revocation and deletion** — distinguish denial of future access from removal of previously copied or observed information.
6. **Independent verification** — preserve reproducible configuration, timestamps, evidence artifacts, expected outcomes, and actual outcomes.

## Required record for each experiment

- Experiment ID and version
- Hypothesis and threat model
- System boundary and components
- Preconditions and test configuration
- Procedure
- Expected result and pass/fail criteria
- Observed result
- Evidence artifacts and integrity hashes
- Deviations, limitations, and residual risks
- Reproduction instructions

## Interpretation rule

A successful test demonstrates only the property tested under the recorded conditions. It must not be generalized to unobserved provider systems, undisclosed copies, legal compulsion, or information already learned by a recipient.
