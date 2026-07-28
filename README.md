# DevOps & Cloud Interview Questions

A curated set of DevOps, Kubernetes, Docker, AWS, and Cloud Security interview questions with concise, interview-ready answers.

> A formatted PDF version is available: `Interview-Questions.pdf`

## Table of Contents

- [Q1. What is Kubernetes and why do we use it?](#q1-what-is-kubernetes-and-why-do-we-use-it)
- [Q2. What problems does Kubernetes solve?](#q2-what-problems-does-kubernetes-solve)
- [Q3. Why Kubernetes instead of just Docker / Docker Compose?](#q3-why-kubernetes-instead-of-just-docker--docker-compose)
- [Q4. Key benefits of Kubernetes](#q4-key-benefits-of-kubernetes)
- [Q5. Real-world scenario: Why adopt Kubernetes?](#q5-real-world-scenario-why-adopt-kubernetes)
- [Q6. Difference between service.yml and deployment.yml](#q6-difference-between-serviceyml-and-deploymentyml)
- [Q7. What is Ansible?](#q7-what-is-ansible)
- [Q8. Continuous feedback loop from Operations to Development](#q8-continuous-feedback-loop-from-operations-to-development)
- [Q9. What is a NACL in AWS security?](#q9-what-is-a-nacl-in-aws-security)
- [Q10. Different types of volumes in AWS](#q10-different-types-of-volumes-in-aws)
- [Q11. Securing a cloud application using a cloud security framework](#q11-securing-a-cloud-application-using-a-cloud-security-framework)
- [Q12. Server-side vs client-side encryption](#q12-server-side-vs-client-side-encryption)
- [Q13. What is connection draining (deregistration delay)?](#q13-what-is-connection-draining-deregistration-delay)
- [Q14. What is Docker?](#q14-what-is-docker)
- [Q15. What is Image Pull Policy in Kubernetes?](#q15-what-is-image-pull-policy-in-kubernetes)
- [Q16. What is a Service in Kubernetes and its types?](#q16-what-is-a-service-in-kubernetes-and-its-types)

---

## Q1. What is Kubernetes and why do we use it?

Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Running containers manually at scale is hard; Kubernetes solves this by providing:

- Automated deployment and rollout/rollback of applications
- Self-healing (restarts failed containers, reschedules on healthy nodes)
- Horizontal scaling based on CPU/memory or custom metrics
- Service discovery and built-in load balancing
- Declarative configuration (you describe the desired state; K8s maintains it)

## Q2. What problems does Kubernetes solve?

Before Kubernetes, teams struggled with manual scaling, downtime during deployments, inconsistent environments, and complex networking. Kubernetes addresses these with:

- High availability: no single point of failure for your workloads
- Zero-downtime deployments via rolling updates
- Efficient resource utilization through bin-packing of containers
- Portability across on-prem, cloud, and hybrid environments
- Consistent, reproducible environments from dev to production

## Q3. Why Kubernetes instead of just Docker / Docker Compose?

Docker packages and runs a single container; Docker Compose runs a few containers on one host. Neither manages containers across many machines in production. Kubernetes orchestrates containers across a cluster of nodes, handling failover, auto-scaling, secrets management, and rolling updates — features essential for production-grade systems.

## Q4. Key benefits of Kubernetes

- **Scalability:** scale apps up/down automatically with demand
- **Self-healing:** auto-restart, reschedule, and replace containers
- **Portability:** run the same workloads anywhere (AWS, Azure, GCP, on-prem)
- **Declarative management:** version-controlled, GitOps-friendly config
- **Extensibility:** CRDs, operators, and a huge ecosystem
- **Cost efficiency:** better hardware utilization reduces infra cost

## Q5. Real-world scenario: Why adopt Kubernetes?

> "We moved to Kubernetes to handle unpredictable traffic spikes. Horizontal Pod Autoscaling let us scale automatically, rolling updates gave us zero-downtime releases, and self-healing reduced on-call incidents. It also standardized our deployments across dev, staging, and production, which cut environment-related bugs significantly."

## Q6. Difference between service.yml and deployment.yml

Both are Kubernetes manifests but define different objects.

| | **deployment.yml** (kind: Deployment) | **service.yml** (kind: Service) |
|---|---|---|
| **Purpose** | Runs your app (manages pods) | Exposes your app (manages networking) |
| **Controls** | Replicas, image/version, rolling updates, self-healing | Stable IP/DNS, load balancing, service type |
| **Type field** | Deployment strategy | ClusterIP / NodePort / LoadBalancer |

**In short:** a Deployment *runs* your app (manages pods); a Service *exposes* your app (manages networking). They work together via matching labels/selectors.

## Q7. What is Ansible?

Ansible is an open-source IT automation tool for configuration management, application deployment, and orchestration. It automates repetitive tasks across many machines from a single control point.

**Key characteristics:**

- **Agentless:** connects over SSH (Linux) or WinRM (Windows), no agent needed
- **Declarative:** you describe the desired state and Ansible enforces it
- **Idempotent:** re-running makes no changes if already in the desired state
- **Push-based:** the control node pushes config to managed nodes
- **YAML-based:** automation written in easy-to-read playbooks

**Core concepts:** Control Node, Managed Nodes, Inventory, Modules, Playbooks, Roles, Facts.

## Q8. Continuous feedback loop from Operations to Development

The feedback loop closes the gap between running software (Ops) and the people building it (Dev) — the **Monitor → Plan** part of the DevOps infinity loop.

**How feedback flows (Ops → Dev):**

- **Monitoring & Observability:** metrics, logs, traces (Prometheus, Grafana, ELK, Jaeger)
- **Alerting:** thresholds/anomalies trigger alerts (Alertmanager, PagerDuty)
- **Incident & error tracking:** exceptions with stack traces (Sentry); blameless post-mortems
- **User & business feedback:** real user monitoring, tickets, usage analytics, A/B tests
- **Feeding back into dev:** bugs become tickets in Jira/GitHub Issues, prioritized next sprint

**Makes it continuous:** CI/CD pipelines, automated monitoring/alerting, feature flags, canary/blue-green deploys, shift-left testing.

## Q9. What is a NACL in AWS security?

A NACL (Network Access Control List) is a **stateless** firewall that controls inbound/outbound traffic at the **subnet** level inside a VPC.

- Operates at subnet level (affects all resources in the subnet)
- **Stateless:** return traffic is not auto-allowed — needs explicit inbound & outbound rules
- Supports both **ALLOW and DENY** rules
- Rules evaluated in order by number (lowest first); first match wins

**NACL vs Security Group:**

| Aspect | NACL | Security Group |
|---|---|---|
| Level | Subnet | Instance/ENI |
| State | Stateless | Stateful |
| Rules | Allow + Deny | Allow only |

## Q10. Different types of volumes in AWS

AWS block storage is provided by **Amazon EBS**, in two families:

**SSD-backed (IOPS-intensive):**

- **gp3** — General Purpose SSD, cost-effective, independently provisioned IOPS/throughput
- **gp2** — older General Purpose SSD, IOPS scale with size (3 IOPS/GB)
- **io2 / io2 Block Express** — highest performance & durability for critical DBs
- **io1** — Provisioned IOPS SSD for I/O-intensive workloads

**HDD-backed (throughput-intensive):**

- **st1** — Throughput Optimized HDD (big data, logs, streaming)
- **sc1** — Cold HDD (lowest cost, infrequent access)

> Boot volumes must be SSD-backed (st1/sc1 cannot be boot volumes).
> Related storage: EBS (block), S3 (object), EFS (file/NFS), Instance Store (ephemeral).

## Q11. Securing a cloud application using a cloud security framework

Apply layered controls across identity, network, data, and workloads, guided by a recognized framework.

**Frameworks:** AWS Well-Architected (Security Pillar), CIS Benchmarks, NIST CSF, ISO/IEC 27001, CSA CCM.

**Key controls:**

- **IAM:** least privilege, MFA, no long-lived root credentials
- **Network:** VPCs, Security Groups, NACLs, private subnets, WAF
- **Data:** encrypt at rest (KMS) and in transit (TLS); use a secrets vault
- **Detection:** CloudTrail, GuardDuty, Security Hub, Config
- **Workload/app:** scan images/dependencies, patch, OWASP Top 10, runtime protection
- **Automation:** IaC scanning & policy-as-code (Checkov, tfsec) in CI/CD

**Principles:** defense in depth, zero trust, shared responsibility model, continuous compliance.

## Q12. Server-side vs client-side encryption

Both protect data at rest; they differ in *where* encryption happens and *who* controls the keys.

| Aspect | Server-Side (SSE) | Client-Side (CSE) |
|---|---|---|
| Encryption location | On the server (after upload) | On the client (before upload) |
| Who encrypts | Cloud provider / service | Your application |
| Key control | Provider (or shared) | Fully you |
| Provider sees plaintext? | Yes (briefly) | No — only ciphertext |
| Ease of use | Simple, transparent | More complex |

**AWS S3 SSE options:** SSE-S3 (AWS keys), SSE-KMS (KMS keys, auditable), SSE-C (customer-provided key).

> **Analogy:** SSE = the post office locks your letter in a safe; CSE = you lock it in your own box before handing it over.

## Q13. What is connection draining (deregistration delay)?

Connection draining (called **deregistration delay** in AWS ALB/NLB) lets a load balancer finish serving in-flight requests to a backend instance before removing it, instead of cutting connections abruptly.

**How it works:**

1. Instance is marked for removal (scale-in, deployment, failed health check)
2. Load balancer stops sending **new** requests to it
3. Existing in-flight requests complete, up to a configurable timeout
4. Instance is fully removed once connections finish (or timeout expires)

> Default timeout: 300s (5 min); configurable 1–3600s. Enables zero-downtime deployments.

## Q14. What is Docker?

Docker is an open-source containerization platform that packages an application with all its dependencies into a lightweight, portable **container** that runs the same in any environment — solving the "it works on my machine" problem.

**Key concepts:** Dockerfile (build instructions), Image (read-only template), Container (running instance), Registry/Docker Hub (image storage), Docker Engine (runtime).

**Docker vs VM:** containers share the host OS kernel (lightweight, seconds to start); VMs run a full guest OS each (heavy, slower).

## Q15. What is Image Pull Policy in Kubernetes?

A container-level setting telling the kubelet **when** to pull the image from the registry.

- **Always** — pull every time a pod starts (ensures latest)
- **IfNotPresent** — pull only if the image isn't already on the node
- **Never** — never pull; use only a local image (fails if missing)

**Defaults by tag:** `:latest` or no tag → `Always`; specific tag (e.g., `:1.2.3`) → `IfNotPresent`.

> **Best practice:** use immutable tags (not `:latest`) with `IfNotPresent` for predictable deployments.

## Q16. What is a Service in Kubernetes and its types?

A Service provides a stable network endpoint (IP + DNS) to access a group of pods, load-balancing traffic via label selectors — since pod IPs change when recreated.

**Types:**

- **ClusterIP** (default) — internal-only cluster IP for pod-to-pod communication
- **NodePort** — exposes a static port on every node's IP for external access (`NodeIP:NodePort`)
- **LoadBalancer** — provisions an external cloud load balancer (AWS/Azure/GCP)
- **ExternalName** — maps the Service to an external DNS name (CNAME)

> NodePort builds on ClusterIP, and LoadBalancer builds on NodePort. For HTTP path/host routing, use an **Ingress** in front of a ClusterIP Service.

---

## How to regenerate the PDF

```powershell
python generate_k8s_pdf.py
```

This produces `Interview-Questions.pdf` from the questions defined in `generate_k8s_pdf.py`.
