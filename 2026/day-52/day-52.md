Task 1: Default Namespaces
kubectl get namespaces

 You’ll see:

default
kube-system
kube-public
kube-node-lease
 Check pods in kube-system
kubectl get pods -n kube-system

 Count them:

kubectl get pods -n kube-system | wc -l

 Answer:

The number of pods depends on the cluster, typically 6–10 pods (etcd, API server, scheduler, controller-manager, coredns, kube-proxy, etc.).

 Task 2: Custom Namespaces

After creating:

kubectl create namespace dev
kubectl create namespace staging
 Check pods
kubectl get pods

 You will NOT see nginx-dev or nginx-staging

kubectl get pods -A

 You WILL see them

 Answer:

kubectl get pods shows only the default namespace.
kubectl get pods -A shows pods from all namespaces including dev and staging.

 Task 3: Deployment Output Meaning
kubectl get deployments -n dev

Example:

NAME               READY   UP-TO-DATE   AVAILABLE
nginx-deployment   3/3     3            3
 Meaning:
READY (3/3)
 Running pods / desired pods
UP-TO-DATE (3)
 Pods updated to latest spec
AVAILABLE (3)
 Pods ready to serve traffic

 Answer:

READY shows how many pods are running successfully, UP-TO-DATE shows how many pods match the current configuration, and AVAILABLE shows how many pods are ready to serve traffic.

 Task 4: Self-Healing

After deleting a pod:

kubectl delete pod <pod-name> -n dev

 Kubernetes creates a new one automatically

 Answer:

The replacement pod has a different name because it is newly created by the Deployment controller.

 Task 5: Scaling
Scale up:
kubectl scale deployment nginx-deployment --replicas=5 -n dev
Scale down:
kubectl scale deployment nginx-deployment --replicas=2 -n dev

 Answer:

When scaling down from 5 to 2, Kubernetes terminates the extra 3 pods to match the desired state.

 Task 6: Rolling Update & Rollback
Update image:
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
Rollback:
kubectl rollout undo deployment/nginx-deployment -n dev
Verify:
kubectl describe deployment nginx-deployment -n dev | grep Image

 Answer:

After rollback, the image version returns to nginx:1.24.

 Task 7: Clean Up
kubectl delete deployment nginx-deployment -n dev
kubectl delete namespace dev staging production
Verify:
kubectl get pods -A

 No pods remaining

📌 Answer:

Yes, all resources are deleted. Deleting a namespace removes all resources inside it.
