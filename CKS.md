# Certified Kubernetes Security Specialist (CKS)

## Preparation
<img alt="CKS icon" src="https://www.cncf.io/wp-content/uploads/2020/11/kubernetes-security-specialist-logo.svg" align="right" width="25%">

* I invested about 5 working days over a period of three weeks before the exam.  
* As a seasoned developer in the public cloud, I was quick with `kubectl`, security-context and so on, but not so much with api-server config.
* You can find many things in the docs, so time is one of the main issues.  
* Another issue is knowing the CLI commands of tools like `kubeadm`, `crictl`, `etcdctl`, `systemctl`, `apt`, `dpkg`, `openssl`, `dmesg`, `sha512sum`, `conftest`, `strace`, `apparmor_parser`.
* Retrospectively, spending maybe one more day would not have been advisable, as I had to look up a lot of api-server-related stuff in the docs during the exam.
* Also it is efficient to pass CKA and CKS in close succession because CKA-knowledge is also important for CKS.

## Curated resources

* Interactive learning environments
    * Two tries for **two different** interactive killer.sh are included in your exam voucher. Use them, but use them wisely!
    * If you need more, create an account at [killercoda](https://killercoda.com/killer-shell-cka)
    * [ViktorUJ/cks](https://github.com/ViktorUJ/cks/tree/master/tasks/cks/labs) - local platform for learning kubernetes and preparation for CKS
* Community resources
    * [Walid Shaari's Curated resources](https://github.com/walidshaari/Certified-Kubernetes-Security-Specialist)
    * [Video Tutorials by Venkata Ramana Gali](https://www.youtube.com/watch?v=jvmShTBSBoA&list=PLFkEchqXDZx6Bw3B2NRVc499j1TavjOvm),
      corresponding [Repo](https://github.com/ramanagali/Interview_Guide/blob/main/CKS_Preparation_Guide.md)
* Example questions
    * [Mohamed Abukar's mock exam questions](https://github.com/moabukar/CKS-Exercises-Certified-Kubernetes-Security-Specialist/tree/main/7-mock-exam-questions)
    * These look promising, including a learning platform: [ViktorUJ/cks](https://github.com/ViktorUJ/cks/tree/master/tasks/cks/labs)
    * [These](https://github.com/snigdhasambitak/cks) look like old killer.sh Questions with solutions, but no harm in having a look at them before having your first killer.sh try.
* [Udemy Courses](https://www.udemy.com/topic/certified-kubernetes-security-specialist-cks/)  
  even when you don't take a course, having a look at the content might help discover topics that are relevant
* There are a lot more when you follow the links in the repos above.

## Tools and technologies

Also look as [CKAD](CKAD.md#tools-and-technologies) and [CKA](CKA.md#tools-and-technologies).

* curl
  * Validate certificate: `curl -vk 2>&1  https://xyz | grep -i subject`
  * [Accessing the Kubernetes API from a Pod](https://kubernetes.io/docs/tasks/run-application/access-api-from-pod/)
    Note the path to the secrets
```bash
k run debug-node --rm -it --image alpine --
'curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api/v1/namespaces/default/secrets'
```
* `kube-apiserver`, `kubelet`, etc.
  * `/etc/kubernetes/manifests`
  * `/etc/kubernetes/manifests/kube-apiserver.yaml`
  * `/etc/kubernetes/manifests/kube-controller-manager.yaml`
  * `/var/lib/kubelet/config.yaml`
  * apiserver is automatically restarted when `kube-apiserver.yaml` is changed.  
    If need be this can be forced using `crictl rmp`, or even using `kubectl` (`k delete pod`) on a control plane node or by restarting the service
```shell
# systemd
systemctl status kubelet
systemctl restart kubelet
# or legacy wrapper script
service kubelet restart
service kubelet status
```
* [AppArmor](https://kubernetes.io/docs/tutorials/security/apparmor/)
  * AppArmor profiles are specified _per-container_. To specify the AppArmor profile to run a Pod container with, add an annotation to the Pod's metadata
  * `container.apparmor.security.beta.kubernetes.io/<container_name>: <profile_ref>`
  * `container.apparmor.security.beta.kubernetes.io/c1: localhost/very-secure`
  * or `runtime/default`
  * Profile must be present on node
  * Accessing [App Armor Docs](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation) is permitted
  * See [k8s docs](https://kubernetes.io/docs/tutorials/security/apparmor/) for an example
```shell
apparmor_parser -q <<EOF
EOF
# or
scp profile.txt node:/tmp/profile.txt
#then
apparmor_parser -q profile.txt
# optional check
apparmor_status
# or just load it again
apparmor_parser -q profile.txt # Output: profile already exists
```
* [Seccomp](https://kubernetes.io/docs/tutorials/security/seccomp/)
  * Kubernetes lets you automatically apply seccomp profiles loaded onto a node to your Pods and containers.
  * [Enable the use of `RuntimeDefault` as the default seccomp profile for all workloads](https://kubernetes.io/docs/tutorials/security/seccomp/#enable-the-use-of-runtimedefault-as-the-default-seccomp-profile-for-all-workloads)
    run the kubelet with the `--seccomp-default` [command line flag](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet)
    in `/var/lib/kubelet/config.yaml` as in [this example](https://github.com/moabukar/CKS-Exercises-Certified-Kubernetes-Security-Specialist/blob/main/7-mock-exam-questions/Q5-Seccomp.md#2---change-the-seccomp-profile-by-adding-the-below-argument-in-the-kubelet-config-file) then `systemctl restart kubelet`
  * Set default profile or an explicit one as shown in docs
```yaml
  securityContext:
    seccompProfile:
      type: RuntimeDefault
```
or
```yaml
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
```
* [PSA](https://kubernetes.io/docs/concepts/security/pod-security-admission/)
  * PSAs are set namespace-wide
  * Try out e.g. this example: [Apply predefined Pod-level security policies using PodSecurity  |  Google Kubernetes Engine (GKE)  |  Google Cloud](https://cloud.google.com/kubernetes-engine/docs/how-to/podsecurityadmission)
  * `k label --overwrite ns restricted-ns pod-security.kubernetes.io/enforce=restricted`
  * `k label --overwrite ns baseline-ns pod-security.kubernetes.io/warn=baseline`
* Falco
  * Might log to syslog ➡️ `cat /var/log/syslog | grep -i xyz` or `journalctl -fu falco | grep "xyz error"`
  * Output UID and Container ID: `%user.uid,%container.id` see [supported fields doc](https://falco.org/docs/reference/rules/supported-fields/) (allowed in exam!)
```yaml
rules_file:
  - /etc/falco/falco_rules.yaml
  - /etc/falco/falco_rules.local.yaml
  - /etc/falco/rules.d # custom rules
```
* Tracee
  similar to Falco, eBPF kernel events, signatures trigger alerts.
* Trivy
  * `trivy image alpine --severity=HIGH,CRITICAL`
* Kubesec
  * `kubesec scan k8s-deployment.yaml | jq`
  * `kubesec scan jenkins/tmp-docker-gid-grepper.yaml | jq '.[].scoring.advise'`
```yaml
apiVersion: v1
kind: Pod
spec:
  enableServiceLinks: false
  automountServiceAccountToken: false
  containers:
  - name: restricted
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
*  Kube-bench
  * Has to run on node! Either operator, pod, or binary on node
  * `kube-bench run --targets=master > bench.txt`
  * `kube-bench run --targets=node > bench.txt`
  * Using `--target` makes solving specific tasks much easier because there usually are a lot of results
  * Writing to file allows for comparing the results after mitigation.
  * `kube-bench run --targets=master | less` also works
* `crictl`, similar to `docker` but for CRI
```shell
crictl pods
crictl ps

crictl ps --pod $podid # all containers of pod, also works with grep $podid
crictl logs $containerid # see output of crictl ps --pod for $containerid
crictl rmp $podid # remove pod
```
* `etcdctl`: Access secrets directly: (command can be found in docs for [encryption at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)):
```shell
  ETCDCTL_API=3 etcdctl \
   --cacert=/etc/kubernetes/pki/etcd/ca.crt   \
   --cert=/etc/kubernetes/pki/etcd/server.crt \
   --key=/etc/kubernetes/pki/etcd/server.key  \
   get /registry/secrets/default/secret1 | hexdump -C
```
* `strace`
  Count time, calls, and errors for each system call and report a summary on program exit:
  `strace -p pid -c`
  or ongoing output, e.g. in separate tmux pane/window `strace -p pid 2>&1 | grep -i kill`
  e.g. syscalls of container
```shell
crictl pods # -> POD_ID
crictl ps --pod POD_ID # -> CONTAINER_ID
crictl inspect CONTAINER_ID | grep -i pid # -> PID
strace -p PID -c
# Ctrl + C 
#% time     seconds  usecs/call     calls    errors syscall
#------ ----------- ----------- --------- --------- ---------------
# 81,08    0,000030          30         1         1 rt_sigtimedwait
# 18,92    0,000007           3         2           wait4
#------ ----------- ----------- --------- --------- ----------------
#100,00    0,000037          12         3         1 total
```
* OPA [conftest](https://www.conftest.dev/)
  * `conftest test some.yaml`
  * looks for policies in folder `policy`
* OPA [Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/)
  *  OPA = general purpose policy engine; Gatekeeper = kubernetes-specific impl of OPA as K8s Admission controller
  * `ConstraintTemplate` ➡️ describe rego
  * `Constraint` ➡️ describes matching rules, parameters, etc., lists violations
    * Violations of constraints are listed in the `status` field of the corresponding constraint
    * The audit pod emits JSON-formatted audit logs to stdout.
    * Prometheus Metrics (not relevant for CKS)
  * [example](https://open-policy-agent.github.io/gatekeeper/website/docs/howto):
```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-must-have-gk
spec:
  enforcementAction: dryrun # deny|warn
```

```sh
# from killer.sh
## Question 7
k get crd | grep gatekeeper
k edit blacklistimages pod-trusted-images # constraint: add parameter
k edit constrainttemplates blacklistimages #-> template: change rego expression

## Preview Question 2
# constraint, add parameter for namespace
k edit requiredlabels namespace-mandatory-labels
# template, fix rego
k edit constrainttemplates requiredlabels
``` 
* `dmesg` - prints kernel logs
* `kubeadm`
```shell
# control plane
k drain $NODE
# ssh to master
apt update && apt install kubeadm=1.29.0-1.1 # if necessary
kubeadm upgrade plan # outputs commands for apply
kubeadm upgrade apply v1.24.1 # use specific version! Outputs message to upgrade kubelet
apt install kubelet=1.29.0-1.1 kubectl=1.29.0-1.1
service kubelet restart # systemctl restart kubelet
k uncordon $NODE

# node: drain, ssh
apt update && apt install kubeadm=1.29.0-1.1
kubeadm upgrade node
service kubelet restart
# exit, k uncordon
```
* sha512sum
```bash
  sha512sum -c FILE # can contain all sums and binaries
  # FILE
  # SUM1  /a/file
  # SUM2  another-file
  # or individual files
  sha512sum -c <<< "$1  $2"
```