### create deployment and expose
kubectl create deployment httpenv --image=bretfisher/httpenv
kubectl expose deployment httpenv --port 8888

### Run shpod
kubectl apply -f https://k8smastery.com/shpod.yaml  
kubectl attach --namespace=shpod -ti shpod  

### Optain IP address programmtically
IP=$(kubectl get service httpenv -o go-template --template '{{ .spec.clusterIP }}')
- go lang is involved here

### Send a few requests
curl http://$IP:8888/
```
{"HOME":"/root","HOSTNAME":"httpenv-6bf64f7c4f-bzdsr","KUBERNETES_PORT":"tcp://10.152.183.1:443","KUBERNETES_PORT_443_TCP":"tcp://10.152.183.1:443","KUBERNETES_PORT_443_TCP_ADDR":"10.152.183.1","KUBERNETES_PORT_443_TCP_PORT":"443","KUBERNETES_PORT_443_TCP_PROTO":"tcp","KUBERNETES_SERVICE_HOST":"10.152.183.1","KUBERNETES_SERVICE_PORT":"443","KUBERNETES_SERVICE_PORT_HTTPS":"443","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"}
```
### filter
curl -s http://$IP:8888/ | jq .HOSTNAME
```
"httpenv-6bf64f7c4f-bzdsr"
```

### Headless Services
- it can be obtained by setting **clusterIP** field to **None** in yaml
    - or --cluster-ip=None
- service doesn't have a virtual IP address
- no load balancer
- CoreDNS will return the pods's IP addresses as multiple A records
- This gives us an easy way to discover all the replicas for a deployment

### Services and endpoints
- kubectl get endpoints
    ```
    NAME         ENDPOINTS              AGE
    httpenv      10.1.28.161:8888       20m
    kubernetes   192.168.64.128:16443   24d
    ```
- kubectl get endpoints httpenv -o yaml
    ```
    apiVersion: v1
    kind: Endpoints
    metadata:
    annotations:
        endpoints.kubernetes.io/last-change-trigger-time: "2020-07-03T13:49:09-07:00"
    creationTimestamp: "2020-07-03T20:49:09Z"
    labels:
        app: httpenv
    name: httpenv
    namespace: default
    resourceVersion: "2233753"
    selfLink: /api/v1/namespaces/default/endpoints/httpenv
    uid: 37b38544-75b4-4414-9870-f1418b1bf6f6
    subsets:
    - addresses:
    - ip: 10.1.28.161
        nodeName: ubuntu
        targetRef:
        kind: Pod
        name: httpenv-6bf64f7c4f-bzdsr
        namespace: default
        resourceVersion: "2233039"
        uid: 49af8c4c-35be-4f62-87a9-bdf3b328eb43
    ports:
    - port: 8888
        protocol: TCP
    ```
- kubectl get pods -l app=httpenv -o wide
    ```
    NAME                       READY   STATUS    RESTARTS   AGE   IP            NODE     NOMINATED NODE   READINESS GATES
    httpenv-6bf64f7c4f-bzdsr   1/1     Running   0          25m   10.1.28.161   ubuntu   <none>           <none>
    ```