# DevOps Interview Q&A

**Kubernetes & Ansible & AWS & Cloud Security**

> These notes match the content of `Interview-Questions.pdf` exactly.

## Table of Contents

- [Q1. What is Kubernetes and why do we use it?](#q1-what-is-kubernetes-and-why-do-we-use-it)
- [Q2. What problems does Kubernetes solve?](#q2-what-problems-does-kubernetes-solve)
- [Q3. Why Kubernetes instead of just Docker / Docker Compose?](#q3-why-kubernetes-instead-of-just-docker--docker-compose)
- [Q4. What are the key benefits of Kubernetes? (Interview summary)](#q4-what-are-the-key-benefits-of-kubernetes-interview-summary)
- [Q5. Real-world scenario: Why did your team adopt Kubernetes?](#q5-real-world-scenario-why-did-your-team-adopt-kubernetes)
- [Q6. What is the difference between service.yml and deployment.yml?](#q6-what-is-the-difference-between-serviceyml-and-deploymentyml)
- [Q7. What is Ansible?](#q7-what-is-ansible)
- [Q8. How does the continuous feedback loop from Operations to Development work?](#q8-how-does-the-continuous-feedback-loop-from-operations-to-development-work)
- [Q9. What is a NACL in AWS security?](#q9-what-is-a-nacl-in-aws-security)
- [Q10. What are the different types of volumes in AWS?](#q10-what-are-the-different-types-of-volumes-in-aws)
- [Q11. How can we secure an application in the cloud using a cloud security framework?](#q11-how-can-we-secure-an-application-in-the-cloud-using-a-cloud-security-framework)
- [Q12. What is server-side encryption vs client-side encryption?](#q12-what-is-server-side-encryption-vs-client-side-encryption)
- [Q13. What is connection draining (deregistration delay)?](#q13-what-is-connection-draining-deregistration-delay)
- [Q14. What is Docker?](#q14-what-is-docker)
- [Q15. What is Image Pull Policy in Kubernetes?](#q15-what-is-image-pull-policy-in-kubernetes)
- [Q16. What is a Service in Kubernetes and what are its types?](#q16-what-is-a-service-in-kubernetes-and-what-are-its-types)
- [Q17. What is a DaemonSet in Kubernetes?](#q17-what-is-a-daemonset-in-kubernetes)

---

## Q1. What is Kubernetes and why do we use it?

Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

We use it because running containers manually at scale is hard. Kubernetes solves this by providing:

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

Docker packages and runs a single container; Docker Compose runs a few containers on one host. Neither manages containers across many machines in production.

Kubernetes goes further by orchestrating containers across a cluster of nodes, handling failover, auto-scaling, secrets management, and rolling updates - features essential for production-grade systems.

## Q4. What are the key benefits of Kubernetes? (Interview summary)

- Scalability: scale apps up/down automatically with demand
- Self-healing: auto-restart, reschedule, and replace containers
- Portability: run the same workloads anywhere (AWS, Azure, GCP, on-prem)
- Declarative management: version-controlled, GitOps-friendly config
- Extensibility: CRDs, operators, and a huge ecosystem
- Cost efficiency: better hardware utilization reduces infra cost

## Q5. Real-world scenario: Why did your team adopt Kubernetes?

A strong interview answer ties benefits to outcomes. Example:

> "We moved to Kubernetes to handle unpredictable traffic spikes. Horizontal Pod Autoscaling let us scale automatically, rolling updates gave us zero-downtime releases, and self-healing reduced on-call incidents. It also standardized our deployments across dev, staging, and production, which cut environment-related bugs significantly."

## Q6. What is the difference between service.yml and deployment.yml?

Both are Kubernetes manifest files, but they define different objects with completely different responsibilities.

**deployment.yml (kind: Deployment)** manages the application PODS - the actual running containers. It controls:

- How many replicas (pods) of your app should run
- Which container image and version to use
- Rolling updates, rollbacks, and self-healing of pods
- Resource requests/limits and pod configuration

**service.yml (kind: Service)** provides stable NETWORK access to those pods. Pods are ephemeral and get new IPs when recreated, so a Service gives them a fixed endpoint. It controls:

- A stable IP/DNS name to reach the pods
- Load balancing traffic across the matching pods
- Service type: ClusterIP, NodePort, or LoadBalancer
- Which pods to route to (via label selectors)

In short: a Deployment RUNS your app (manages pods), while a Service EXPOSES your app (manages networking). They work together - the Deployment creates pods, and the Service routes traffic to them using matching labels/selectors.

## Q7. What is Ansible?

Ansible is an open-source IT automation tool used for configuration management, application deployment, and orchestration. It automates repetitive tasks - installing software, configuring servers, deploying apps - across many machines from a single control point.

**Key characteristics:**

- Agentless: connects over SSH (Linux) or WinRM (Windows), no agent needed
- Declarative: you describe the desired state and Ansible enforces it
- Idempotent: re-running makes no changes if already in desired state
- Push-based: the control node pushes config to managed nodes
- YAML-based: automation written in easy-to-read playbooks

**Core concepts:**

- Control Node: machine where Ansible runs
- Managed Nodes: servers/devices Ansible configures
- Inventory: file listing managed hosts, grouped by role/environment
- Modules: reusable units of work (apt, copy, service, user, ...)
- Playbooks: YAML files defining tasks to run on hosts
- Roles: reusable, structured way to organize playbooks
- Facts: system info Ansible gathers automatically about hosts

Why use it: consistency across servers, scalability (1 or 1000 hosts), simplicity (no agents, human-readable YAML), and automation that eliminates manual work and human error.

## Q8. How does the continuous feedback loop from Operations to Development work?

In DevOps, the feedback loop closes the gap between running software (Ops) and the people building it (Dev). It ensures that what happens in production continuously informs and improves development. This is the Monitor -> Plan part of the DevOps infinity loop.

**How the feedback flows (Ops -> Dev):**

- Monitoring & Observability: metrics, logs, and traces collected from production (Prometheus, Grafana, ELK, Jaeger, OpenTelemetry)
- Alerting: thresholds/anomalies trigger alerts to on-call devs (Alertmanager, PagerDuty, Opsgenie)
- Incident management & error tracking: exceptions with stack traces pushed to developers (Sentry, Rollbar); blameless post-mortems
- User & business feedback: real user monitoring, support tickets, feature usage analytics, A/B test results
- Feeding back into development: bugs/incidents become tickets in Jira/GitHub Issues, prioritized in the next sprint

**What makes the loop continuous:**

- CI/CD pipelines: fixes flow back to production quickly and safely
- Automated monitoring/alerting: issues detected without manual checks
- Feature flags: instantly roll back or limit exposure
- Canary / Blue-Green deploys: feedback from a small subset first
- Shift-left testing: feedback caught earlier, before production

Example: a release raises the error rate from 0.1% to 5% -> Prometheus detects it -> Alertmanager pages the dev -> Sentry shows the exact exception -> team rolls back via feature flag -> a GitHub Issue is created and the fix ships through CI/CD, closing the loop.

Why it matters: faster recovery (lower MTTR), higher quality driven by real production data, data-driven decisions, and a shared 'you build it, you run it' ownership culture.

## Q9. What is a NACL in AWS security?

A NACL (Network Access Control List) is a stateless firewall that controls inbound and outbound traffic at the SUBNET level inside an Amazon VPC. It acts as an additional layer of defense, sitting in front of the resources in a subnet.

**Key characteristics:**

- Operates at the subnet level (affects all resources in the subnet)
- Stateless: return traffic is NOT automatically allowed - you must add explicit rules for both inbound and outbound
- Supports both ALLOW and DENY rules
- Rules are evaluated in order by rule number (lowest first); the first match wins
- Every subnet is associated with a NACL; the default NACL allows all traffic until you restrict it

**NACL vs Security Group (a very common interview follow-up):**

- Level: NACL works at the subnet level; Security Group works at the instance/ENI level
- State: NACL is stateless; Security Group is stateful (return traffic auto-allowed)
- Rules: NACL supports allow AND deny; Security Group supports allow only
- Evaluation: NACL processes rules in number order; Security Group evaluates all rules together

Why use it: NACLs provide subnet-wide guardrails - for example, explicitly blocking a malicious IP range or isolating a database subnet - complementing Security Groups for defense in depth.

## Q10. What are the different types of volumes in AWS?

In AWS, block storage volumes are provided by Amazon EBS (Elastic Block Store), attached to EC2 instances. EBS volume types fall into two families: SSD-backed (for IOPS-intensive/transactional workloads) and HDD-backed (for throughput-intensive workloads).

**SSD-backed volumes:**

- gp3 (General Purpose SSD): latest generation, cost-effective, baseline 3000 IOPS with independently provisioned IOPS/throughput
- gp2 (General Purpose SSD): older generation, IOPS scale with volume size (3 IOPS per GB)
- io2 / io2 Block Express (Provisioned IOPS SSD): highest performance and durability for mission-critical databases
- io1 (Provisioned IOPS SSD): high IOPS for I/O-intensive workloads

**HDD-backed volumes:**

- st1 (Throughput Optimized HDD): low-cost, high-throughput for big data, log processing, and streaming workloads
- sc1 (Cold HDD): lowest-cost HDD for infrequently accessed data

Note: The boot/root volume must be SSD-backed (gp2/gp3/io1/io2); HDD types (st1/sc1) cannot be used as boot volumes.

Related AWS storage (interview follow-up): EBS is block storage; S3 is object storage; EFS is a managed elastic NFS file system; and Instance Store is temporary (ephemeral) storage physically attached to the host that is lost when the instance stops.

## Q11. How can we secure an application in the cloud using a cloud security framework?

Securing a cloud application means applying layered controls across identity, network, data, and workloads, guided by a recognized cloud security framework. Frameworks provide a structured checklist of best practices so nothing critical is missed.

**Common cloud security frameworks:**

- AWS Well-Architected Framework (Security Pillar)
- CIS Benchmarks (hardening baselines for cloud services)
- NIST Cybersecurity Framework (Identify, Protect, Detect, Respond, Recover)
- ISO/IEC 27001 and CSA Cloud Controls Matrix (CCM)

**Key security controls to apply:**

- Identity & Access Management: enforce least privilege with IAM roles/policies, enable MFA, avoid long-lived root credentials
- Network security: use VPCs, subnets, Security Groups and NACLs, private subnets, and WAF to filter web traffic
- Data protection: encrypt data at rest (KMS) and in transit (TLS), manage secrets with a vault (AWS Secrets Manager)
- Detection & monitoring: enable logging and auditing (CloudTrail, GuardDuty, Security Hub, Config) for continuous visibility
- Workload/app security: scan images and dependencies, patch regularly, apply the OWASP Top 10, use runtime protection
- Automation & compliance: use IaC scanning and policy-as-code (e.g., Checkov, tfsec) to enforce guardrails in CI/CD

Guiding principles: adopt defense in depth (multiple layers), zero trust (never trust, always verify), the shared responsibility model (cloud provider secures the cloud, you secure what's in it), and continuous compliance rather than one-time checks.

## Q12. What is server-side encryption vs client-side encryption?

Both protect data at rest, but they differ in where the encryption and decryption happen and who controls the keys.

**Server-Side Encryption (SSE):** data is encrypted by the service/server AFTER it receives your data, and decrypted when you read it back.

- Where it happens: on the server, after upload
- Who manages keys: usually the cloud provider (with options for your own keys)
- Data in transit: sent as plaintext, protected separately by TLS/HTTPS
- Transparency: invisible to the app - easy to enable
- AWS S3 options: SSE-S3 (AWS-managed keys), SSE-KMS (KMS-managed, auditable), SSE-C (customer-provided key)

**Client-Side Encryption (CSE):** data is encrypted by the client/application BEFORE it is sent to the server, so the server only ever stores ciphertext and never sees the plaintext or keys.

- Where it happens: on the client, before upload
- Who manages keys: you - the provider never has them
- Data in transit: already encrypted before it leaves your machine
- Transparency: more work - the app handles encryption and key management

Comparison: with SSE the provider briefly sees plaintext and it is simple to use - best for general protection at rest. With CSE the provider only sees ciphertext and you fully control the keys - best for highly sensitive, zero-trust data, at the cost of more complexity.

Analogy: SSE is like handing a letter to the post office and they lock it in a safe; CSE is like locking the letter in your own box before handing it over, so they never see the contents.

## Q13. What is connection draining (deregistration delay)?

Connection draining - called deregistration delay in AWS Application and Network Load Balancers - lets a load balancer finish serving in-flight requests to a backend instance before removing it, instead of cutting connections abruptly.

Why it's needed: when an instance is removed, deregistered, or becomes unhealthy, any requests it is currently handling would be dropped if traffic stopped instantly. Connection draining lets those existing connections complete gracefully.

**How it works:**

- An instance is marked for removal (scale-in, deployment, or failed health check)
- The load balancer stops sending NEW requests to that instance
- Existing in-flight requests are allowed to complete, up to a configurable timeout
- Once all connections finish (or the timeout expires), the instance is fully removed

**Key details:**

- AWS term for ALB/NLB: deregistration delay; for Classic ELB: connection draining
- Default timeout: 300 seconds (5 minutes)
- Configurable range: 1 to 3600 seconds

Benefits: no dropped requests during scaling or deployments, better user experience, zero-downtime deployments and smooth rolling updates, and graceful removal of terminating or unhealthy instances.

## Q14. What is Docker?

Docker is an open-source containerization platform that lets you package an application together with all its dependencies, libraries, and configuration into a single lightweight, portable unit called a container. The container runs the same way on any environment - dev, test, or production - solving the classic 'it works on my machine' problem.

How it works: Docker uses OS-level virtualization. Containers share the host operating system's kernel but run in isolated user spaces, making them far lighter and faster to start than virtual machines.

**Key concepts:**

- Dockerfile: a text file with instructions to build an image
- Image: a read-only template (app + dependencies) used to create containers
- Container: a running instance of an image - isolated and portable
- Docker Hub / Registry: a repository to store and share images
- Docker Engine: the runtime that builds and runs containers

**Docker vs Virtual Machine (common follow-up):**

- Containers share the host OS kernel; VMs run a full guest OS each
- Containers are lightweight (MBs) and start in seconds; VMs are heavy (GBs) and slower to boot
- Containers give higher density and efficiency on the same hardware

Why use it: portability across environments, consistency, fast startup, efficient resource use, easy scaling, and a smooth fit with CI/CD and microservices architectures.

## Q15. What is Image Pull Policy in Kubernetes?

Image Pull Policy is a container-level setting in Kubernetes that tells the kubelet WHEN to pull (download) the container image from the registry - whether to always fetch a fresh copy or reuse the one already cached on the node.

**The three policies:**

- Always: pulls the image from the registry every time a pod starts, even if already on the node - ensures the latest image
- IfNotPresent: pulls only if the image is not already on the node; otherwise uses the local cached copy - saves bandwidth
- Never: never pulls; uses only a locally present image and fails if the image is not already on the node

**Default behavior (important for interviews):** Kubernetes chooses the default based on the image tag.

- Tag is :latest or no tag -> defaults to Always
- Specific tag (e.g., :1.2.3) -> defaults to IfNotPresent

**When to use each:**

- Always: dev/CI where the tag stays the same but content changes often
- IfNotPresent: production with immutable, versioned tags - faster startup and less bandwidth
- Never: air-gapped environments or pre-loaded images (e.g., local testing with minikube)

Best practice: use specific, immutable image tags (not :latest) with IfNotPresent. Relying on :latest with Always can cause unpredictable deployments because the running image may silently change.

## Q16. What is a Service in Kubernetes and what are its types?

A Service in Kubernetes is an abstraction that provides a stable, permanent network endpoint (IP and DNS name) to access a group of pods. Because pods are ephemeral and their IPs change when recreated, a Service gives clients a fixed address and load-balances traffic across the matching pods using label selectors.

**The four main Service types:**

- ClusterIP (default): exposes the Service on an internal cluster IP, reachable only from WITHIN the cluster - used for internal pod-to-pod communication
- NodePort: exposes the Service on a static port on every node's IP, making it reachable from OUTSIDE the cluster via NodeIP:NodePort
- LoadBalancer: provisions an external cloud load balancer (AWS ELB, Azure LB, GCP LB) that routes external traffic to the Service - the standard way to expose apps in the cloud
- ExternalName: maps the Service to an external DNS name (a CNAME) instead of pods - used to access external services from inside the cluster

How they relate: NodePort builds on ClusterIP, and LoadBalancer builds on NodePort - each type layers additional exposure on top of the previous one.

Why use it: stable networking, automatic load balancing, service discovery via DNS, and decoupling clients from the changing set of pod IPs. For HTTP routing with paths/hosts, an Ingress is often used in front of a ClusterIP Service.

## Q17. What is a DaemonSet in Kubernetes?

A DaemonSet is a Kubernetes workload object that ensures a copy of a specific pod runs on ALL (or a selected subset of) nodes in the cluster. As nodes are added, the DaemonSet automatically schedules a pod onto them; as nodes are removed, those pods are garbage collected.

It is typically used for node-level background services / agents that must run everywhere, such as:

- Log collectors (Fluentd, Filebeat) gathering logs from every node
- Monitoring agents (Prometheus Node Exporter, Datadog agent)
- Networking components (CNI plugins, kube-proxy)
- Storage daemons and security/scanning agents

**Key characteristics:**

- Runs exactly one pod per node (by default), not a set number of replicas
- Automatically adds pods to new nodes and removes them from deleted nodes
- Can target specific nodes using nodeSelector, node affinity, or tolerations
- Tolerations let DaemonSet pods run even on tainted nodes (e.g., control-plane)

**DaemonSet vs Deployment (common follow-up):** a Deployment runs a desired NUMBER of replicas placed anywhere the scheduler chooses, used for scalable applications. A DaemonSet runs ONE pod PER node, used for node-level agents. Use a Deployment for app workloads and a DaemonSet for per-node infrastructure services.

---
