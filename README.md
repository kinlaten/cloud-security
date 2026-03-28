# General
## Build-time security
- Code & Image scanning .e.g. Trivy 
- Host mitigate attacks related to privilege escalation
- Use minimal base image .e.g. scratch, -alpine, distroless
- K8s network security: role-based access control (RBAC), label taxonomies, label governance, and admission controls
- K8s datastore, API server: strong credentials, public key infrastructure (PKI) for access, and transport layer security (TLS) for data in transit encryptio
- Security groups are more aligned with the Kubernetes architecture than traditional firewalls and security gateways; however, even security groups are not aware of the context for workloads running inside the cluster

## Deploy-time security 
As Kubernetes is based on a flat network, without any special meaning for pod IP addresses, very few of these traditional appliances are able to provide any meaningful workload-aware network security and instead have to treat the whole cluster as a single entity. It should be clear that Kubernetes requires a new approach to network security
- new workloads are allowed to talk to which other workload (not rely on special meanings of IP addresses or network topology)
- new manage network policies: policy recommendations, policy impact previews, and policy staging 
- new monitor and visualize network traffic: both cluster-scoped holistic view
- new intrusion detection and threat defense: policy violation alerting, network anomaly detection, and integrated threat feeds
- new remediation workflows
- new mechanisms for auditing configuration and policy changes for complianceand also Kubernetes-aware network flow logs to meet compliance requirements (since tra‐ ditional network flow logs are IP-based and have little long-term meaning in the context of Kubernetes).

## K8s Runtime Security

### Network Access Control

Observation → Intrusion detection

Encryption Data ← Scan data

Continuous Compliance: SOC 2, PCI, HIPAA, GDPR

### Observation (Pod → Svc) Mesh

Service-to-Service communication (svc <-> svc).

Overlay applications on that mesh network.

Metadata: Capture metadata of the nodes hosting those pods.

Metrics: Track pod metrics such as traffic, latency, and operations.

DNS Activity: Monitor DNS activity pointing to services.

Distributed Tracing: Trace user transactions across the system.

External Communication: Monitor when a service communicates with external entities.

### Machine Learning & Logging

K8s Activity Logs: Feed logs into Machine Learning models.

Baselining: Use ML to create baselines for individual metrics.

Detection: Use these baselines to detect abnormal behavior.

### Security Frameworks

MITRE ATT&CK Matrix: Use the Enterprise matrix or the specific Threat Matrix for Kubernetes.

# Host Security (Host Sec)

## 1. Choice of OS

Use Immutable systems.

Enable Auto-updates.

Ensure use of a New kernel.

## 2. Non-essential Processes

Remove via systemd or /etc/init.d/.

Note: By default, Immutable OS options are minimal and preferred.

## 3. Host Firewall

Use tools like iptables or firewalld.

Warning: Ensure these rules are compatible with K8s traffic.

Better Approach: Use network plugins like Cilium or Calico.

## 4. Benchmarks

Always check the latest CIS Benchmarks.

# Cluster Hardening
## 1. Datastore: etcd

Critical Risk: If etcd leaks, all nodes effectively leak root privileges.

PKI (x.509): Use x.509 based Public Key Infrastructure.

Keys & Certificates: Use a combination of keys and certificates:

    - Peer Set: One set for communication between etcd instances.

    - Client Set: One set for client credentials (e.g., for the API server).

CA Configuration: Configure a Certificate Authority (CA) to generate client credentials.

Network Security: Use network firewall rules so that only the control plane nodes can access the etcd instances.

## 2. API server

## 3. Encrypt k8s secrets at rest

By default, it is off
When turn on, it only applied for new appended secret -> need rewrite all sets

Use AES-CBC with PKCS: 32 byte encryption

Host of etcd

    - Local: if api server compromise, etcd store compromise

    - External KMS: use DEK <- KEK which provided by KMS as central root of trust. DEK is stored along encrypted etcd, while KEK never stored on cluster

## 4. Rotate crendentials frequently 

Build auto rotate credentials worker

Note: no need rewrite all secret when rotate KEK on KMS

## 5. Authen / RBAC

Principle: least privilege access

Use groups and roles instead of grant to individual

Use cloud IAM / on-prem Auth service. Avoid K8s basic auth / svc account token which is bad for rotating secrets

## 6. Cloud metadata API

VM instance/pod by default have access to cloud metadata API. Limit access this by:

- Use pod IAM credentials

- Limit cloud privilege of each instance

- use network policy block pod access to the API

## Audit

Use policy determine which events are recorded for which resources and level of detail

Depend on level of security, may essential to or not to log detail of secrets

[More detail](https://kubegrade.com/kubernetes-auditing/)


## 8. Disable alpha/beta feature

## 9. Update K8s

## 10. Use bench tools

- Kube-bench
