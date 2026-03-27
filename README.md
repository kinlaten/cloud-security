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
