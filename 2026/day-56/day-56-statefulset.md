Task 1: Understand the Problem (Deployment vs StatefulSet)

First create a normal Deployment.

Create nginx-deploy.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx

Apply:

kubectl apply -f nginx-deploy.yaml
kubectl get pods

Expected pod names:

web-deploy-6d4d9f7c7f-abc12
web-deploy-6d4d9f7c7f-def34
web-deploy-6d4d9f7c7f-ghi56

Notice the random suffixes.

Delete one pod
kubectl delete pod <one-pod-name>
kubectl get pods

New pod example:

web-deploy-6d4d9f7c7f-xyz99

Different random name.

Verification answer

Why is this bad for a database cluster?

Because databases need:

fixed node identity
predictable peer names
stable replication endpoints

Example:

mysql-0
mysql-1
mysql-2

If names keep changing, replication and cluster membership breaks.

This is why Deployments are bad for DB clusters.

Delete deployment
kubectl delete deployment web-deploy
Task 2: Create a Headless Service

Create web-headless.yaml

apiVersion: v1
kind: Service
metadata:
  name: web-headless
spec:
  clusterIP: None
  selector:
    app: web
  ports:
    - port: 80
      name: http

Apply:

kubectl apply -f web-headless.yaml
kubectl get svc

Expected:

NAME           TYPE        CLUSTER-IP   PORT(S)
web-headless   ClusterIP   None         80/TCP
Verification answer

 CLUSTER-IP = None

This is exactly correct.

A headless service gives each pod its own DNS record.

Task 3: Create a StatefulSet

Create web-statefulset.yaml

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: web-headless
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx
          volumeMounts:
            - name: web-data
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: web-data
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 100Mi

Apply:

kubectl apply -f web-statefulset.yaml
kubectl get pods -l app=web -w

Watch the ordered startup:

web-0
web-1
web-2

in sequence.

Check PVCs
kubectl get pvc

Expected:

web-data-web-0
web-data-web-1
web-data-web-2
Verification answer

Exact pod names:

web-0
web-1
web-2

PVC names:

web-data-web-0
web-data-web-1
web-data-web-2

Pattern:

<template-name>-<pod-name>

Perfect.

Task 4: Stable Network Identity

Run test pod:

kubectl run dns-test --image=busybox -it --rm --restart=Never -- sh

Inside pod:

nslookup web-0.web-headless.default.svc.cluster.local
nslookup web-1.web-headless.default.svc.cluster.local
nslookup web-2.web-headless.default.svc.cluster.local
Compare IPs

In another terminal:

kubectl get pods -o wide

Check pod IPs.

Verification answer

 Yes, nslookup IP should match pod IP.

This proves stable DNS identity.

Task 5: Stable Storage — Data Survives

Write data:

kubectl exec web-0 -- sh -c "echo 'Data from web-0' > /usr/share/nginx/html/index.html"

Check:

kubectl exec web-0 -- cat /usr/share/nginx/html/index.html

Expected:

Data from web-0
Delete pod
kubectl delete pod web-0

Wait for recreation.

Check again:

kubectl exec web-0 -- cat /usr/share/nginx/html/index.html

Expected:

Data from web-0
Verification answer

 Yes, data is identical after recreation

Because same PVC reattaches automatically.

Task 6: Ordered Scaling

Scale up:

kubectl scale statefulset web --replicas=5
kubectl get pods -w

Observe:

web-3
web-4

created in order.

Scale down
kubectl scale statefulset web --replicas=3

Pods terminate in reverse order:

web-4
web-3
Check PVCs
kubectl get pvc

Expected:

web-data-web-0
web-data-web-1
web-data-web-2
web-data-web-3
web-data-web-4
Verification answer

 After scale down, 5 PVCs still exist

Very important interview point:

StatefulSet keeps PVCs for safety.

Task 7: Clean Up

Delete StatefulSet and service:

kubectl delete statefulset web
kubectl delete svc web-headless

Check PVCs:

kubectl get pvc

They still remain.

Delete manually:

kubectl delete pvc --all
Verification answer

 PVCs are NOT auto-deleted with StatefulSet

This is a safety feature to protect data.

Super Important Interview Answer

Deployment vs StatefulSet

Feature	Deployment	StatefulSet
Pod name	Random	Stable
Storage	Shared/ephemeral	Dedicated per pod
DNS	Random	Stable
Startup	Parallel	Ordered

