## create all dashboard resources
- kubectl apply -f https://k8smastery.com/insecure-dashboard.yaml
- this only results in 404 error when I try to access the localhost at node port
- I have no idea what causes the failure

## I've tried official one
- https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
- kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
    ```
    namespace/kubernetes-dashboard created
    serviceaccount/kubernetes-dashboard created
    service/kubernetes-dashboard created
    secret/kubernetes-dashboard-certs created
    secret/kubernetes-dashboard-csrf created
    secret/kubernetes-dashboard-key-holder created
    configmap/kubernetes-dashboard-settings created
    role.rbac.authorization.k8s.io/kubernetes-dashboard created
    clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
    rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
    clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
    deployment.apps/kubernetes-dashboard created
    service/dashboard-metrics-scraper created
    deployment.apps/dashboard-metrics-scraper created
    ```
- access the ui
    - use proxy
    ```
    kubectl proxy
    ```
    - use the address
    ```
    http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
    ```

- Create the dashboard service account
    ```
    kubectl create serviceaccount dashboard-admin-sa
    ```

- bind the dashboard-admin-service-account service account to the cluster-admin role
    ```
    kubectl create clusterrolebinding dashboard-admin-sa --clusterrole=cluster-admin --serviceaccount=default:dashboard-admin-sa
    ```
- Use kubectl describe to get the access token
    ```
    kubectl get secrets
    ```
    ```
    kubectl describe secret dashboard-admin-sa-token-zwjfk
    ```
-Copy the token and enter it into the token field on the Kubernetes dashboard login page.

## Using microk8s dashboard
- https://microk8s.io/docs/addon-dashboard
- enable addon
    ```
    microk8s enable dashboard
    ```
- get access token
    ```
    token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
    microk8s kubectl -n kube-system describe secret $token
    ```
- forwarding port
    ```
    microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443
    ```
- access
    ```
    https://127.0.0.1:10443
    ```
- I have no idea, this is not working