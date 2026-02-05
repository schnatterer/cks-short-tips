# Kubestronaut
<img alt="Kubestronaut icon" src="https://www.cncf.io/wp-content/uploads/2024/03/kubestronaut-stacked-color.svg" align="right" width="15%">

General advice on how to become a Kubestronaut and if you're up for it, the [**Kubestronaut in 4 weeks**](#kubestronaut-in-four-weeks) challenge 🚀.

## Topical overlap of Kubestronaut exams
There is a clear topical overlap between the Kubestronaut exams. Here is a rough approximation: 

<img alt="topical overlaps between the Kubestronaut exams" src="Kubestronaut-certs.drawio.svg" width="75%">


* KCNA is somewhat of a theoretical basis of CKAD,
* For the practical exams, CKAD lays groundwork for CKA,
* which in turn lays the groundwork for CKS.
* KCSA is again theoretical and mainly covers parts of CKA with some theory that cannot be covered practically in CKS. It also has some Kubernetes basics that are closer to CKA/CKAD.

Some practical examples:

* **KCNA**: requires theoretical knowledge of containers, basic Kubernetes resources like Pods, Deployments, Services, etc. and a basic understanding of `kubectl`.
* **CKAD**: requires about the same knowledge, but practical and [fast](README.md#train-for-speed).
* **CKA**:
  * Typically, has questions that could be part of CKAD like `helm`, `NetPols`, `PV`s+`PVC`s, `Service`s, `Ingress`es, RBAC, Resource Management and Kustomize.
  * In addition, it requires knowledge about `CRDs`, static pods, `kubeadm`, debugging control planes, working with Kubelet, etcd, CNI, Gateway API
* **CKS**: 
  * Basesd on things from CKAD but might not ask for them specifically (except for security-related things like NetPols and RBAC maybe)
  * Based on things from CKA like `kubeadm`, debugging control planes, working with Kubelet, etcd
  * In addition, it requires knowledge about advanced security-related k8s topics like authN, certificates, plugins, auditing, AppArmor, seccomp, PSA. 
  * On top of that there are other tools like `kube-bench`, Trivy, Falco, OPA
* **KCSA**: 
  * Might have some basic Kubernetes/`kubectl` questions, but also theoretical questions related to CKS like Auditing, AppArmor, Admission Control, RBAC, authN, container security, K8s hardening/multi-tenancy. 
  * On top of that there are many terms you should know for CKS but can't be asked there, like provenance, attestation, control plane vs. data plane, certificates (e.g .server cert, `0=system:masters` ), vulnerability scanning/triage, trust boundaries, securing kubelet, cloud provider's metadata APIs. 
  * This includes threat modeling and compliance frameworks. 

## Order of exams

What does this topical overlap mean for the order of the exams? 

### My recommendation

1. Start practical with CKAD,
2. then move on to CKA,
3. and on to CKS.
4. Head on to the theory: KCNA and
5. finally KCSA.

### Why?

1. CKAD is rather easy, and you get to know the practical exam env. It also lays the topical foundation for the next exams.
2. CKA is the next step up the ladder, laying not only the topical foundation for CKS but also being a strict requirement by CNCF.
3. CKS is the most challenging part of all exams.
4. After that, KCNA is really easy, and you get to know the exam env for multiple-choice.
5. Become a Kubestronaut after finishing with KCSA, which is a bit more challenging than KCNA but after all the CK* exams more of a lap of honor.

### Alternatives
* You could also do the multiple-choice exams first. I think the practical exams are more important and challenging and include almost all topics covered in the multiple-choice exams. Getting at the challenging exams with a clear head and doing the easier ones at the end (where a certain exam fatigue might have set in) seems logical to me. 
* KCNA is so easy you could also do it earlier (right after CKAD, for example). But same here, focusing on the more challenging and important practical exams seems logical.

### Preparation

Advice on how long to prepare for each exam is difficult.  
You can find my preparation in the individual exam pages: [CKAD](CKAD.md#preparation), [CKA](CKA.md#preparation), [CKS](CKS.md#preparation), [KCNA](KCNA.md#preparation), [KCSA](KCSA.md#preparation).  

The Kubestronaut in four weeks challenge below also offers a [timeline](#timeline).

## Kubestronaut in four weeks

Challenge: Become a Kubestronaut in four weeks.

Based on [my own experience](#my-experience), it is efficient and [absolutely possible](#my-experience), at least for the seasoned developer or cloud engineer.

If you take on this challenge, I'd love to hear from you how it went. Any feedback is welcome.

### Timeline

Now, how to spend the four weeks wisely?

* Week 1-2: CKAD + CKA: my minimalistic take for re-certification (was tough but worked)
  * Tuesday after lunch: CKAD (about [8hrs of prep](CKAD.md#preparation))
  * Thursday after lunch CKA (about [12 hours of prep](CKA.md#preparation), tough!)
* Week 2 or 3: CKS (+ KCNA)
  * Friday after lunch: CKS ([one week of prep](CKS.md#preparation))
  * If you feel brave, do KCNA in the afternoon. You [literally don't need any preparation](KCNA.md#preparation).
* Week 3 or 4: KCSA (+ KCNA)
  * If not done already, do KCNA on Monday morning. You [literally don't need any preparation](KCNA.md#preparation).
  * Tuesday after lunch: KCSA (I suggest a minimum [4 hours of prep](KCSA.md#preparation)).

My general recommendation for scheduling exams:

* Schedule multiple exams over a time period that feels doable. This [increases commitment and avoids procrastination](README.md#have-a-plan-for-preparation-dont-procrastinate).
* Leave > 48h between exams.
  * This allows you to reschedule in case you did not pass the exam.
  * Results take up to 24h to arrive, and you can reschedule until 24 before the exam. Hence, the > 48h.
* For CKS you can likely only plan ahead but only schedule once CKA is passed, as CKA is a prerequisite for CKS.

### My experience

In a way, I did this challenge myself. I became a Kubestronaut in two weeks (but had a valid CKS before).

I also know of others who made it, e.g. read of others who made it, e.g.
* [LinkedIn](https://www.linkedin.com/posts/kevin-welter_kubernetes-cncf-cka-activity-7420465100706271232-UHhx)
* [🔒️ cncf-kubestronauts](https://cloud-native.slack.com/archives/C06KKSDSKR7/p1769507365587559) slack (Kubestronauts-only)

Here is my story:

At the end of 2025, the notice of my upcoming CKS expiry sparked the idea to become a Kubestronaut.  
What a nice challenge to start the new year with!  
Also, a lot of discounts on certs at the end of the year.

I actually started with the easy thing first (KCNA and KCSA).  
In retrospect, KCSA goes much better with CKS, so I recommend doing them in close succession.
Doing CKAD and CKS in one week (while having other work stuff to do) is quite a challenge, but possible.

Good luck with your Kubestronaut journey 🍀!  
I'd love to hear your feedback.
