# Cloud Native Security Associate (KCSA)

## Preparation
<img alt="KCSA icon" src="https://www.cncf.io/wp-content/uploads/2024/03/kubernetes-kcsa-color.svg" align="right" width="25%">

* For an experienced k8s user (i.e. developer, not admin/cloud engineer), KCSA is definitely more of a challenge than KCNA (which is none) but still much easier than CKAD.
* Other than with CK* exams, time is absolutely not an issue. I took my time and finished in less than an hour of the 1,5h exam time.
* I re-acquainted myself with the advice my past self [gave on CKS](CKS.md) and worked through the 300 example questions of [kubernetes-security-kcsa-mock](https://github.com/thiago4go/kubernetes-security-kcsa-mock) and followed up on topics I was unsure about.
* This totaled to about 4 hours, which in retrospect was a rather minimalist approach for an experienced developer (not cluster-admin) and security enthusiast (not expert).
* I was unsure about 20/60 questions, but in the end passed with 91%

## Curated resources

* Open Source interactive mock exam environment with 300 questions 🤯  
  [thiago4go/kubernetes-security-kcsa-mock](https://github.com/thiago4go/kubernetes-security-kcsa-mock)
* [Another mock exam env](https://certificationpractice.com/practice-exams/kubernetes-and-cloud-native-security-associate-kcsa) with 60 questions in 90 mins (requires email address after 20 questions, though 🤨). 

## Tools and technologies

* `strace -p 1234`
* `kubectl get componentstatuses`
* `kubectl certificate approve`
* [Trust Boundaries](https://notes.kodekloud.com/docs/Kubernetes-and-Cloud-Native-Security-Associate-KCSA/Kubernetes-Threat-Model/Kubernetes-Trust-Boundaries-and-Data-Flow)
  * Boundaries
    * Cluster
    * Node
    * Namespace
    * Pod
  * example actions in Kubernetes cross trust boundaries
    * Accessing the Kubernetes API server
    * Mounting hostPath volumes into pods
    * Using service accounts across different namespaces
    * Pulling container images from public registries
    * Probably also: Communicating between pods within the same namespace
* [Compliance Frameworks](https://notes.kodekloud.com/docs/Kubernetes-and-Cloud-Native-Security-Associate-KCSA/Compliance-and-Security-Frameworks/Compliance-Frameworks) 
  * GDPR, 
  * HIPAA (ensure confidentiality, integrity, etc of protected health info (PHI)),
  * PCI DSS (Payment Card Industry Data Security Standard): A set of security standards for systems handling cardholder data, applicable to container-based workflows
  * NIST Cybersecurity Framework (CSF)
    * five core functions: Identify, Protect, Detect, Respond, and Recover.
    * A set of risk management guidelines to identify, protect, detect, respond to, and recover from cyber threats
    * NIST SP 800-53 Rev. 5:  provides comprehensive guidelines on security and privacy controls specifically for federal information
  * NSA/CISA Kubernetes Hardening: Securing supply chain components and minimizing container privileges
    CIS Controls provide a prioritized set of cybersecurity best practices to defend against common cyber attacks. e.g.
    * Don't use namespaces `kube-system` and `kube-public`.
  * CIS Benchmarks
  * FedRAMP:
    * compliance framework is mandatory for U.S. federal agencies
    * primarily recognized for emphasizing continuous policy review and network traffic monitoring
* Botkube: primary purpose is to automate Slack-based alerts for compliance and security events (and other cluster activities).
* Threat Modeling:
  * DREAD vs STRIDE
    * DREAD (evaluate and prioritize severity of potential sec threats)  
    * STRIDE (identify and categorize potential sec threats)  
  * STRIDE: To effectively mitigate the 'Elevation of Privilege' threat category from the STRIDE model within a Kubernetes cluster, which of the following security controls should be implemented?
    * Implementing strict Role-Based Access Control (RBAC) with least privilege
    * Configuring 'allowPrivilegeEscalation: false' in the Pod Security Context
    * Meaning
      * **Spoofing**: Pretending to be someone or something else. (Violates **Authentication**)
      * **Tampering**: Unauthorized modification of data or code. (Violates **Integrity**)
      * **Repudiation**: Claiming you didn't perform an action due to a lack of evidence. (Violates **Non-repudiation**)
      * **Information Disclosure**: Leaking private data to unauthorized users. (Violates **Confidentiality**)
      * **Denial of Service**: Crashing or exhausting resources to block legitimate users. (Violates **Availability**)
      * **Elevation of Privilege**: Gaining more access/permissions than intended. (Violates **Authorization**)
  * DREAD model to score risks.
    * **Damage Potential**: How high is the impact (e.g., data loss, system downtime)?
    * **Reproducibility**: How easy is it for an attacker to repeat the attack?
    * **Exploitability**: What is the level of effort or skill required to perform the attack?
    * **Affected Users**: Roughly how many people or systems would be impacted?
    * **Discoverability**: How easy is it for an attacker to find the vulnerability?
  * ATT&CK: A knowledge base of adversary tactics and techniques used in cyberattacks.
  * Data Flow Diagram (DFD): Entry points, exit points, data flows, and potential threat vectors within the system
  * Microsoft Security Development Lifecycle (SDL)
    * Security development framework incorporates threat modeling:
    * incorporate threat modeling and mitigation throughout the software development process.
* Static Pods: In a Kubernetes environment, any YAML manifest placed directly into the /etc/kubernetes/manifests directory on a worker node is automatically detected and managed by the Kubelet service running on that specific node, rather than the Kubernetes API server.
* etcd
  * To minimize the attack surface of the control plane's datastore, a platform engineer is reviewing the startup arguments for the etcd service. Which configurations should be applied to restrict network exposure and strictly validate client connections?
    * Configure --client-cert-auth=true to enforce mutual TLS for all incoming connections
    * Set --listen-client-urls to the specific private interface IP or localhost only
* Admission Control  
  <img alt="Admission Controller Phases" src="https://kubernetes.io/images/blog/2019-03-21-a-guide-to-kubernetes-admission-controllers/admission-controller-phases.png" width="50%"/> 
* Certs
  * Server vs client cert
  * `0=system:masters` client cert brings you into `system:masters` group, which is bound to the "cluster-admin" superuser role by the default binding [docs](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
* scheduler
  * Sec best practices
    * Use Role-Based Access Control (RBAC) to restrict access to the scheduler API
    * Run the scheduler as a non-root user to minimize privilege escalation risks
    * Configure network policies to restrict network access to and from the scheduler
* controller-manager:
  * kube-controller-manager is exposing internal system performance details. Which configuration update resolves this information disclosure vulnerability?  
    --profiling=false
* apparmor:
  * list all profiles: `sudo apparmor_status`, `sudo aa-status`
* API Server
  * Rate Limiting: `APIPriorityAndFairness` (often implemented via FlowSchema and PriorityLevelConfiguration objects).
  * [Authentication](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#external-integrations): native support for JWT and for OpenID; other protocols (LDAP, SAML, Kerberos, alternate X.509 schemes) can be integrated via authentication proxy or webhook. 
* kubelet
  * [Authentication](https://kubernetes.io/docs/reference/access-authn-authz/kubelet-authn-authz/): disable anonymous access  `--anonymous-auth=false`
  * Authorization: The correct method to restrict access to the kubelet API securely is to configure the kubelet with the `--authorization-mode=Webhook` and `--authentication-token-webhook=true` flags.
  * Which Admission Controller should be enabled to strictly limit the Kubelet so it can only modify Pods bound to its own node?  
    To prevent a compromised node from interfering with Pods on other nodes, you should enable the Node Admission Controller.  
    Specifically, this is achieved by using the `NodeRestriction` admission plugin in conjunction with the Node Authorizer.
* More terms
  * data plane = worker nodes
  * cloud metadata APIs: [limit access via netpols](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/#restricting-cloud-metadata-api-access) to avoid lateral movement.
  * Data Exfiltration
  * vulnerability scanning, triage
  * provenance
  * Pipeline attestation
* [Audit](https://notes.kodekloud.com/docs/Certified-Kubernetes-Security-Specialist-CKS/Monitoring-Logging-and-Runtime-Security/Use-Audit-Logs-to-monitor-access)
  * Which of the following are valid stages defined in the Kubernetes audit logging lifecycle that indicate a distinct point in the request processing timeline? (Select Two)
    * ResponseComplete
    * RequestReceived
    * (Panic)
  * Which audit policy rule for `secrets` satisfies this requirement while still maintaining an audit trail of the access?
    Set the `level` to `Request` for resources matching `secrets`
    Set the `omitStages` list to include `ResponseComplete` for `secrets`
    Set the `level` to `None` for resources matching `secrets`
    **Set the `level` to `Metadata` for resources matching `secrets`**
  * Which of the following steps are necessary to enable audit logging in Kubernetes?
    * Specify '--audit-log-path' in the API server configuration
    * Add '--audit-policy-file' flag to API server
    * Create an audit policy file
  * valid components: Rules, Levels, Stages
  * How do you configure a Kubernetes audit policy rule to log events for all resources within a specific namespace?
    `Use 'namespaces: ["<namespace>"]' under the rule's 'namespaces' field`
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  - level: Request
    namespaces: ["production"] # This filters for the specific namespace
    resources:
      - group: "" # Core API group
        resources: ["pods", "configmaps", "secrets"]
```
| Level | What it logs | Impact on Secrets |
| :--- | :--- | :--- |
| **None** | Nothing. | No audit trail; fails the requirement. |
| **Metadata** | Request metadata (user, timestamp, resource, verb) but **not** the request/response body. | **Ideal.** Shows who accessed the secret without logging the sensitive data itself. |
| **Request** | Metadata plus the request body. | **Dangerous.** This would log the sensitive data being sent to the API. |
| **RequestResponse** | Metadata plus both request and response bodies. | **Very Dangerous.** Logs the secret data in both directions. |
