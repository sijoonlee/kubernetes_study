## Deploying NGINX controller

### for Docker Desktop, create Service with LoadBalancer
kubectl apply -f https://k8smastery.com/ic-nginx-lb.yaml

### for minikube/MicroK8s, create Service with hostNetwork
kubectl apply -f https://k8smastery.com/ic-nginx-hn.yaml
