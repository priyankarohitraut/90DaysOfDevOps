Task 1: Create a ConfigMap from Literals

Run this:

kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080
Inspect ConfigMap
kubectl describe configmap app-config
kubectl get configmap app-config -o yaml

Expected output:

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  APP_DEBUG: "false"
  APP_PORT: "8080"
Verify

Yes, you should clearly see:

APP_ENV=production
APP_DEBUG=false
APP_PORT=8080

Stored as plain text, no encoding.

Task 2: Create ConfigMap from File

Create file:

nano default.conf

Paste this:

server {
    listen 80;

    location /health {
        return 200 'healthy';
        add_header Content-Type text/plain;
    }
}

Save.

Create ConfigMap
kubectl create configmap nginx-config --from-file=default.conf=default.conf
Verify
kubectl get configmap nginx-config -o yaml

Expected:

data:
  default.conf: |
    server {
        listen 80;
        location /health {
            return 200 'healthy';
            add_header Content-Type text/plain;
        }
    }

Yes, file content should be visible.

Task 3: Use ConfigMaps in a Pod
Pod 1 → Environment Variables

Create file busybox-config-pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: busybox-config
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "env | grep APP && sleep 3600"]
    envFrom:
    - configMapRef:
        name: app-config

Apply:

kubectl apply -f busybox-config-pod.yml

Check logs:

kubectl logs busybox-config

Expected:

APP_ENV=production
APP_DEBUG=false
APP_PORT=8080
Pod 2 → Mount Config File

Create nginx-config-pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: nginx-config-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: nginx-config-volume
      mountPath: /etc/nginx/conf.d
  volumes:
  - name: nginx-config-volume
    configMap:
      name: nginx-config

Apply:

kubectl apply -f nginx-config-pod.yml
Test Health Endpoint
kubectl exec nginx-config-pod -- curl -s http://localhost/health

Expected:

healthy

 Yes, /health should respond.

Task 4: Create a Secret

Run:

kubectl create secret generic db-credentials \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASSWORD='s3cureP@ssw0rd'
Inspect Secret
kubectl get secret db-credentials -o yaml

Expected:

data:
  DB_PASSWORD: czNjdXJlUEBzc3cwcmQ=
  DB_USER: YWRtaW4=

This is base64 encoded.

Decode Password
echo 'czNjdXJlUEBzc3cwcmQ=' | base64 --decode

Expected:

s3cureP@ssw0rd

 Yes, password should decode to plaintext.

Task 5: Use Secrets in a Pod

Create secret-pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: DB_USER
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/db-credentials
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: db-credentials

Apply:

kubectl apply -f secret-pod.yml
Verify Mounted Files
kubectl exec secret-pod -- ls /etc/db-credentials

Expected:

DB_PASSWORD
DB_USER

Read file:

kubectl exec secret-pod -- cat /etc/db-credentials/DB_PASSWORD

Expected:

s3cureP@ssw0rd

 Mounted file values are plaintext, not base64.

Very important interview point.

Task 6: Update ConfigMap and Observe Propagation

Create ConfigMap:

kubectl create configmap live-config --from-literal=message=hello
Pod Manifest

Create live-config-pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: live-config-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command:
      - sh
      - -c
      - |
        while true; do
          cat /config/message
          sleep 5
        done
    volumeMounts:
    - name: config-volume
      mountPath: /config
  volumes:
  - name: config-volume
    configMap:
      name: live-config

Apply:

kubectl apply -f live-config-pod.yml

Check logs:

kubectl logs -f live-config-pod

Expected:

hello
hello
hello
Update ConfigMap
kubectl patch configmap live-config --type merge -p '{"data":{"message":"world"}}'

Wait 30–60 sec.

Logs should change automatically:

world
world
world

 Yes, volume updates without pod restart.

IMPORTANT INTERVIEW QUESTION

Do environment variables update automatically?

Answer:

No

Only volume-mounted ConfigMaps auto-refresh.

Task 7: Clean Up
kubectl delete pod busybox-config nginx-config-pod secret-pod live-config-pod
kubectl delete configmap app-config nginx-config live-config
kubectl delete secret db-credentials
SUPER IMPORTANT INTERVIEW ANSWER

They often ask:

ConfigMap vs Secret?

Best answer:

ConfigMap stores non-sensitive configuration data.
Secret stores sensitive data like passwords, API keys, tokens.
Secrets are base64 encoded and support RBAC + encryption at rest.
