# Kubernetes Certified Application Developer (CKAD)

## Preparation
<img src="https://www.cncf.io/wp-content/uploads/2021/09/kubernetes-ckad-color.svg" align="right" width="25%">

* For an experienced k8s user (i.e. developer, not admin/cloud engineer), CKAD is not too much of a challenge given an exam-oriented preparation.
* You can find everything in the k8s docs, so time is the only issue.
* Knowing your way around the docs and [being fast](README.md#train-for-speed) in the terminal and with YAML is still of the essence.
* For me as an experienced developer, 8 hours of focused preparation (on two days) was enough: 
  * In the morning of the day before the exam, I worked through [tools and technologies](#tools-and-technologies).
  * in the afternoon I took my killer.sh simulated exam 
  * Afterward and in the morning of the day of the exam, I learned from my mistakes and did the remaining exercises.
  * Took the exam after lunch and succeeded with almost 90% even though [I lost 15 minutes](README.md#be-ready-for-non-kubernetes-challenges) due to a technical issue 😠.

## Curated resources

* Interactive learning environments
    * Two tries for the same interactive killer.sh exam are included in your exam voucher. Use them, but use them wisely!
    * If you need more, create an account at [killercoda](https://killercoda.com/killer-shell-cka)
* Community resources
    * [Walid Shaari's Curated resources](https://github.com/walidshaari/Kubernetes-Certified-Administrator/blob/main/README-ckad.md)
    * If you're into reddit, there is an active subreddit [r/ckad/](https://www.reddit.com/r/ckad/) 
* Example questions
    * [techwithmohamed/CKAD-Certified-Kubernetes-Application-Developer](https://github.com/techwithmohamed/CKAD-Certified-Kubernetes-Application-Developer) has Kustomize examples

## Tools and technologies

* [Train for speed](README.md#train-for-speed)
* `docker build`, `docker save`, `docker load`
* Know how it works: Service ➡️ Ingress ➡️ Pod.  
  Be ready to debug it.
* Executing commands in pods:
  `command: [ 'sh', '-c', 'My long command' ]`
* Searching something in all pods `k get pod -o yaml | grep myContainerName`
* `CronJobs`
  * know how to find an example in the [docs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs).
  * Have a basic understanding of cron expressions.
* [native sidecars](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/): `init` containers with `restartPolicy: Always`
* RBAC: `Role`, `RoleBinding`, verbs, resources
* Create PVs, PVCs
* Mount PVCs, CMs, Secrets into the file system of pods  
  💡 node name in env of pode: [`fieldRef`](https://kubernetes.io/docs/tasks/inject-data-application/environment-variable-expose-pod-information/)
* securityContext Syntax
```yaml
    securityContext:
      runAsNonRoot: true
      runAsUser: 100000
      runAsGroup: 100000
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      seccompProfile:
        type: RuntimeDefault
      capabilities:
        drop:
          - ALL
```
* Pod Security Standards
  * Can be enabled by label on namespace
  * Have a rough overview of the different levels: `privileged`, `baseline`, `restricted`.
* Know Resource requests and limits.
  * Be able to find the syntax in [docs](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pod-level-resources/)
  * Resources can be assigned both on pod and container level.
  * `kubectl describe node` ➡️ overview of available resources on a node.
* NetPols
    * [Docs](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
    * [ahmetb/kubernetes-network-policy-recipes](https://github.com/ahmetb/kubernetes-network-policy-recipes)
    * [editor.networkpolicy.io](https://editor.networkpolicy.io/)
    * cidr: single IP address is `/32` 
    * matching namespace by label`kubernetes.io/metadata.name`
    * Allow only intra-ns traffic: `ingress.from:` `- podSelector: {}`
*  Kustomize
    * [Docs](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)
    * Dry run (output YAML): `kubectl kustomize .`
    * Apply: `kubectl apply -k .` 
* Admission Control
    * `ResourceQuota` vs [`LimitRange`](https://kubernetes.io/docs/concepts/policy/limit-range/): `quotas` set a resource budget for a namespace, `limits` limit resources of pods.
    * [`ValidatingAdmissionPolicy`](https://kubernetes.io/docs/reference/access-authn-authz/validating-admission-policy/) Write validating admissions in a declarative way using Common Expression Language (CEL) instead of developping and deploying your own validating admission webhook.
    * (`MutatingAdmissionPolicy` still beta in 1.34)
* Services
  * you can acces pods via subdomains using headless services (`clusterIP: None`):  
    `busybox-1.busybox-subdomain.default.svc.cluster.local`  
    In Pod:  
  ```yaml
  spec:  
    hostname: busybox-1  
    subdomain: busybox-subdomain # Must match the Service name
  ```
  * Similarly, you can access pods via IP
    Query Pod by IP `10-244-1-5.namespace.pod.cluster.local`  
    Notice the `pod` instead of the `svc`!