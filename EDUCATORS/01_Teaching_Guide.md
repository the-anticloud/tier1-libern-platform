# LIBERN_PLATFORM — Educator's Teaching Guide

## Course Fit: platform engineering, multi-tenancy, AI infrastructure, DevOps

## 3-Week Module: Sovereign AI Platform Architecture

### Week 1: Platform Engineering for AI
**Lecture Topics:**
- What LIBERN_PLATFORM provides: multi-tenant isolation, model routing, quota management
- Platform vs. infrastructure: abstraction layers in the Anticloud stack
- Tenant isolation models: process-level, container-level, namespace-level
- API design for AI platform services

**Lab Exercise:**
```python
from libern import Platform, Tenant
platform = Platform.from_config("libern_config.yaml")
tenant = Tenant(id="team-alpha", quota={"tokens_per_day": 1_000_000, "gpu_hours": 4})
platform.register_tenant(tenant)
client = platform.get_client(tenant_id="team-alpha")
response = client.complete("Explain the LIBERN platform architecture",
                            model="ollama/mistral:7b")
print(response.text)
print(f"Tokens used today: {client.usage.tokens_today}")
```

### Week 2: Quota Management and Routing
**Lecture Topics:**
- Token bucket algorithms for rate limiting
- Priority routing: premium tenants get GPU, standard tenants get CPU
- Observability: per-tenant metrics, latency histograms, error rates
- Graceful degradation when quota is exceeded

**Lab Exercise:**
```python
from libern import Platform, QuotaPolicy
platform = Platform.from_config("libern_config.yaml")
policy = QuotaPolicy(
    burst_tokens=10_000,
    refill_rate=1_000,  # tokens per minute
    overflow_action="queue"  # options: queue, reject, degrade
)
platform.set_policy("team-alpha", policy)
# Simulate quota exhaustion
for i in range(20):
    r = platform.get_client("team-alpha").complete("test", max_tokens=1000)
    print(f"Request {i}: {r.status}")
```

### Week 3: LIBERN_PLATFORM and the Anticloud Ecosystem
**Lecture Topics:**
- LIBERN as the platform layer over KAZCADE_RUNTIME and PAX
- Integration with SOVEREIGN_OS for system-level access control
- AIOSS format for cross-tenant audit logs
- Deploying LIBERN with ANTICLOUD_DEPLOYMENT

**Lab Exercise:**
```python
from libern import Platform
from aioss_format import AIOSSLedger
ledger = AIOSSLedger()
platform = Platform.from_config("libern_config.yaml", ledger=ledger)
platform.serve()
ledger.export("libern_audit.aioss.json")
```

## Exam Questions
1. Explain the token bucket algorithm. How does it allow short bursts while enforcing long-term rate limits?
2. Describe three tenant isolation strategies. For a university lab setting with 50 student tenants on one GPU server, which would you choose and why?
3. What AIOSS ledger fields would you record for each LIBERN_PLATFORM request to satisfy an enterprise compliance audit?
