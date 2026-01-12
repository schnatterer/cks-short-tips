cks-short-tips
===

1. [Have a plan for preparation](#have-a-plan-for-preparation)
   - [Curated CKS Resources](#curated-cks-resources)
2. [Train for speed](#train-for-speed)
   - [Disable unsafe pasting](#disable-unsafe-pasting)
   - [Use aliases and tab completion](#use-aliases-and-tab-completion)
   - [Be fast with CLI tools](#be-fast-with-cli-tools)
3. [Know your tools and technologies](#know-your-tools-and-technologies)
4. [Be thorough](#be-thorough)
5. [Don't give up](#dont-give-up)

<img width="25%" style="float: right" src="https://www.cncf.io/wp-content/uploads/2020/11/kubernetes-security-specialist-logo.svg" ></img>

# Have a plan for preparation

There a [tons of resources](#curated-cks-resources) out there. 
Use them to acquire a wider knowledge, before training specifically for the exam.

Here's my plan, as an example:

I invested about 5 working days over a period of three weeks before the exam.  
As a seasoned developer in the public cloud I was quick with kubectl, security-context and so on, but not so much with api-server config.  
Retrospectively, spending maybe one more day would not have been advisable, as I had to look up a lot of api-server-related stuff in the docs during the exam.

1. **Read the Docs for the Certs.**  
   IMO most importantly: [Resources Allowed: All LF Certification Programs - T&C DOCS (Candidate Facing Resources)](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed#certified-kubernetes-security-specialist-cks).
2. **Work your way through the curriculum.**  
   * read through [Walid Shaari's curated resources](https://github.com/walidshaari/Certified-Kubernetes-Security-Specialist), which add a lot of useful detail to rather abstract curriculum.
   * follow-up reading on every topic!
   * get your handy dirty! Install every tool on your local kubernetes cluster and apply the getting started docs.  
     I use k3d via [gitops playground](https://github.com/cloudogu/gitops-playground)  
3. **Solve example questions, learn from the results and from your errors.**  
   * [Mohamed Abukar's mock exam questions](https://github.com/moabukar/CKS-Exercises-Certified-Kubernetes-Security-Specialist/tree/main/7-mock-exam-questions)
   * [These](https://github.com/snigdhasambitak/cks) look like old killer.sh Questions with solutions, but no harm in having a look at them before having you first killer.sh try.
   * These look promising, including a learning platform: [ViktorUJ/cks](https://github.com/ViktorUJ/cks/tree/master/tasks/cks/labs)  
     I discovered them only after I passed my CKS, though.
4. **Have your first try at killer.sh (the week before the exam)**  
   * Take it serious, but be prepared to fail.
   * Main objective: Get accustomed to the exam environment and find out what to improve
   * Note down all your weak spots, both technologically as well es regarding efficiency on the shell/keyboard
5. **Have your second try at killer.sh (one or two days before the exam)**
   * This should give you confidence that you're now fully prepared and very time efficient with the tools
   * Note down some more fine-tuning
   * Follow up on the things that were not perfect
6. **Pass the exam**
   * Begin with the first question and try to solve as many questions as possible
   * Only if you're very unsure, flag some questions and come back later.  
     Note that this takes some time, because you'll likely read the question and think about it twice.  
     On the other hand, losing a lot of time with one question might lead to not having enough time to complete easier tasks later.
   * [Don't give up until the exam ends 💪](#dont-give-up)
   * 🥂

## Curated CKS Resources

* Interactive learning environments
  * Two tries for interactive killer.sh included in your exam voucher. Use them, but use them wisely!
  * If you need more, create an account as [killercoda](https://killercoda.com/killer-shell-cka)
  * [ViktorUJ/cks](https://github.com/ViktorUJ/cks/tree/master/tasks/cks/labs) - local platform for learning kubernetes and preparation for CKS
* Community resources
  * [Walid Shaari's curated resources](https://github.com/walidshaari/Certified-Kubernetes-Security-Specialist)
  * [Video Tutorials by Venkata Ramana Gali](https://www.youtube.com/watch?v=jvmShTBSBoA&list=PLFkEchqXDZx6Bw3B2NRVc499j1TavjOvm),
    corresponding [Repo](https://github.com/ramanagali/Interview_Guide/blob/main/CKS_Preparation_Guide.md)
* Example questions
  * [Mohamed Abukar's mock exam questions](https://github.com/moabukar/CKS-Exercises-Certified-Kubernetes-Security-Specialist/tree/main/7-mock-exam-questions)
  * These look promising, including a learning platform: [ViktorUJ/cks](https://github.com/ViktorUJ/cks/tree/master/tasks/cks/labs)
  * [These](https://github.com/snigdhasambitak/cks) look like old killer.sh Questions with solutions, but no harm in having a look at them before having you first killer.sh try.
* [Udemy Courses](https://www.udemy.com/topic/certified-kubernetes-security-specialist-cks/)  
  even when you don't take a course, having a look at the content might help discovering topics that are relevant
* There are a lot more, when you follow the links in the repos above.

# Train for speed

## Disable unsafe pasting

[Disable](https://killer.sh/faq) "unsafe pasting" in VM terminal emulator
  `"Terminal Preferences->General->Show unsafe paste dialog".`

## Use aliases and tab completion

Those are the ones I used
```shell
vi ~/.bashrc
alias kg='k get'
alias kd='k describe'
alias ka='k apply -f'
alias kn='k config set-context --current --namespace'

export do="--dry-run=client -o yaml"

# : wq
source ~/.bashrc
# or 
tmux
```

Note:
* Tab completion does not work for aliases 😐️   
 So I mainly use them to get a fast overview of pods, services, etc.
```bash
kg po
kg svc
kg ing
kg deploy
```
* There already is an alias `k=kubectl`.  
  Tab completion does work for it, so use e.g. `k get po <TAB>` instead of typing or copying pod names
* killer.sh recommends defining these variables (BTW, for this to work in zsh, you would have to enforce word splitting: `$=do`).  
  Instead of the latter, I usually do `ctrl` + `z` + `bg`
```bash
export do="--dry-run=client -o yaml"    # k create deploy nginx --image=nginx $do

export now="--force --grace-period 0"   # k delete pod x $now
```
* vi is already set up with useful options, usually no need to edit `~/.vimrc`

## Be fast with CLI tools

* Bash:
    * `ctrl` + `z`: keep open but return to shell
    * then `fg` return e.g. in `vi`
    * or `bg` keeps running in background e.g. for killing pods
* `tmux` - first things in an exam: edit `bashrc` (see above), then start `tmux`
	* split pane, preferable horizontal (`ctrl`+`b`+`"`) (easier for copying with mouse)
	* move from pane to pane using `ctrl`+`b` + cursor keys
	* select mode (scrolling) `ctrl`+`b` + `]` - then use search or cursor keys or page up/down for navigating
	  Careful: `ctrl`+`w` might close tab. Better use mouse for copying.
	* search: in select mode `ctrl`+`s` or `r` for reverse
	  `n` for next hit; `Shift+n` for previous hit
	* increase size of pane by keeping `ctrl`+`b` pressed and using cursor keys
    * `ctrl`+`z` to toggle full screen for a pane
* `vim`
	* `w` write, don't quit. Hints:
      * use other tmux pane to run `ka`, then come back to fix potential syntactic errors
      * or use `ctrl` + `z` to send `vim` to background, run `ka` then `fg` to return to `vim` 
    * `q` quit, can be combined to `wq` 
	* `y`  = yank = copy
	* `p` = paste
	* `d` = delete = cut. If you want to delete more lines just do, e.g. `dddd`
    * `u` = undo
	* redo: `ctrl` + `r`
	* `v` = visual (select), use cursor keys, then `y` or `p`
	* indent: select then `shift` + `>` or `<` (repeat with `.`)
* `less`
	* switch search to case-insensitive using `:i`
	* search using `/`
* `ssh` / `scp`, to access or copy files to/from nodes
* `cut --delimiter ' ' --fields 9 # delimiter space, print field 9`

# Know your tools and technologies

* curl
    * Validate certificate: `curl -vk 2>&1  https://xyz | grep -i subject`
    * [Accessing the Kubernetes API from a Pod](https://kubernetes.io/docs/tasks/run-application/access-api-from-pod/)
      Note the path to the secrets
```bash
k run debug-node --rm -it --image alpine --
'curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api/v1/namespaces/default/secrets'
```
* `kube-apiserver`, `kubelet`, etc.
    *  interne YAMLs konfigurierbar:
    * `/etc/kubernetes/manifests`
    * `/etc/kubernetes/manifests/kube-apiserver.yaml`
    * `/etc/kubernetes/manifests/kube-controller-manager.yaml`
    * `/var/lib/kubelet/config.yaml`
    * apiserver is automatically restarted when `kube-apiserver.yaml` is changed.  
      If need be this can be forced using `crictl rmp` , or even using `kubectl` (`k delete pod`) on a controlplane node or using restarting the service
```shell
#SystemD
systemctl status kubelet
systemctl restart kubelet
# or system V
service kubelet restart
service kubelet status
```
* [AppArmor](https://kubernetes.io/docs/tutorials/security/apparmor/)
    * AppArmor profiles are specified _per-container_. To specify the AppArmor profile to run a Pod container with, add an annotation to the Pod's metadata
    * `container.apparmor.security.beta.kubernetes.io/<container_name>: <profile_ref>`
    * `container.apparmor.security.beta.kubernetes.io/c1: localhost/very-secure`
    * or `runtime/default`
    * Profile must be present on node
    * Accessing [App Armor Docs](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation) ist permitted
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
apparmor_parser -q profile.txt # Output profile alrady exists
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
# or
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
```
* [PSA](https://kubernetes.io/docs/concepts/security/pod-security-admission/)
    * PSAs are set namespace-wide
    * Try out e.g. this example: [Apply predefined Pod-level security policies using PodSecurity  |  Google Kubernetes Engine (GKE)  |  Google Cloud](https://cloud.google.com/kubernetes-engine/docs/how-to/podsecurityadmission)
    * `kubectl label --overwrite ns restricted-ns pod-security.kubernetes.io/enforce=restricted`
    * `kubectl label --overwrite ns baseline-ns pod-security.kubernetes.io/warn=baseline`
* Falco
    * Might log to syslog -> `cat /var/log/syslog | grep -i xyz` or `journalctl -fu falco | grep "xyz error"`
    * Output UID and Container ID: `%user.uid,%container.id` see [supported fields doc](https://falco.org/docs/reference/rules/supported-fields/) (allowed in exam!)
```yaml
rules_file:
  - /etc/falco/falco_rules.yaml
  - /etc/falco/falco_rules.local.yaml
  - /etc/falco/rules.d # custom rules
```
* Tracee
  similar to falco, ebpf kernel events, signatures tigger alert.
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
	* Has to run on node! Either operator, pod or binary on node
	* `kube-bench run --targets=master > bench.txt`
	* `kube-bench run --targets=node > bench.txt`
	* Using `--target` makes solving specific tasks much easier because there usually are a lot of results
	* Writing to file allows for comparing the results after mitigation.
	* `kube-bench run --targets=master | less` also works
* crictl, similar to `docker` but for CRI
```shell
crictl pods
crictl ps

crictl ps --pod $podid # all containers of pod, also works with grep $podid
crictl logs $containerid # see output of crcitcl ps --pod for $containerid
crictl rmp $podid #remove pod
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
critctl ps --pod POD_ID # -> CONTAINER_ID
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
    * `ConstraintTemplate` -> describe rego
    * `Constraint` -> describes matching rules, parameters, etc., lists violations
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
kubeadm upgrade apply v1.24.1 #use specific version! Outputs message to upgrade kubelet
apt install kubelet=1.29.0-1.1 kubectl=1.29.0-1.1
service kubelet restart #systemctl restart kubelet
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
  #SUM1  /a/file
  #SUM2  another-file
  # or indivudal files
  sha512sum -c <<< "$1  $2"
```

* You might want to install helpful software. Not sure if it is allowed, though
    * `apt install bat`-> binary is called `batcat` - syntax highlighted YAML files
    * or `tldr` if you need examples for using binaries
      or `curl cheat.sh/cut`

# Be thorough

* Switch to the correct kubecontext first (there is a command to copy at the beginning of each question)
* Read questions carefully! There a several subtasks.
* Read again when done to see if you did not miss anything.  
* Validate your work, e.g. `k get node`, `k get pod`, `k exec -it ...`
* Double check if you wrote the result to the right file
  * Sometimes it has to be written to a file on the main host
  * Sometimes it has to be written to a file on a node
  * Sometimes it is not necessary to write a result
* Every exercise features links to the relevant docs in the header. Have a look a them before searching in the docs first!
* Copy each YAML before editing to your home folder, to be able to reset after possibly messing it up.  
  Don't copy it to the folder of the original file, this might confuse a running api server, for example.

In both my killer.sh tries I missed some points.  
Even though I solved things correctly or would have been able to, I wrote the result into a wrong file or missed to solve a subtask.  
Don't do that, be thorough!

# Don't give up

* You are going to need every point to can score and every second you're granted
* If some tasks might seem to difficult, flag them and tackle them again at the end
* Having only one or two minutes left is enough time to score some points by partially solving some question
* You'll be surprised how fast you can learn to solve tasks under pressure
* If you don't know how to solve, start by reading the docs linked at the top
* You don't need to solve the whole question, partial solutions score points as well
* You never know, a partial solution saved in the last seconds might be the one point you need for the 70% passing score!

Don't give up until you're kicked out of the exam 💪
