Task 1: Resource Requests and Limits

Create resource-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: resource-pod
spec:
  containers:
    - name: nginx
      image: nginx
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "250m"
          memory: "256Mi"

Apply:

kubectl apply -f resource-pod.yaml

Inspect:

kubectl describe pod resource-pod

Look for:

Requests:
  cpu:     100m
  memory:  128Mi
Limits:
  cpu:     250m
  memory:  256Mi
QoS Class: Burstable
 Verify Answer
QoS Class = Burstable

Because:

requests ≠ limits
both are defined

Quick rule:

Guaranteed → requests = limits
Burstable → requests and/or limits set but not equal
BestEffort → neither set
 Task 2: OOMKilled — Exceed Memory Limit

Create oom-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: oom-pod
spec:
  containers:
    - name: stress
      image: polinux/stress
      command: ["stress"]
      args: ["--vm", "1", "--vm-bytes", "200M", "--vm-hang", "1"]
      resources:
        limits:
          memory: "100Mi"

Apply:

kubectl apply -f oom-pod.yaml

Check:

kubectl describe pod oom-pod

Look for:

Reason: OOMKilled
Exit Code: 137
 Verify Answer
Exit Code = 137

Important interview point:

137 = 128 + 9

Where 9 = SIGKILL

 Task 3: Pending Pod — Too Large Request

Create pending-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: huge-request-pod
spec:
  containers:
    - name: nginx
      image: nginx
      resources:
        requests:
          cpu: "100"
          memory: "128Gi"

Apply:

kubectl apply -f pending-pod.yaml

Check:

kubectl get pod huge-request-pod

Expected:

STATUS = Pending

Now describe:

kubectl describe pod huge-request-pod
 Verify Answer

Look in Events:

0/1 nodes are available: 1 Insufficient cpu, 1 Insufficient memory

That is the scheduler message.

 Task 4: Liveness Probe

Create liveness-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
spec:
  containers:
    - name: busybox
      image: busybox
      command:
        - sh
        - -c
        - |
          touch /tmp/healthy
          sleep 30
          rm -f /tmp/healthy
          sleep 600
      livenessProbe:
        exec:
          command:
            - cat
            - /tmp/healthy
        periodSeconds: 5
        failureThreshold: 3

Apply:

kubectl apply -f liveness-pod.yaml

Watch:

kubectl get pod liveness-pod -w

After ~45 sec, restart should happen.

Check restart count
kubectl get pod liveness-pod

Example:

RESTARTS = 1
 Verify Answer
Restart count = 1 (or more if you wait longer)
 Task 5: Readiness Probe

Create readiness-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: readiness-pod
spec:
  containers:
    - name: nginx
      image: nginx
      readinessProbe:
        httpGet:
          path: /
          port: 80
        periodSeconds: 5

Apply:

kubectl apply -f readiness-pod.yaml

Expose service:

kubectl expose pod readiness-pod --port=80 --name=readiness-svc

Check endpoints:

kubectl get endpoints readiness-svc
Break readiness
kubectl exec readiness-pod -- rm /usr/share/nginx/html/index.html

Wait 15 sec.

Check:

kubectl get pod readiness-pod
kubectl get endpoints readiness-svc

Expected:

READY = 0/1
endpoints = <none>
 Verify Answer
No, container is NOT restarted

Very important difference:

liveness = restart
readiness = remove from traffic
 Task 6: Startup Probe

Create startup-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: startup-pod
spec:
  containers:
    - name: busybox
      image: busybox
      command:
        - sh
        - -c
        - |
          sleep 20
          touch /tmp/started
          sleep 600
      startupProbe:
        exec:
          command: ["cat", "/tmp/started"]
        periodSeconds: 5
        failureThreshold: 12
      livenessProbe:
        exec:
          command: ["cat", "/tmp/started"]
        periodSeconds: 5

Apply:

kubectl apply -f startup-pod.yaml
 Verify Answer

If failureThreshold: 2

Then total startup time budget:

2 × 5 = 10 seconds

But app starts in 20 sec.

So:

Container will restart before startup completes

This is a very common interview question.

 Task 7: Clean Up
kubectl delete pod resource-pod
kubectl delete pod oom-pod
kubectl delete pod huge-request-pod
kubectl delete pod liveness-pod
kubectl delete pod readiness-pod
kubectl delete pod startup-pod
kubectl delete svc readiness-svc
