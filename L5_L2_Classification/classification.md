# L5 Narrow / L2 General Classification — LIBERN_PLATFORM
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE

## L5 Narrow
LIBERN_PLATFORM specializes in sovereign AI platform provisioning: single-deployment stack
coordination via gRPC. No multi-cloud, no Terraform (that is TIER_3 territory). Scope: hardware
abstraction, GPU allocation, PAX module provisioning, inter-module gRPC routing.

## L2 General
Abstracts CPU/GPU/embedded hardware differences behind a uniform gRPC API. Higher-tier projects
never query hardware directly — they call LIBERN_PLATFORM.

## PAX Integration
PAX 27B is provisioned by LIBERN_PLATFORM: GPU memory allocation, inference context config,
capability advertisement via gRPC service definition.

## AIOSS Audit Relevance
Every platform event (module provision/deprovision, config change, GPU allocation) is chained.

## Regulatory
ISO/IEC 42001 (AI management system), NIST AI RMF 1.0, EU AI Act Art. 9 (risk management)
