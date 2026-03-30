Task 1: See the Problem — Data Lost on Pod Deletion

This shows why normal Pod storage is temporary.

Create file emptydir-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: emptydir-pod
spec:
  containers:
    - name: busybox
      image: busybox
      command:
        - sh
        - -c
        - |
          date > /data/message.txt
          echo "Hello from Pod" >> /data/message.txt
          sleep 3600
      volumeMounts:
        - name: temp-storage
          mountPath: /data
  volumes:
    - name: temp-storage
      emptyDir: {}

Apply it:

kubectl apply -f emptydir-pod.yaml
Verify data
kubectl exec emptydir-pod -- cat /data/message.txt

Example output:

Mon Mar 30 12:30:00 UTC 2026
Hello from Pod
Delete and recreate
kubectl delete pod emptydir-pod
kubectl apply -f emptydir-pod.yaml

Check again:

kubectl exec emptydir-pod -- cat /data/message.txt
Verify answer

 Timestamp will be different

Because emptyDir exists only while Pod is running.

Once Pod is deleted → data is lost.

This is a very common interview question.

Task 2: Create a PersistentVolume (Static Provisioning)

Create pv.yaml

apiVersion: v1
kind: PersistentVolume
metadata:
  name: app-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /tmp/k8s-pv-data

Apply:

kubectl apply -f pv.yaml

Check:

kubectl get pv

Expected:

NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      ...
app-pv   1Gi        RWO            Retain          Available
Verify answer

 STATUS = Available

This means PV is created but not yet claimed.

Task 3: Create a PersistentVolumeClaim

Create pvc.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi

Apply:

kubectl apply -f pvc.yaml

Check:

kubectl get pvc
kubectl get pv

Expected:

PVC STATUS = Bound
PV STATUS = Bound
Verify answer

Run:

kubectl get pvc

Expected:

NAME      STATUS   VOLUME
app-pvc   Bound    app-pv

 VOLUME column = app-pv

This shows PVC is attached to the manual PV.

Task 4: Use PVC in a Pod — Data Survives

Create pv-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  containers:
    - name: busybox
      image: busybox
      command:
        - sh
        - -c
        - |
          echo "Data from first pod" >> /data/message.txt
          sleep 3600
      volumeMounts:
        - mountPath: /data
          name: storage
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: app-pvc

Apply:

kubectl apply -f pv-pod.yaml

Check:

kubectl exec pv-pod -- cat /data/message.txt
Recreate pod
kubectl delete pod pv-pod
kubectl apply -f pv-pod.yaml

Add second message:

kubectl exec pv-pod -- sh -c "echo 'Data from second pod' >> /data/message.txt"

Check file:

kubectl exec pv-pod -- cat /data/message.txt

Expected:

Data from first pod
Data from second pod
Verify answer

 Yes, file contains data from both Pods

This proves persistence works.

Task 5: StorageClasses and Dynamic Provisioning

Run:

kubectl get storageclass
kubectl describe storageclass

Example output in kind/minikube:

NAME                 PROVISIONER
standard (default)   rancher.io/local-path

or

standard (default)
Verify answer

Default StorageClass is usually:

standard

(Depends on cluster type)

Task 6: Dynamic Provisioning

Create dynamic-pvc.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: dynamic-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  resources:
    requests:
      storage: 500Mi

Apply:

kubectl apply -f dynamic-pvc.yaml

Check:

kubectl get pv
kubectl get pvc

Now a new PV should auto-create.

Verify answer

Now total PVs = 2

Example:

app-pv           → manual
pvc-xxxx-xxxx    → dynamic

Manual:

app-pv

Dynamic:

pvc-<random-id>
Task 7: Clean Up

Delete pods first:

kubectl delete pod emptydir-pod pv-pod

Delete PVCs:

kubectl delete pvc app-pvc dynamic-pvc

Check PVs:

kubectl get pv

Expected:

dynamic PV → deleted automatically
manual PV → Released
Verify answer

 Auto-deleted → dynamic PV

Because reclaim policy = Delete

 Retained → manual PV

Because reclaim policy = Retain

This is extremely important for interviews.
