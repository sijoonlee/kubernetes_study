# kubectl get node
- kubectl get node
    ```
    NAME     STATUS   ROLES    AGE   VERSION
    ubuntu   Ready    <none>   21d   v1.17.7
    ```
- kubectl get node -o wide
    ```
    NAME     STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE       KERNEL-VERSION     CONTAINER-RUNTIME
    ubuntu   Ready    <none>   21d   v1.17.7   192.168.64.128   <none>        Ubuntu 19.10   5.3.0-61-generic   containerd://1.2.5
    ```
- kubectl get node -o yaml
    ```
    apiVersion: v1
    items:
    - apiVersion: v1
    kind: Node
    metadata:
        annotations:
        node.alpha.kubernetes.io/ttl: "0"
        volumes.kubernetes.io/controller-managed-attach-detach: "true"
        creationTimestamp: "2020-06-09T13:14:31Z"
        labels:
        beta.kubernetes.io/arch: amd64
        beta.kubernetes.io/os: linux
        kubernetes.io/arch: amd64
        kubernetes.io/hostname: ubuntu
        kubernetes.io/os: linux
        microk8s.io/cluster: "true"
        name: ubuntu
        resourceVersion: "2027819"
        selfLink: /api/v1/nodes/ubuntu
        uid: 65d6d347-edec-4151-b76f-e61e41971df5
    spec: {}
    status:
        addresses:
        - address: 192.168.64.128
        type: InternalIP
        - address: ubuntu
        type: Hostname
        allocatable:
        cpu: "4"
        ephemeral-storage: 50425312Ki
        hugepages-1Gi: "0"
        hugepages-2Mi: "0"
        memory: 8022448Ki
        pods: "110"
        capacity:
        cpu: "4"
        ephemeral-storage: 51473888Ki
        hugepages-1Gi: "0"
        hugepages-2Mi: "0"
        memory: 8124848Ki
        pods: "110"
        conditions:
        - lastHeartbeatTime: "2020-06-30T16:02:13Z"
        lastTransitionTime: "2020-06-24T11:08:06Z"
        message: kubelet has sufficient memory available
        reason: KubeletHasSufficientMemory
        status: "False"
        type: MemoryPressure
        - lastHeartbeatTime: "2020-06-30T16:02:13Z"
        lastTransitionTime: "2020-06-24T11:08:06Z"
        message: kubelet has no disk pressure
        reason: KubeletHasNoDiskPressure
        status: "False"
        type: DiskPressure
        - lastHeartbeatTime: "2020-06-30T16:02:13Z"
        lastTransitionTime: "2020-06-24T11:08:06Z"
        message: kubelet has sufficient PID available
        reason: KubeletHasSufficientPID
        status: "False"
        type: PIDPressure
        - lastHeartbeatTime: "2020-06-30T16:02:13Z"
        lastTransitionTime: "2020-06-30T10:56:25Z"
        message: kubelet is posting ready status. AppArmor enabled
        reason: KubeletReady
        status: "True"
        type: Ready
        daemonEndpoints:
        kubeletEndpoint:
            Port: 10250
        images:
        - names:
        - docker.io/coredns/coredns@sha256:e83beb5e43f8513fa735e77ffc5859640baea30a882a11cc75c4c3244a737d3c
        - docker.io/coredns/coredns:1.5.0
        sizeBytes: 13341835
        - names:
        - k8s.gcr.io/pause@sha256:f78411e19d84a252e53bff71a4407a5686c46983a2c2eeed83929b888179acea
        - k8s.gcr.io/pause:3.1
        sizeBytes: 317164
        nodeInfo:
        architecture: amd64
        bootID: ea10ca63-b6dc-41e1-9e4d-3fe8c675a1fb
        containerRuntimeVersion: containerd://1.2.5
        kernelVersion: 5.3.0-61-generic
        kubeProxyVersion: v1.17.7
        kubeletVersion: v1.17.7
        machineID: 2bb3d657a5cb482dbe2c2a7a11c2720d
        operatingSystem: linux
        osImage: Ubuntu 19.10
        systemUUID: fc804d56-0208-2181-3989-8c596122185a
    kind: List
    metadata:
    resourceVersion: ""
    selfLink: ""
    ```

- kubectl get nodes -o json | jq ".items[] | {name: .metadata.name} + .status.capacity"
    ```
    {
    "name": "ubuntu",
    "cpu": "4",
    "ephemeral-storage": "51473888Ki",
    "hugepages-1Gi": "0",
    "hugepages-2Mi": "0",
    "memory": "8124848Ki",
    "pods": "110"
    }
    ```

## kubectl get services
- service: a stable endpoint to connect to somthing
- kubectl get services
- kubectl get svc
```
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   23d
```

## kubectl get pods
- kubectl get pods
    ```
    No resources found in default namespace.

    ```
- kubectl get pods --all-namespaces
- kubectl get pods -A
    ```
    NAMESPACE     NAME                                              READY   STATUS    RESTARTS   AGE
    ingress       nginx-ingress-microk8s-controller-29dz8           1/1     Running   2          45h
    kube-system   coredns-9b8997588-skdb4                           1/1     Running   62         23d
    kube-system   dashboard-metrics-scraper-687667bb6c-lrwlx        1/1     Running   2          45h
    kube-system   heapster-v1.5.2-5c58f64f8b-llkhb                  4/4     Running   8          45h
    kube-system   kubernetes-dashboard-5c848cc544-fmdn9             1/1     Running   2          45h
    kube-system   monitoring-influxdb-grafana-v4-6d599df6bf-zs797   2/2     Running   4          45h
    shpod         shpod                                             1/1     Running   2          45h
    ```
- kubectl get pods --namespace=kube-system
- kubectl get pods -n kube-system
    ```
    NAME                                              READY   STATUS    RESTARTS   AGE
    coredns-9b8997588-skdb4                           1/1     Running   62         23d
    dashboard-metrics-scraper-687667bb6c-lrwlx        1/1     Running   2          45h
    heapster-v1.5.2-5c58f64f8b-llkhb                  4/4     Running   8          45h
    kubernetes-dashboard-5c848cc544-fmdn9             1/1     Running   2          45h
    monitoring-influxdb-grafana-v4-6d599df6bf-zs797   2/2     Running   4          45h
    ```
- what's there
    - etcd: etcd server
    - kube-apiserver: API server
    - kube-controller-manager
    - kube-scheduler
    - coredns
    - kube-proxy: manage port mapping
    - <net name>: managing the network overlay
    - READY column indicates the number of containers in the pod
    - it won't show host services, only shows containers


## kubectl get namespaces
    ```
    NAME              STATUS   AGE
    default           Active   23d
    ingress           Active   45h
    kube-node-lease   Active   23d
    kube-public       Active   23d  // created by installer & used for security bootstrapping
    kube-system       Active   23d
    shpod             Active   45h
    ```

## Note
    - **kubectl delete** and **kubectl label** affect resources across multiple namespaces