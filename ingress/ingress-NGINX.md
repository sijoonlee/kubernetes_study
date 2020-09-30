## Ingress in action: NGINX
- Plan
    - Ingress controller: NGINX 
    - DNS: nip.io
        - *.127.0.0.1.nip.io resolves to 127.0.0.1
        - so we can use various FQDN's without editing our hosts file
    - deploy pods listening on port 80
        - will use **hostNetwork** mode
        - if loadbalancer is available, we don't need **hostNetwork**

## What's **hostNetwork**
- without hostNetwork
    - normally, each pod gets its own network namespace
    - an IP address is assigned to the pod
    - this IP address is routed/connected to the cluster network
    - all containers of that pod are sharing that network namespace
    - therefore, they share the same IP address
- hostNetwork = true
    - no network namespace is created
    - The pod uses the network namespace of the host
    - The pod can receive traffic from outside direclty on any port
    - Downside - network polices won't work for the pod

## MicroK8s
- has a built-in NGINX installer: microk8s.enable ingress
- but we'll use YAML for learning purpose
- hostNetwork: true

## Deploying the NGINX Ingress controller
- NGINX Ingress Controller installation guide
    - https://kubernetes.github.io/ingress-nginx/deploy/
- The two main sections in the YAML are:
    - NGINX Deployment (or DaemonSet) and all its required resources
        - Namespace
        - ConfigMaps (storing NGINX configs)
        - ServiceAccount (authenticate to Kubernetes API)
        - Role/ClusterRole/RoleBindings (authorization to API parts)
        - LimitRange (limit cpu/memory of NGINX)
    - Service to expose NGINX on 80/443
        - different for each Kubernetes distribution
- for minikube/MicroK8s, create Service with hostNetwork
    ```
    kubectl apply -f https://k8smastery.com/ic-nginx-hn.yaml
    ```
- check the pod
    ```
    kubectl describe -n ingress-nginx deploy/nginx-ingress-controller
    ```
- try below address on browser
    127.0.0.1
    cheddar.127.0.0.1.nip.io

### Setting up host-based routing ingress rules
- we will route <name-of-cheese>.127.0.0.1.nip.io to the corresponding deployment
    - 3 images of cheese (errm/cheese:cheddar, errm/cheese:stiltion, errm/cheese:wensleydale)
    - 3 deployments, 3 services, 3 ingress rules
- run 3 deployments
    ```
    kubectl create deployment cheddar --image=errm/cheese:cheddar
    kubectl create deployment stilton --image=errm/cheese:stilton
    kubectl create deployment wensleydale --image=errm/cheese:wensleydale
    ```
- create 3 servcies
    ```
    kubectl expose deployment cheddar --port=80
    kubectl expose deployment stilton --port=80
    kubectl expose deployment wensleydale --port=80
    ```
- kubectl apply -f ingress.yaml
    ```
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    name: cheddar
    spec:
    rules:
    - host: cheddar.A.B.C.D.nip.io
        http:
        paths:
        - path: /
            backend:
            serviceName: cheddar
            servicePort: 80
    ---
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    name: stilton
    spec:
    rules:
    - host: stilton.A.B.C.D.nip.io
        http:
        paths:
        - path: /
            backend:
            serviceName: stilton
            servicePort: 80
    ---
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    name: wensleydale
    spec:
    rules:
    - host: wensleydale.A.B.C.D.nip.io
        http:
        paths:
        - path: /
            backend:
            serviceName: wensleydale
            servicePort: 80
    ---

    ```
- kubectl describe ingress cheddar
    ```
    Name:             cheddar
    Namespace:        default
    Address:          10.152.183.163  ------------------------------> this is from status (see next command)
    Default backend:  default-http-backend:80 (<none>)
    Rules:
    Host                      Path  Backends
    ----                      ----  --------
    cheddar.127.0.0.1.nip.io  
                                /   cheddar:80 (10.1.28.132:80)
    Annotations:
    kubectl.kubernetes.io/last-applied-configuration:  {"apiVersion":"networking.k8s.io/v1beta1","kind":"Ingress","metadata":{"annotations":{},"name":"cheddar","namespace":"default"},"spec":{"rules":[{"host":"cheddar.127.0.0.1.nip.io","http":{"paths":[{"backend":{"serviceName":"cheddar","servicePort":80},"path":"/"}]}}]}}

    Events:
    Type    Reason  Age   From                      Message
    ----    ------  ----  ----                      -------
    Normal  CREATE  52m   nginx-ingress-controller  Ingress default/cheddar
    Normal  UPDATE  51m   nginx-ingress-controller  Ingress default/cheddar
    ```

- kubectl get ingress/stilton -o yaml
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
    annotations:
        kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"networking.k8s.io/v1beta1","kind":"Ingress","metadata":{"annotations":{},"name":"cheddar","namespace":"default"},"spec":{"rules":[{"host":"cheddar.127.0.0.1.nip.io","http":{"paths":[{"backend":{"serviceName":"cheddar","servicePort":80},"path":"/"}]}}]}}
    creationTimestamp: "2020-07-15T23:05:59Z"
    generation: 1
    name: cheddar
    namespace: default
    resourceVersion: "3351531"
    selfLink: /apis/extensions/v1beta1/namespaces/default/ingresses/cheddar
    uid: 4cf88632-1b85-4679-8173-fb0daa18c216
    spec:
    rules:
    - host: cheddar.127.0.0.1.nip.io
        http:
        paths:
        - backend:
            serviceName: cheddar
            servicePort: 80
            path: /
    status:
    loadBalancer:
        ingress:
        - ip: 10.152.183.163
    ```


## Annotations

### Adding features
- redirect
    ```
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    name: my-google
    annotations:
        nginx.ingress.kubernetes.io/permanent-redirect: https://www.google.com
    spec:
    rules:
    - http:
        paths:
        - path: /
            backend:
            serviceName: doesntmatter
            servicePort: 80
    ```
