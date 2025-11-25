# Kagent Agents

This directory contains pre-built AI agents for various Kubernetes and cloud-native use cases. Each agent is a Helm chart that can be deployed alongside the main kagent installation.

## Available Agents

### Core Kubernetes Agents

| Agent | Description |
|-------|-------------|
| `k8s` | Kubernetes Expert Agent for cluster operations, troubleshooting, and maintenance |
| `helm` | Helm package management agent for release lifecycle management |
| `observability` | Observability agent for Prometheus, Grafana, and Kubernetes monitoring |
| `promql` | PromQL query generation agent for Prometheus |

### Service Mesh Agents

| Agent | Description |
|-------|-------------|
| `istio` | Istio service mesh expert for traffic management, security, and observability |
| `kgateway` | Kubernetes Gateway API expert agent |

### Network Agents

| Agent | Description |
|-------|-------------|
| `cilium-debug` | Cilium debugging and troubleshooting agent |
| `cilium-manager` | Cilium lifecycle and configuration management agent |
| `cilium-policy` | Cilium network policy management agent |

### GitOps Agents

| Agent | Description |
|-------|-------------|
| `argo-rollouts` | Argo Rollouts agent for progressive delivery |

### TelcoCloud Agents

These agents are specialized for telecommunications and 5G network use cases:

| Agent | Description |
|-------|-------------|
| `5g-core` | 5G Core Network Function management (AMF, SMF, UPF, NRF, AUSF, UDM, PCF, NSSF) |
| `cnf-lifecycle` | Cloud-Native Network Function lifecycle management (deploy, upgrade, scale, heal) |
| `network-service-assurance` | Telecom service monitoring, SLA management, and fault detection |
| `network-slice` | 5G network slice provisioning and management (eMBB, URLLC, MIoT) |
| `telco-edge` | Edge computing and MEC (Multi-access Edge Computing) management |

## Installing Agents

Agents can be installed after the main kagent installation:

```bash
# First ensure kagent is installed
helm install kagent ./helm/kagent/ --namespace kagent

# Install an agent (e.g., 5g-core agent)
helm install 5g-core-agent ./helm/agents/5g-core/ \
  --namespace kagent \
  --set modelConfigRef=default-model-config
```

## Agent Configuration

Each agent accepts the following common configuration values:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `modelConfigRef` | Reference to a ModelConfig resource | `""` (uses default) |
| `resources.requests.cpu` | CPU request | `100m` |
| `resources.requests.memory` | Memory request | `256Mi` |
| `resources.limits.cpu` | CPU limit | `1000m` |
| `resources.limits.memory` | Memory limit | `1Gi` |

## TelcoCloud Agent Details

### 5G Core Agent

The 5G Core Agent specializes in managing 5G core network functions following 3GPP standards. It understands:

- **Network Functions**: AMF, SMF, UPF, NRF, AUSF, UDM, PCF, NSSF
- **Interfaces**: N1-N22 service-based interfaces
- **Operations**: Registration, session management, service discovery
- **Troubleshooting**: Call flow analysis, NF communication issues

Example use cases:
- "Check the status of all 5G core network functions"
- "Troubleshoot UE registration failures"
- "Scale the UPF deployment for increased traffic"

### CNF Lifecycle Agent

The CNF Lifecycle Agent manages the complete lifecycle of Cloud-Native Network Functions:

- **Instantiation**: Deploy new CNFs from Helm charts
- **Configuration**: Day-2 configuration changes
- **Scaling**: Horizontal and vertical scaling with HPA
- **Healing**: Self-healing and operator-driven remediation
- **Upgrades**: Rolling, blue-green, and canary deployments
- **Rollback**: Quick recovery to previous versions

Example use cases:
- "Upgrade the CNF to the latest version with zero downtime"
- "Configure auto-scaling based on traffic"
- "Rollback to the last known good version"

### Network Service Assurance Agent

The Service Assurance Agent provides telecom-grade service monitoring:

- **KPI Monitoring**: Latency, throughput, success rates
- **KQI Tracking**: End-user experience metrics
- **SLA Management**: Compliance tracking and alerting
- **Fault Detection**: Alarm correlation and root cause analysis

Example use cases:
- "What is the current service availability?"
- "Are we meeting our SLA targets?"
- "Investigate the cause of increased latency"

### Network Slice Agent

The Network Slice Agent manages 5G network slicing:

- **Slice Types**: eMBB (SST 1), URLLC (SST 2), MIoT (SST 3)
- **S-NSSAI Configuration**: SST and SD management
- **Resource Isolation**: Guaranteed resources per slice
- **QoS Policies**: Slice-specific quality of service

Example use cases:
- "Create a new URLLC slice for industrial automation"
- "Check slice resource allocation"
- "Troubleshoot slice selection issues"

### Telco Edge Agent

The Telco Edge Agent manages edge computing infrastructure:

- **Edge Sites**: Far edge, near edge, regional edge
- **ETSI MEC**: MEC orchestrator, platform, applications
- **Multi-cluster**: Federation and cross-cluster networking
- **Workload Placement**: Latency-optimized deployment

Example use cases:
- "Deploy the video analytics app to the edge"
- "Check the health of edge clusters"
- "Optimize resource usage at edge sites"

## Development

To add a new agent:

1. Create a new directory under `helm/agents/`
2. Add `Chart-template.yaml`, `values.yaml`, and `templates/agent.yaml`
3. Update the Makefile `helm-agents` and `helm-publish` targets
4. Update this README with the new agent documentation

See existing agents for template examples.
