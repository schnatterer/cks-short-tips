ckad-cka-cks-kubestronaut
===

[<img src="https://about.cloudogu.com/assets/cloudogu-logo-f1f26e5c32f240d0a161cebe71e91138ee5662c1f99fb314539a4763ef5959a946130f1a12e9bc39c83c127092a66b919d1fae86fadd5fe9639d06032787a03d.svg" align="right" width="25%">](https://cloudogu.com/)

Tips for preparing for the CKAD, CKA, CKS, KCSA and KCNA exams and how to become a kubestronaut.

This repo contains experiences I gathered while passing seven CNCF certifications between 2018 and 2026, working as a developer, trainer, cloud engineer, tech lead and field CTO at [Cloudogu](https://cloudogu.com/).

[<img title="CKAD" width="15%" src="https://www.cncf.io/wp-content/uploads/2021/09/kubernetes-ckad-color.svg">](CKAD.md)
[<img title="CKA" width="15%" src="https://www.cncf.io/wp-content/uploads/2021/09/kubernetes-cka-color.svg">](CKA.md)
[<img title="CKS" width="15%" src="https://www.cncf.io/wp-content/uploads/2020/11/kubernetes-security-specialist-logo.svg">](CKS.md)
[<img title="KCSA" width="15%" src="https://www.cncf.io/wp-content/uploads/2024/03/kubernetes-kcsa-color.svg" >](KCSA.md)
[<img title="KCNA" width="15%" src="https://www.cncf.io/wp-content/uploads/2021/09/kcna_color.svg">](KCNA.md)
[<img title="Kubestronaut" width="15%" src="https://www.cncf.io/wp-content/uploads/2024/03/kubestronaut-stacked-color.svg">](Kubestronaut.md)


This main README contains generic tips that are true for each of the exams.  
In addition, there are separate files for each exam, containing these chapters:

* Preparation (how I (as an experienced engineer) prepared for the exam)
* Curated resources (useful resources I used)
* Tools and technologies (a compact list of things I deem important for the exam)

## TOC
* [README](README.md) (📌 you are here)
  * [Short Tips](#short-tips)
* [CKAD](CKAD.md)
  * [Preparation](CKAD.md#preparation)
  * [Curated resources](CKAD.md#curated-resources)
  * [Tools and technologies](CKAD.md#tools-and-technologies)
* [CKA](CKA.md)
  * [Preparation](CKA.md#preparation)
  * [Curated resources](CKA.md#curated-resources)
  * [Tools and technologies](CKA.md#tools-and-technologies)
* [CKS](CKS.md)
  * [Preparation](CKS.md#preparation)
  * [Curated resources](CKS.md#curated-resources)
  * [Tools and technologies](CKS.md#tools-and-technologies)
* [KCNA](KCNA.md)
    * [Preparation](KCNA.md#preparation)
* [KCSA](KCSA.md)
  * [Preparation](KCSA.md#preparation)
  * [Curated resources](KCSA.md#curated-resources)
  * [Tools and technologies](KCSA.md#tools-and-technologies)

## Short Tips

1. [Have a plan for preparation. Don't procrastinate.](#have-a-plan-for-preparation-dont-procrastinate)
2. [Train for speed](#train-for-speed)
3. [Know your tools and technologies](#know-your-tools-and-technologies)
4. [Be ready for non-kubernetes challenges](#be-ready-for-non-kubernetes-challenges) 
5. [Be thorough](#be-thorough)
6. [Don't give up](#dont-give-up)

### Have a plan for preparation. Don't procrastinate.

Plan your learning time. My personal estimates can be found in the `Preparation` section: [CKAD](CKAD.md#preparation), [CKA](CKA.md#preparation), [CKS](CKS.md#preparation).  

Here is the plan I used for exams:

0. **Schedule the exam.** This increases commitment and avoids procrastination.  
   You can always reschedule it up until 24 hours before the exam.
1. **Read the Docs for the Certs.**  
   * Read the [candidate handbook](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2) to learn about the exam platform.
   * Find out which [resources are allowed](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed) for your specific exam.
   * The most important ones are the kubernetes docs. Knowing your way around these will save you a lot of time during the exam. 
2. **Work your way through the curriculum.**
   * [cncf/curriculum](https://github.com/cncf/curriculum): look for the PDFs at the bottom.
   * Read through preparations notes.  
     See the `Curated resources` sections: [CKAD](CKAD.md#curated-resources), [CKA](CKA.md#curated-resources), [CKS](CKS.md#curated-resources) 
   * e.g. Walid Shaari's Curated resources offer insight on all three certifications, which add a lot of useful details to the rather abstract curriculums.
   * Follow-up reading on every topic!
   * Get your hands dirty! Experiment with imperative `kubectl` commands on your local cluster.  
     I use k3d via [gitops playground](https://github.com/cloudogu/gitops-playground)  
   * For CKS install every tool (like falco and OPA) on your local kubernetes cluster and apply the getting started docs.
3. **Solve example questions, learn from the results and from your errors.**  
   See the `Curated resources` sections: [CKAD](CKAD.md#curated-resources), [CKA](CKA.md#curated-resources), [CKS](CKS.md#curated-resources)
4. **Have your first try at killer.sh (a couple of days before the exam)**  
   * Take it seriously, but be prepared to fail.
   * Main objective: Get accustomed to the exam environment and find out what to improve
   * Note down all your weak spots, both technologically as well es regarding efficiency on the shell/keyboard
5. **Have your second try at killer.sh (one or two days before the exam)**
   * This should give you confidence that you're now fully prepared and very time-efficient with the tools
   * Note down some more fine-tuning
   * Follow up on the things that were not perfect
6. **Pass the exam**
   * Begin with the first question and try to solve as many questions as possible
   * Only if you're very unsure, flag some questions and come back later.  
     Note that this takes some time because you'll likely read the question and think about it twice.  
     On the other hand, losing a lot of time with one question might lead to not having enough time to complete easier tasks later.
   * [Don't give up until the exam ends 💪](#dont-give-up)
   * 🥂

### Train for speed

Be fast on the command line!

Note that other than in the killer.sh env, in the proper exam you can switch between windows using Alt+Tab.   
Use it to quickly switch between docs in the browser and the terminal or multiple terminal instances.

#### kubectl

* There already is an alias `k=kubectl`. Use it!  
  Tab completion does work for it, so use e.g. `k get po <TAB>` instead of typing or copying pod names
* Use shortnames for resources, e.g. `po`, `svc`, `deploy`, `ns`, `ing`, `sa`, etc.  
  See ` k api-resources`  
  💡 List only namespaced resources: `k api-resources --namespaced`  
* Use `k run` to create pods
* Use `k expose` to create services. Parameters like `--name abc` `type=NodePort` are useful.
* Use `k create` to create other resources. Like Deployments, Secrets, etc.  
  But: Try out if finding the examples in the docs works faster for you. e.g. I prefer using the docs for `ingress`.
* Use `--dry-run -o yaml > out.yaml` to generate skeleton YAMLs.  
  * `--dry-run` without e.g. `=client` is depcrecated, but still faster! Just ignore the warning.
  * For non-idempotent resources like `deployments` and rather simple tasks, it might be faster to execute the command without `--dry-run` and then `k edit` the resource in-situ.
* Know how to switch a namespace as fast as possible: `k config set-context --current --namespace=ns`:
  `set-co<Tab>` , `--cur<Tab>`, `--na<Tab>`, either paste NS or start typing it then `<Tab>`.
* For killing single pods use `k delete pod x --now&`.  
  It has a `grace-period` of 1 second, but I still think it is faster than typing `--force --grace-period 0`. The `&` sends it to the background so you can continue typing the next command.
* For killing pods in a deployment use `k rollout` restart or `k scale`.
* Simple commands for testing something: 
  * `k run test --image nginx:alpine`  
    then  
    `k exec test -- wget -O- ..`
  * On Shot Alternative:  
    `k run tmp --restart=Never --rm -i --image=nginx:alpine -- curl ..`
* Prefer `k get events` over `k events`, because it has more parmeters like `--sort-by`


#### Other CLI tools

* Bash:
    * `ctrl` + `z`: keep open but return to shell
    * then `fg` return e.g. in `vi`
    * or `bg` keeps running in background e.g. for killing pods
    * Cursor up/down to quickly repeat last command (e.g. `k apply`)
    * `Ctrl + r` for searching in history. Often faster than cursor if it is not the last command you want to execute again.
    * When needing to run a command before finishing a line, save it by prepending with `#`.  
     Often faster than ctrl+c and retyping everything from scratch.
* `vim`
    * vi is already set up with useful options (like paste mode), usually no need to edit `~/.vimrc`
	* `w` write, don't quit. Hint:  
      use `ctrl` + `z` to send `vim` to background, run `k apply -f` then `fg` to return to `vim` 
    * `q` quit, can be combined to `wq` 
	* `y`  = yank = copy
	* `p` = paste
	* `d` = delete = cut. If you want to delete more lines just do, e.g. `dddd`
    * `u` = undo
	* redo: `ctrl` + `r`
	* `v` = visual (select), use cursor keys, then `y` or `d`
	* indent: select then `shift` + `>` or `<` (repeat with `.`)
* `less`
	* switch search to case-insensitive using `:i`
	* search using `/`
* `helm`
    * `helm search repo nginx --versions`
    * `helm history` -> `helm rollback cart-service 1 -n production`
    * `helm show values scm-manager/scm-manager` - show possible values of chart
    * `helm get values scmm` - show actual values of release
    * `helm upgrade internal-issue-report-apiv2 killershell/nginx -version <TARGET_VERSION> --reuse-values --dry-run`
* In CKA and CKS in tasks relating to `kubeadm`, Kubelet and debugging, `sudo -i` is faster than prepending all commands with `sudo`.
* (optional) `k get XYZ -o yaml | yq e` highlights an existing resource.  
  For existing files you can use `vi`.
* (optional) `ssh` / `scp`, to access or copy files to/from nodes
* (optional) `cut --delimiter ' ' --fields 9 # delimiter space, print field 9`

#### What about aliases and tmux?

I used to use and recommend using aliases and tmux, which is no longer true.
By now, in the exam
* you're working in a remote desktop environment,
* have to solve the questions on different hosts (via `ssh`)
* and tmux is no longer pre-installed.

That means you would have to configure aliases and install tmux multiple times.

Instead, if you really need a second terminal, you could open another instance of it.  
Most of the time I use the clipboard instead or switch out of vim to the terminal quickly using `ctrl` + `z`.

### Know your tools and technologies

See [CKAD](CKAD.md#tools-and-technologies), [CKA](CKA.md#tools-and-technologies), [CKS](CKS.md#tools-and-technologies)

### Be ready for non-kubernetes challenges

* The exam takes 2 hours. You can check in between 30 mins before and 15 mins after the exam.
* I recommend being there as early as possible, as the check-in process takes between 10 minutes (typical) and > 30 Minutes (technical errors).  
  This time is not subtracted from your exam time.
* You are allowed to bring a glass of water. Do it! Staying hydrated helps you think.
* When there is an error during check-in, you can always restart the PSI browser.  
  But you'll have to go through the whole check-in process again.
* Some things to avoid during check-in
  * Multi-monitor setup: Make sure to disable all but one monitor and fold your laptop when using an external monitor.  
    I once had disabled them only in the OS, and the proctor asked me to close my laptop. This enabled my second monitor, leading to a full-screen error. I then had to restart the PSI browser and do the check-in all over again.  
    Lesson Learned: Close the laptop and switch off the other monitor physically or even unplug it.
  * Have a long wire on your webcam or use your phone to check in.  
    * Once my phone camera screen froze, I did not see what the camera was transmitting. But it still worked for the proctor.
      Until my phone switched the display off.
    * I then switched to the webcam, but due to the panning around the room, the webcam must have disconnected shortly. I was presented with a full-screen error and had to restart the PSI browser and do the check-in all over again.
* You can reload the remote desktop environment using `Ctrl` + `R`, e.g. when it shows reconnecting but nothing is happening
* Don't use the thumb buttons of your mouse.  
  I once pressed them out of habit. This led to the PSI browser returning to the check-in. I managed to return to my exam but could only see the question while the remote desktop was showing a reconnecting message. When it did not come back, I contacted the proctor, who called in tech support. Before doing anything they unhurriedly asked me for my email, OS, etc. All the while my exam time was ticking away. At some point, I must have pressed `Ctrl` + `R` in desperation. Then my remote desktop came back, but I still had to get rid of the tech support which kept pinging me in the chat. This whole thing took me about 15 minutes and made me quite nervous.
* Make sure to not look to the side.  
  Once I felt a bit warm in the flow and took off my jacket and hung it over my chair. Later I put it back on and got a full-screen warning that said if I ever looked to the side again, my exam would be canceled 😓

### Be thorough


* Switch to the correct kubecontex first (there is a command to copy at the beginning of each question)
* Read questions carefully! There are several subtasks.
* Switch namespaces! It is much less error-prone and usually faster to switch once and no longer having to remember to add `-n x` to each command or `namespace: x` to a resource.  
 As soon as you discover a namespace in the question, switch to it!  
 Sometimes they are implicit, e.g. when talking about resources in `kube-system` or they are hidden in a YAML file. 
* Read the question again when you're done solving it to see if you did not miss anything.  
* Validate your work, e.g. `k get node`, `k get pod`, `k exec -it ...`
* Double-check if you wrote the result to the right file
  * Sometimes it has to be written to a file on the main host
  * Sometimes it has to be written to a file on a node
  * Sometimes it is not necessary to write a result
* Every exercise features links to the relevant docs in the header. Have a look at them before searching in the docs first!  
  These might even be docs that were not on the list of allowed resources 🤯
  e.g. during CKA you might have to install some tool (CNI) using its docs.
* Copy each YAML before editing to your home folder to be able to reset after possibly messing it up.  
  Don't copy it to the folder of the original file; this might confuse a running api server, for example.  
  The same is true for `k edit`: do a `k get XYZ -o yaml > backup.yaml` to see if you can find the YAML in the output.

In most of my killer.sh test exams I miss some points.  
Even though I solved things correctly or would have been able to, I wrote the result into a wrong file or missed to solve a subtask.  
Don't do that, be thorough!

### Don't give up

* You are going to need every point to can score and every second you're granted
* If some tasks might seem too difficult, flag them and tackle them again at the end.
  Some questions can be solved in a very short time, but might be at the end.
  Especially when you're planning to work sequentially through them (my strategy), you might miss easy points when you run out of time before arriving at the end.
* Having only one or two minutes left is enough time to score some points by partially solving some question
* You'll be surprised how fast you can learn to solve tasks under pressure
* If you don't know how to solve, start by reading the docs linked at the top. Do that at the end, when all other tasks are solved.
* You don't need to solve the whole question, partial solutions score points as well
* You never know, a partial solution saved in the last seconds might be the one point you need for the 66% passing score!
* I passed three of my exams with around 70%, because I never gave up 😊

Don't give up until you're kicked out of the exam 💪