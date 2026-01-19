# Kubernetes Certified Administrator (CKA)

## Preparation

* For an experienced k8s user (i.e. developer, not admin/cloud engineer), CKA is more of a challenge than CKAD.  
  If you haven't used kubeadm before and have never set up a cluster manually before, expect some learning curve!
* You can find almost everything in the docs, so time is the main issue.  
* Knowing the basic CLI commands of tools like `kubeadm`, `crictl`, `etcdctl`, `systemctl`, `apt`, `dpkg` and `openssl` advisable. 
* Knowing your way around the docs and [being fast](README.md#train-for-speed) in the terminal and with YAML is still of the essence.
* For me as an experienced developer (not admin!) 12 hours of focused preparation (on three days) was minimal but enough.  
  I just passed in 2020 and re-certified with about 80% in 2026:
    * After passing CKAD, I spent 3 hours working through [tools and technologies](#tools-and-technologies).
    * The next morning, I took the first killer.sh simulated exam and learned from my mistakes.
    * In the afternoon I did the second killer.sh exam (which was tougher) 
    * On the day of the exam, I spent the morning learning from my mistakes and doing the remaining exercises.
    * Took the exam after lunch and succeeded well enough.

## Curated Resources

* Interactive learning environments
    * Two tries for **two different** interactive killer.sh are included in your exam voucher. Use them, but use them wisely!
    * If you need more, create an account at [killercoda](https://killercoda.com/killer-shell-cka)
* Community resources
    * [Walid Shaari's curated resources](https://github.com/walidshaari/Kubernetes-Certified-Administrator/blob/main/README.md)
* Example questions
    * [15 example questions from 2024 by Artem Lajko](https://itnext.io/how-the-cka-certification-has-changed-from-2021-to-2024-1e98b1ff6c88)
    * [arush-sal/cka-practice-environment](https://github.com/arush-sal/cka-practice-environment/tree/master/lab/files/questions) several years old, but looking through the example questions still gives you a good idea of what to expect.

## Tools and technologies

Also look at [CKAD](CKAD.md#tools-and-technologies).

* If you have enough time and are not too experienced administering k8s clusters, do Kelsey Hightower's [kubernetes-the-hard-way](https://github.com/kelseyhightower/kubernetes-the-hard-way/)
* [Static pod](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/) (generic VMs) or [mmumshad/kubernetes-the-hard-way](https://github.com/mmumshad/kubernetes-the-hard-way) (local VMs using Vagrant) or [ghik/kubernetes-the-harder-way](https://github.com/ghik/kubernetes-the-harder-way) (local VMs using QEMU).
    * `/etc/kubernetes/manifests`
    * YAML manifest on the worker node is automatically detected and managed by the kubelet on that specific node, rather than Kubernetes API server.
* `nodeSelector` and `nodeAffinity`
    * Simple field in pod, matching label
    * When the label's value is empty use this: `node-role.kubernetes.io/control-plane: ""`
    * Selector is simpler (all or nothing), affinity provides more options (`matchExpressions` using `operator`)
* `nodeAffinity` vs. `podAffinity`: which node to run on vs. which pods to be near to. There's also antiAffinity.
* [`topologySpreadConstraint`](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/): topologySpreadConstraint: more complex that podAffinity, allows spreading across availability zones
* Taints/Tolerations
    * Taints applied to nodes avoid scheduling certain pods. toleration are the counterpart on pods allowing scheduling on certain nodes.
    * [docs](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)
    * When the label key does not matter (or is set with an empty value)
        * `key: node-role.kubernetes.io/control-plane`
          `operator: Exists` ➡️ Can be left out
    * effects
      `NoSchedule` ➡️ the usual suspect only for new pods
      `NoExecute` ➡️ would also evict existing pods
* `kubeadm`
    * [general docs](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)
    * [upgrade docs](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/), note the slight differences for control plane and [node](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/upgrading-linux-nodes/#drain-the-node).
    *  [**Troubleshooting Clusters**](https://kubernetes.io/docs/tasks/debug/debug-cluster/)
    * `drain` and `uncordon`
    * kubelet runs as process using systemd
    * All other core components are static pods in `/etc/kubernetes/manifests/`, i.e. `kube-apiserver`, `kube-scheduler.yaml`, `kube-controller-manager.yaml`, `etcd`
    * CoreDNS and CNI (e.g. calico, weave-net) are installed directly into the cluster and can be changed there.
    * Also, kubelet config can be edited in cluster ➡️ it is distributed to the nodes from there
      `kubectl edit cm -n kube-system kubelet-config`
    * How to Restart api-server? Three options
        * Move Manifest:  `sudo mv /etc/kubernetes/manifests/kube-apiserver.yaml /tmp/`  
          To restart: `sudo mv /tmp/kube-apiserver.yaml /etc/kubernetes/manifests/`
        * Stop pod:  
          `sudo crictl ps --name kube-apiserver -q`  
          `sudo crictl stop $CONTAINER_ID`
        * Restart kubelet: `sudo systemctl restart kubelet`
        * ➡️ Restart can take a minute: `watch crictl ps`
    * Be ready to install Container Runtime via present `.deb` packages
      `dpkg -i`, then `systemctl start`
    * Preparing the env by using [`/etc/sysctl.d/k8s.conf`](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
      E.g. to set `net.ipv4.ip_forward = 1`
* kubelet
    * `sudo systemctl status kubelet # start|stop` (systemd)
    * (`sudo service kubelet status` (legacy wrapper))
    * `sudo journalctl -u kubelet`
    * `sudo systemctl edit kubelet`
    * Config in running cluster: `kubectl edit cm -n kube-system kubelet-config` edit cm!
      (`/var/lib/kubelet/config.yaml` is generated from it on every node)
    * kubelet down leads to api-server down, because kubeadm starts core components as static pods managed by kubelet.
    * Debugging: Running binary manually `/usr/local/bin/kubelet` might help
    * When you have to fix a component (like kubelet) in one cluster, check how it's set up on another node in the same or even another cluster. 
      You can copy config files over etc.
    * Certs: `/var/lib/kubelet/pki/kubelet.crt`
* api-server
    * `kubectl -n kube-system get pod`
    * `/etc/kubernetes/manifests/`  ➡️ api server config YAMLs
    * `/var/log/kube-apiserver.log`
    * `systemctl status kube-apiserver`
    * Know how to set the CIDR rules for the service network.
* Scheduler/scheduling
    * Manual scheduling by setting `nodeName` in pod
    * Disable scheduler temporarily by moving scheduler.yaml from `/etc/kubernetes/manifests/` to different folder. 
      Returning it will restart scheduler.
* `etcd`
    * `/etc/kubernetes/manifests/etcd.yaml`
    * `/etc/kubernetes/pki/etcd/server.crt`
    * `cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep etcd`
    * From inside the etcd pod: 
        * `etcdctl version`
        * `etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key endpoint health`
    * From control plane
      `ETCDCTL_API=3 etcdctl snapshot save /opt/course/7/etcd-snapshot.db --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key`
    * `--endpoints=https://127.0.0.1:2379` is not needed in pod nor api-server node
* Certificates
    * `kubectl certificate approve` or `deny`.
    * `openssl x509 -noout -text -in /etc/kubernetes/pki/apiserver.crt`
    * `sudo kubeadm certs`
        * renew
        * check-expiration
    * Enable certificate rotation for the cluster
        * kubelet configuration file (usually found at `/var/lib/kubelet/config.yaml`)
    * [docs](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/)
* HorizontalPodAutoscaler
    * `kubectl autoscale rs foo --min=2 --max=5 --cpu-percent=50`
    * [docs](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/)
* PVCs, Volumes
    * Know the docs: [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/) and [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
    * Practice creating `PV`s and `PVC`s from the docs.
    * Be able to manually connect PVC and PV
    * Be able to create a storage class
    * 💡 With dynamic provisioning (like [local-path-provisioner](https://github.com/rancher/local-path-provisioner)), PVCs get bound only when they are mounted into pod
* Gateway API
    * [Internal docs](https://kubernetes.io/docs/concepts/services-networking/gateway/) are enough for simple questions
    * [External docs](https://gateway-api.sigs.k8s.io/) (allowed in CKA!) for advanced things like modifying or routing based on HTTP headers
    * `HTTPRoute` ➡️ similar to ingress, just different syntax and more options
    * `Gateway` ➡️ Contains user facing ports and TLS `certificateRefs` (kubernetes secrets)
    * `GatewayClass` ~ `IngressClass`
    * Be able to migrate an Ingress to a `Gateway` and `HTTPRoute` including TLS cert
* Understand and use CoreDNS
    * [CoreDNS Debugging docs](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)
    * [CoreDNS ConfigMap options](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)
    * `kubectl edit configmap coredns -n kube-system`
* `crictl` Basics. Subcommands similar to docker, e.g. `crictl ps`, `logs`, `inspect` but also knows pods
    * `crictl ps` (list **containers**)
    * `crictl pods` (list **pods**)
    * `crictl rmp` (remove pod) (without `p` removes containers)
    * `crictl version` also RuntimeName and version
    * `crictl info` CNI, containerd, status
* Important folders to remember
    * `/etc/kubernetes/manifests/`, e.g. `kube-apiserver.yaml`
    * `/var/log/` e.g. `kube-apiserver.log`
    * `/etc/kubernetes/pki/` e.g. `etcd/server.crt`
    * `/var/lib/kubelet/` e.g. `pki/`
    * `/etc/cni/net.d/` - kubelet takes CNI config from there
* Know how to work with CRDs 
  * `k get crd | grep ...`
  * Get overview of CRD fields:  
    `k explain applications --recursive`
  * Get description single CRD field:  
    `k explain applications.argoproj.io.spec.sources --recursive`
  * Be able to use RBAC on CRDs.
* [QoS/Pod Eviction](https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/)
    * Pods that are using more resources than they requested are evicted first
    * If some Pods containers have no resource requests/limits set, then by default those are considered to use more than requested
    * Kubernetes assigns Quality of Service classes to Pods based on the defined resources and limits.
    * Practice creating your own [`PriorityClass`](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/) and assigning it to pods.
* Debugging Applications:
  * [Troubleshoot application failure](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/) 
  * [Pending or terminated pods](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#troubleshooting)
* CNI:
    * [Docs CNI with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network)
    * Know how to install CNI.  
      Try it with [Calico](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart) in kubeadm, for example.  
      ➡️ netPols are enabled by default.
      For exercises in the exam, docs like calico's will be linked and allowed, if necessary.
    * Check node `ready` after install.
    * For training [Calico the hard way](https://docs.tigera.io/calico/latest/getting-started/kubernetes/hardway/overview) sheds a lot of light into this
