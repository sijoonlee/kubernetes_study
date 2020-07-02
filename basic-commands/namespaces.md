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

## kube-public
- kubectl -n kube-public get configmaps
- kubectl -n kube-public get configmap cluster-info -o yaml