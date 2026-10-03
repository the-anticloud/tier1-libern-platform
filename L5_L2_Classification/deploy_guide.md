# Deploy Guide — LIBERN_PLATFORM
## Prerequisites
- Python 3.11+, grpcio 1.62+, protobuf 4.25+, psutil 5.9+, PAX 27B weights

## Environment
- 16GB RAM. GPU optional. gRPC on localhost:50051.

## Install
```bash
pip install anticloud-libern grpcio protobuf psutil
```

## Start platform
```bash
python -m libern_platform --config ./libern_config.yaml --port 50051
```

## Air-Gap
gRPC is local-only. All protobuf definitions compile offline.

## AIOSS Integration
```bash
aioss init --module LIBERN_PLATFORM --output ./platform.aioss
```

## Verification
```bash
grpc_health_probe -addr=localhost:50051
aioss verify --chain ./platform.aioss
```
