Task 1: Nginx Pod – Expected Results

After:

kubectl apply -f nginx-pod.yaml
✔ Verify:
kubectl get pods

 You should see:

nginx-pod   Running
 Inside the Pod
kubectl exec -it nginx-pod -- /bin/bash

Then:

curl localhost:80

 Expected Output:

HTML of Nginx Welcome Page

 Final Answer:

Yes, I can see the Nginx welcome page when curling localhost inside the pod.

Task 2: BusyBox Pod – Key Learning
kubectl logs busybox-pod

 Output:

Hello from BusyBox

 Final Answer:

Yes, I can see "Hello from BusyBox" in the logs.

 Important Concept
Nginx = long-running server 
BusyBox = short task  (exits immediately)

 That’s why we use:

command: ["sh", "-c", "echo Hello from BusyBox && sleep 3600"]
 Task 3: Imperative vs Declarative
🟢 Imperative
kubectl run redis-pod --image=redis:latest
Quick
No YAML
Not ideal for production
🔵 Declarative
kubectl apply -f nginx-pod.yaml
Uses YAML
Version control friendly
Used in real DevOps
 What you’ll notice in generated YAML:

When you run:

kubectl get pod redis-pod -o yaml

 Extra fields:

status
uid
resourceVersion
creationTimestamp

 Final Answer:

Kubernetes automatically adds system-generated metadata like UID, timestamps, and status, which are not present in manually written manifests.


 Task 4: Dry Run Validation
 If you remove image field
kubectl apply -f nginx-pod.yaml --dry-run=client

 Error:

error: error validating data: ValidationError(Pod.spec.containers[0]): missing required field "image"

 Final Answer:

Kubernetes throws a validation error saying the "image" field is required.


 Task 5: Labels Practice
 View labels:
kubectl get pods --show-labels
 Filter:
kubectl get pods -l app=nginx
kubectl get pods -l environment=dev
 Third Pod (Write this )
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    environment: staging
    team: devops
spec:
  containers:
  - name: myapp
    image: nginx:latest
    
 Task 6: Clean Up
kubectl delete pod nginx-pod
kubectl delete pod busybox-pod
kubectl delete pod redis-pod

 Important Learning

Standalone Pods are NOT self-healing. Once deleted, they are gone permanently.

 What to Write in day-51-pods.md
 1. Four Required Fields in Manifest
apiVersion   → API version (v1, apps/v1)
kind         → Type of resource (Pod, Deployment)
metadata     → Name, labels
spec         → Desired state (containers, image, ports)
 2. Difference: Imperative vs Declarative
Imperative	Declarative
kubectl run	kubectl apply -f
Quick	Structured
Not reusable	Version controlled
Not production-ready	Production standard
 3. What happens when Pod is deleted?

The pod is permanently deleted and not recreated because no controller manages it.

 Final DevOps Insight

 Real-world:

 We don’t use Pods directly
 We use Deployments (next step
