## how to expose ConfigMap to container
- ConfigMap can be exposed as plain files in the container
    - declaring a volume and mounting it in the container

- ConfigMap can be exposed as environment variables in the container
    - using Downward API

## ConfigMap as file demo
- will use official haproxy image
- It expects to find its configuration in /usr/local/etc/haproxy/haproxy.cfg
- haproxy.cfg looks like
    ```
    global
    daemon
    maxconn 256

    defaults
    mode tcp
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

    frontend the-frontend
    bind *:80
    default_backend the-backend

    backend the-backend
    server google.com-80 google.com:80 maxconn 32 check
    server ibm.fr-80 ibm.fr:80 maxconn 32 check
    ```
- create a ConfigMap resource
    ```
    kubectl create configmap haproxy --from-file=haproxy.cfg
    ```
- check
    ```
    kubectl get configmap haproxy -o yaml
    ```
    ```
    apiVersion: v1
    data:
    haproxy.cfg: |+
        global
        daemon
        maxconn 256

        defaults
        mode tcp
        timeout connect 5000ms
        timeout client 50000ms
        timeout server 50000ms

        frontend the-frontend
        bind *:80
        default_backend the-backend

        backend the-backend
        server google.com-80 google.com:80 maxconn 32 check
        server ibm.fr-80 ibm.fr:80 maxconn 32 check

    kind: ConfigMap
    metadata:
    creationTimestamp: "2020-07-15T20:02:02Z"
    name: haproxy
    namespace: default
    resourceVersion: "3325442"
    selfLink: /api/v1/namespaces/default/configmaps/haproxy
    uid: 276035c8-1f4e-467f-b939-8d1cb9f758cc
    ```

- create haproxy pod
    ```
    kubectl apply -f https://k8smastery.com/haproxy.yaml
    ```
- haproxy.yaml looks like
    ```
    apiVersion: v1
    kind: Pod
    metadata:
    name: haproxy
    spec:
    volumes:
    - name: config
        configMap:
        name: haproxy
    containers:
    - name: haproxy
        image: haproxy:1
        volumeMounts:
        - name: config
        mountPath: /usr/local/etc/haproxy/
    ```
- check the IP address allocated to the pod (inside shpod)
    ```
    kubectl attach --namespace=shpod -ti shpod
    kubectl get pod haproxy -o wide
    IP=$(kubectl get pod haproxy -o json | jq -r .status.podIP)
    ```
- try curl $IP serveral times