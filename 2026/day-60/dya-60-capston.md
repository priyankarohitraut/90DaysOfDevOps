Task 1: Create Namespace

Create namespace:

kubectl create namespace capstone

Set default namespace:

kubectl config set-context --current --namespace=capstone

Verify:

kubectl config view --minify | grep namespace

Expected:

namespace: capstone
Task 2: Deploy MySQL
Create Secret

Save as mysql-secret.yaml

apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: capstone
type: Opaque
stringData:
  MYSQL_ROOT_PASSWORD: root123
  MYSQL_DATABASE: wordpress
  MYSQL_USER: wpuser
  MYSQL_PASSWORD: wp123

Apply:

kubectl apply -f mysql-secret.yaml
Create Headless Service

Save as mysql-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: mysql
  namespace: capstone
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
  - port: 3306

Apply:

kubectl apply -f mysql-service.yaml
Create StatefulSet

Save as mysql-statefulset.yaml

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  namespace: capstone
spec:
  serviceName: mysql
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        envFrom:
        - secretRef:
            name: mysql-secret
        ports:
        - containerPort: 3306
        resources:
          requests:
            cpu: 250m
            memory: 512Mi
          limits:
            cpu: 500m
            memory: 1Gi
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi

Apply:

kubectl apply -f mysql-statefulset.yaml
Verify Database
kubectl exec -it mysql-0 -- mysql -u wpuser -pwp123 -e "SHOW DATABASES;"

Expected:

wordpress
Verify Answer

Yes, I can see the wordpress database

Task 3: Deploy WordPress
ConfigMap

Save as wp-configmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: wordpress-config
  namespace: capstone
data:
  WORDPRESS_DB_HOST: mysql-0.mysql.capstone.svc.cluster.local:3306
  WORDPRESS_DB_NAME: wordpress

Apply:

kubectl apply -f wp-configmap.yaml
Deployment

Save as wordpress-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  namespace: capstone
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:latest
        envFrom:
        - configMapRef:
            name: wordpress-config
        env:
        - name: WORDPRESS_DB_USER
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: MYSQL_USER
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: MYSQL_PASSWORD
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 256Mi
          limits:
            cpu: 500m
            memory: 512Mi
        livenessProbe:
          httpGet:
            path: /wp-login.php
            port: 80
        readinessProbe:
          httpGet:
            path: /wp-login.php
            port: 80

Apply:

kubectl apply -f wordpress-deployment.yaml
Verify
kubectl get pods

Expected:

2/2 Running
Verify Answer

Yes, both WordPress pods are running and ready

Task 4: Expose WordPress

Save as wordpress-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: wordpress
  namespace: capstone
spec:
  type: NodePort
  selector:
    app: wordpress
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080

Apply:

kubectl apply -f wordpress-service.yaml
Access

For Minikube:

minikube service wordpress -n capstone

For Kind:

kubectl port-forward svc/wordpress 8080:80 -n capstone

Open:

http://localhost:8080
Verify Answer

Yes, WordPress setup page is visible

Task 5: Self-Healing + Persistence

Delete WordPress pod:

kubectl delete pod <wordpress-pod-name>

Delete MySQL pod:

kubectl delete pod mysql-0

Check recreation:

kubectl get pods -w
Verify Answer

Yes, after deleting both pods, the blog post is still there

This proves:

Deployment self-healing
StatefulSet persistence
PVC data retention
Task 6: HPA

Save as wp-hpa.yaml

apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: wordpress-hpa
  namespace: capstone
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: wordpress
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50

Apply:

kubectl apply -f wp-hpa.yaml

Check:

kubectl get hpa
Verify Answer

Yes, HPA shows min 2, max 10, target 50%

Task 7: Bonus Helm Compare

Use separate namespace:

kubectl create namespace wp-helm
helm install wp-helm bitnami/wordpress -n wp-helm

Compare resources:

kubectl get all -n wp-helm
kubectl get all -n capstone
Interview Ready Answer

Manual YAML gives more control, while Helm provides faster deployment and easier upgrades.

Excellent answer for interviews.

Task 8: Cleanup

Final check:

kubectl get all -n capstone

Delete everything:

kubectl delete namespace capstone
kubectl config set-context --current --namespace=default

Verify:

kubectl get all -n capstone
