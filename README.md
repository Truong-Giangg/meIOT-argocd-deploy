# meIOT ArgoCD Deploy

ArgoCD configuration repository for deploying the meIOT Core API service using Helm charts stored in Azure Container Registry.

## Repository Structure

```
├── applications/
│   └── core-api/
│       └── argocd.yaml          # Core API ArgoCD Application manifest
├── helm/
│   └── core-api/
│       └── values.yaml          # Helm values for Core API deployment
└── README.md
```

## Overview

This repository contains the GitOps configuration for meIOT using ArgoCD and Helm. It uses a dual-repository pattern:
- **Chart Repository**: Azure Container Registry (`meiot.azurecr.io/helm/meiot-helm-chart`) - stores the Helm chart template
- **Values Repository**: This repository contains the environment-specific values and configuration

## Chart Details

- **Chart Name**: dot-net-chart
- **Version**: 1.0.1
- **Registry (OCI ACR)**: oci://meiot.azurecr.io/meiot-helm-chart
- **Release Name**: core-api
- **Target Namespace**: meiot

## Configuration

Edit `helm/core-api/values.yaml` to customize:
- Container image repository and version (points to meiot.azurecr.io)
- Azure Container Registry credentials (imagePullSecrets)
- External secret store configuration (Vault backend)
- MongoDB and Kafka connection details
- Resource requests and limits
- Ingress hostname and TLS settings

## Key Settings

**Image Configuration:**
- Registry: `meiot.azurecr.io`
- Repository: `meiot-core-api`
- Tag: `v1.0`
- Pull Secret: `azurecr-secret`

**External Secrets:**
- Store: Vault backend
- MongoDB credentials: meiot-core-api-mongo-user, meiot-core-api-mongo-pass

**Resources:**
- Memory: 256Mi (request) / 512Mi (limit)
- CPU: 250m (request) / 500m (limit)

**Ingress:**
- Hostname: core-api.meiot.local
- TLS: Enabled with Let's Encrypt

## Deployment

This repository is automatically synced by ArgoCD. The core-api application will be deployed to the `meiot` namespace with the Helm values specified in this repository.

### Prerequisites
- ArgoCD installed and configured
- Kubernetes cluster with meiot namespace
- Azure Container Registry credentials configured
- Vault backend for external secrets management
- MongoDB and Kafka services running in meiot namespace
