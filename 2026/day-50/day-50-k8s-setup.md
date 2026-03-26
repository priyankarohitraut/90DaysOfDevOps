1. Why was Kubernetes created? What problem does it solve beyond Docker?

Docker helps us create and run containers, but it has limitations:

No built-in way to manage many containers across multiple machines
No automatic scaling
No self-healing (if a container crashes, Docker alone won’t restart intelligently across nodes)
No load balancing
No proper orchestration

 Kubernetes was created to solve these problems by providing:

Container orchestration
Auto-scaling
Self-healing
Load balancing
Rolling updates & rollbacks

 In short:
Docker = run containers
Kubernetes = manage containers at scale (production level)

 2. Who created Kubernetes and what was it inspired by?
Kubernetes was created by Google
It was inspired by Google’s internal system called Borg

 Google was already running containers at massive scale internally, and Borg helped manage them.

 Kubernetes is basically:

“An open-source version of Google’s internal container management system”

3. What does "Kubernetes" mean?
The word Kubernetes comes from Greek
It means:
 "Helmsman" or "Ship Pilot"

 That’s why the logo is a ship wheel ⚓

 Idea:
Just like a pilot controls a ship, Kubernetes controls and manages containers



TASK--2
 1. Control Plane (Master Node)

Think of this as the brain of Kubernetes

                [ kubectl ]
                     |
                     v
               [ API Server ]
                     |
     ---------------------------------
     |               |               |
 [etcd]        [Scheduler]   [Controller Manager]
Components:
API Server
Entry point of cluster
All commands (kubectl, UI, API calls) come here
etcd
Key-value database
Stores full cluster state (pods, nodes, configs)
Scheduler
Decides which worker node will run a pod
Controller Manager
Keeps checking:
 “Actual state == Desired state?”
If not → fixes it (e.g., restarts pods)
 2. Worker Node

Think of this as the muscle (execution layer)

         [ Worker Node ]
        ------------------
        |    kubelet     |
        |  kube-proxy    |
        | Container Run  |
        ------------------
Components:
kubelet
Talks to API Server
Ensures containers are running as expected
kube-proxy
Handles networking
Enables pod-to-pod communication
Container Runtime
Runs containers
Examples: containerd, CRI-O
 What happens when you run kubectl apply -f pod.yaml?

Step-by-step flow 

kubectl → API Server
You send request to create pod
API Server → etcd
Stores desired state (new pod definition)
API Server → Scheduler
Scheduler checks:
 Which node has enough resources?
Scheduler → API Server
Updates decision (assigns node)
API Server → kubelet (on chosen node)
kubelet receives instruction:
 “Run this pod”
kubelet → Container Runtime
Pulls image and starts container
Controller Manager
Continuously monitors:
 If pod crashes → recreate


BUT:

 Already running pods keep running
 Cluster becomes read-only / frozen state
 What if a Worker Node goes down?
Controller Manager detects failure
Marks node as NotReady
Pods on that node are considered lost
Scheduler creates new pods on other nodes

 Result:
 Kubernetes self-heals automatically


TASK--3
Install kubectl (Linux – amd64)

Run these commands one by one:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
Verify Installation
kubectl version --client

 Expected output (example):

Client Version: v1.xx.x
Kustomize Version: v5.x.x
 Common Issues (Important for you)
 1. Permission denied
chmod +x kubectl
 2. Command not found

 Check path:

echo $PATH

 Ensure /usr/local/bin is included

 3. Wrong file downloaded (you faced similar before )

If you see:

syntax error near unexpected token `<'

 That means HTML got downloaded instead of binary

✔ Fix:

rm kubectl

Then re-download properly

 Extra Verification (Optional but Good)
kubectl version --client --output=yaml
 Important Concept
kubectl = client tool
It talks to API Server in Kubernetes

 Without cluster:

You can install kubectl 
But cluster commands won’t work 

TASK-4
My Choice: kind (Kubernetes in Docker)
 Why I chose kind
 Uses Docker containers as nodes (lightweight)
 Very fast to create/delete clusters
 Perfect for CI/CD + DevOps practice
 No VM required (unlike minikube sometimes)
 Matches real-world scenarios (used in pipelines)

 Simple reason you can write:

I chose kind because it is lightweight, fast, and uses Docker containers as nodes, making it ideal for local development and CI/CD practice.

 Step-by-Step Setup (Linux)
1️⃣ Install kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
2️⃣ Create Cluster
kind create cluster --name devops-cluster

 This will:

Pull node image
Create control-plane container
Configure kubectl automatically
3️3  Verify Cluster
kubectl cluster-info
kubectl get nodes

 Expected output:

NAME                     STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane   Ready    control-plane   1m    v1.xx.x
 Common Errors (Important for you)
 Segmentation fault (you faced earlier)

 Usually happens due to:

Broken kubectl
Wrong binary

✔ Fix:

kubectl version --client

If error → reinstall kubectl

 Docker not running
docker ps

 If error → start Docker:

sudo systemctl start docker
 Extra Check (Very Important)
kubectl get pods -A

 You should see:

kube-apiserver
etcd
coredns

TASK--5
Run this first
kubectl get pods -n kube-system

 You’ll see something like:

etcd-devops-cluster-control-plane
kube-apiserver-devops-cluster-control-plane
kube-controller-manager-devops-cluster-control-plane
kube-scheduler-devops-cluster-control-plane
coredns-xxxxx
kube-proxy-xxxxx

 Match Pods → Architecture Components

 Control Plane (Master Node)
Pod Name	Component	Role
kube-apiserver-*	API Server	Entry point for all commands
etcd-*	etcd	Stores cluster state
kube-scheduler-*	Scheduler	Assigns pods to nodes
kube-controller-manager-*	Controller Manager	Maintains desired state

 These 4 = Brain of Kubernetes

 Worker Node Components
Pod Name	Component	Role
kube-proxy-*	kube-proxy	Networking (pod communication)

 Runs on every node

Extra System Pods
Pod Name	Purpose
coredns-*	DNS inside cluster (service discovery)

 Example:
Pods talk using names like:

my-service.default.svc.cluster.local
 Important Realization

 In Kubernetes:

Even core components run as pods inside the cluster

This is why Kubernetes is called self-hosted / self-managed syste

API Server → kube-apiserver-* 
etcd → etcd-* 
Scheduler → kube-scheduler-* 
Controller → kube-controller-manager-* 
Networking → kube-proxy-* 
DNS → coredns-* 


TASK--6
What is a kubeconfig?

 kubeconfig is a configuration file used by kubectl to connect to a Kubernetes cluster

It contains:

 Cluster details (API server endpoint)
 User credentials (auth info)
 Contexts (which user + which cluster)
 Current active context

 Simple definition (write this ):

kubeconfig is a file that tells kubectl which cluster to connect to, which user to use, and how to authenticate.

 Where is kubeconfig stored?
Default location:
~/.kube/config

 Example:

/home/rohit/.kube/config
 What’s inside kubeconfig? (Conceptual)
clusters:
users:
contexts:
current-context:
 Breakdown:
cluster
→ API server URL
user
→ credentials (cert/token)
context
→ combination of cluster + user
current-context
→ which cluster you are using now


 Check current cluster
kubectl config current-context

 Output example:

kind-devops-cluster
 List all clusters
kubectl config get-contexts
 View full config
kubectl config view
 Real Understanding (Important)

When you run:

kubectl get nodes

 kubectl:

Reads ~/.kube/config
Finds current context
Connects to API Server
Fetches data

