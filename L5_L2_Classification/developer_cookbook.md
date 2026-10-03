# Developer Cookbook — LIBERN_PLATFORM
**Stack:** Python 3.11, gRPC, protobuf, psutil

## Initialize and provision
```python
from libern_platform import LibernPlatform
platform = LibernPlatform.from_config("./libern_config.yaml")
platform.provision_all()
print(platform.status())
```

## Query capabilities
```python
caps = platform.capabilities()
print(f"GPU: {caps.gpu_available}, PAX: {caps.pax_loaded}, AIOSS: {caps.aioss_active}")
```

## Deploy a module
```python
platform.deploy_module(name="K_BRAINFLOW", tier="TIER_7_BIOSIGNALS_NEURO",
                       config={"sample_rate": 256, "channels": 16})
```

## gRPC client
```python
import grpc
from libern_platform.proto import platform_pb2_grpc, platform_pb2
channel = grpc.insecure_channel("localhost:50051")
stub = platform_pb2_grpc.LibernStub(channel)
resp = stub.Query(platform_pb2.QueryRequest(module="PAX_INFERENCE", payload=query_bytes))
```

## Performance
gRPC + protobuf: ~3x faster than REST/JSON. Streaming RPCs for token streaming.
Keepalive: `options=[('grpc.keepalive_time_ms', 10000)]`.

## Integration
Below KAZCADE_RUNTIME, above hardware. Serves INTE11ECT_APP, MIIRAI_CHAT, all TIER_2 PAX_* modules.
