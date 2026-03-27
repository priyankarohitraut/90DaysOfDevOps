Task 1: Deployment Verification
kubectl apply -f app-deployment.yaml
kubectl get pods -o wide

 Expected:

3 Pods in Running state 
Each pod has a different IP

Example:

web-app-xxxxx   Running   10.244.0.5
web-app-yyyyy   Running   10.244.0.6
web-app-zzzzz   Running   10.244.0.7

 Answer:

Yes, all 3 pods are running. Each pod has a unique IP address, which can change if the pod is recreated.

 Task 2: ClusterIP Service
kubectl apply -f clusterip-service.yaml
kubectl get services

 You’ll see:

web-app-clusterip   ClusterIP   10.x.x.x
 Test from inside cluster
kubectl run test-client --image=busybox:latest --rm -it --restart=Never -- sh

Inside:

wget -qO- http://web-app-clusterip

 Output:

Nginx welcome page 

 Answer:

Yes, the Service responds successfully. When running the request multiple times, traffic is load-balanced across all healthy pods.

 Task 3: DNS Verification

Inside test pod:

nslookup web-app-clusterip

 Output:

Name: web-app-clusterip
Address: 10.x.x.x

 Answer:

The IP returned by nslookup matches the ClusterIP shown in kubectl get services.

 Task 4: NodePort Service
kubectl apply -f nodeport-service.yaml
kubectl get services

 You’ll see:

web-app-nodeport   NodePort   80:30080/TCP
 Access

For your setup (Kind / local):

curl http://localhost:30080

 Output:

Nginx welcome page 

 Answer:

Yes, I can access the Nginx welcome page using NodePort via localhost:30080.

 Task 5: LoadBalancer Service
kubectl apply -f loadbalancer-service.yaml
kubectl get services

 You’ll see:

web-app-loadbalancer   LoadBalancer   <pending>

 Answer:

The EXTERNAL-IP shows <pending> because a local cluster (Kind/Minikube) does not have a cloud provider to provision a real load balancer.

 Task 6: Service Types Comparison
kubectl describe service web-app-loadbalancer

 You’ll see:

ClusterIP 
NodePort 
LoadBalancer config 

 Answer:

Yes, the LoadBalancer service also has a ClusterIP and NodePort assigned, as it builds on top of both.

 Concept Summary (Very Important)
Type	Access	Use
ClusterIP	Inside cluster	Internal communication
NodePort	NodeIP:Port	Testing / Dev
LoadBalancer	Public IP	Production
 Task 7: Clean Up
kubectl delete -f app-deployment.yaml
kubectl delete -f clusterip-service.yaml
kubectl delete -f nodeport-service.yaml
kubectl delete -f loadbalancer-service.yaml
Verify:
kubectl get pods
kubectl get services

 Only:

kubernetes   ClusterIP

 Final Answer:

Yes, all resources are cleaned up. Only the default kubernetes service remains.
