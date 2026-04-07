Task 1: Install Helm

Helm is the package manager for Kubernetes.

Three important concepts:

Chart → package of Kubernetes manifests
Release → installed instance of a chart
Repository → collection of charts
Install on Linux / macOS

Using curl:

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
Verify Installation
helm version
helm env

Expected output example:

version.BuildInfo{Version:"v3.16.2"}
Verify Answer

Question: What version of Helm is installed?

Example:

Installed Helm version is v3.x

Use your exact output in interview/lab.

Task 2: Add Repository and Search

Add Bitnami repo:

helm repo add bitnami https://charts.bitnami.com/bitnami

Update:

helm repo update

Search charts:

helm search repo nginx
helm search repo bitnami
Count Charts

To count total charts:

helm search repo bitnami | wc -l

Expected:
Bitnami usually has hundreds of charts.

Verify Answer

Example:

Bitnami repository currently has 300+ charts

Exact number may vary.

Task 3: Install a Chart

Install NGINX chart:

helm install my-nginx bitnami/nginx
Check Created Resources
kubectl get all

Inspect release:

helm list
helm status my-nginx
helm get manifest my-nginx
Verify Pod Count
kubectl get pods

Usually:

1 Pod
Verify Service Type
kubectl get svc

Expected default:

ClusterIP
Verify Answer

1 Pod is running and the Service type is ClusterIP

Task 4: Customize with Values

Check default values:

helm show values bitnami/nginx
Install Using --set
helm install custom-nginx bitnami/nginx \
  --set replicaCount=3 \
  --set service.type=NodePort
Create Values File

Create custom-values.yaml

replicaCount: 3

service:
  type: NodePort

resources:
  limits:
    cpu: "500m"
    memory: "512Mi"

Install:

helm install file-nginx bitnami/nginx -f custom-values.yaml
Check Values
helm get values file-nginx
kubectl get pods
kubectl get svc
Verify Answer

Yes, the release has 3 replicas and Service type is NodePort

Task 5: Upgrade and Rollback

Upgrade:

helm upgrade my-nginx bitnami/nginx --set replicaCount=5

Check history:

helm history my-nginx

Expected:

REVISION 1 deployed
REVISION 2 upgraded
Rollback
helm rollback my-nginx 1

Check history again:

helm history my-nginx

Expected:

REVISION 1 deployed
REVISION 2 upgraded
REVISION 3 superseded
Verify Answer

After rollback, total revisions are 3

Very important interview point:
rollback creates new revision, not overwrite.

Task 6: Create Your Own Chart

Scaffold chart:

helm create my-app
Explore Structure
ls my-app

Important files:

Chart.yaml
values.yaml
templates/deployment.yaml
Edit values.yaml
replicaCount: 3

image:
  repository: nginx
  tag: "1.25"
Validate
helm lint my-app
Preview Templates
helm template my-release ./my-app

This shows rendered Kubernetes YAML.

Install Chart
helm install my-release ./my-app

Check replicas:

kubectl get deploy

Expected:

3 replicas
Upgrade
helm upgrade my-release ./my-app --set replicaCount=5

Check again:

kubectl get deploy

Expected:

5 replicas
Verify Answer

After install → 3 replicas
After upgrade → 5 replicas

Task 7: Clean Up

Remove releases:

helm uninstall my-nginx
helm uninstall custom-nginx
helm uninstall file-nginx
helm uninstall my-release

Remove local files:

rm -rf my-app custom-values.yaml
Verify
helm list
