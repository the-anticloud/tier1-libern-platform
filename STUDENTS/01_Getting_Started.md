# LIBERN_PLATFORM — Student Getting Started

## What You'll Build
A multi-tenant AI platform that manages multiple users sharing one local inference server, with quota tracking and priority routing.

## Prerequisites
- Python 3.10+
- Ollama running locally
- Basic understanding of REST APIs

## Install
```bash
pip install libern-platform
```

## First Working Example
```python
from libern import Platform, Tenant

# Start a local platform with one model backend
platform = Platform(model_backend="ollama/mistral:7b")

# Register two tenants with different quotas
platform.register_tenant(Tenant(id="alice", quota={"tokens_per_hour": 10000}))
platform.register_tenant(Tenant(id="bob", quota={"tokens_per_hour": 5000}))

# Each tenant gets their own client
alice = platform.get_client("alice")
bob = platform.get_client("bob")

print(alice.complete("What is the Anticloud system?", max_tokens=100).text)
print(bob.complete("Explain platform engineering", max_tokens=100).text)
print(f"Alice tokens used: {alice.usage.tokens_today}")
```

## Check Quota and Usage
```python
from libern import Platform

platform = Platform.from_config("libern_config.yaml")
for tenant_id in platform.list_tenants():
    usage = platform.get_client(tenant_id).usage
    print(f"{tenant_id}: {usage.tokens_today}/{usage.quota.tokens_per_hour} tokens")
```

## On Kaggle (loiskleinner account, T4 GPU)
```python
!pip install libern-platform
!curl -fsSL https://ollama.ai/install.sh | sh
!ollama serve &
import time; time.sleep(5)
!ollama pull mistral:7b
# Run multi-tenant demo with 5 simulated users
```

## What's Next
- Simulate quota exhaustion by sending many requests from one tenant
- Try `overflow_action="degrade"` to see graceful degradation
- Connect to KAZCADE_RUNTIME for production-grade serving
