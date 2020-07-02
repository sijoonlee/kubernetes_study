## kubectl run pingpong --image alpine ping 1.1.1.1
```
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/pingpong created
```
- Note
    - since 1.18, 'run' only creates a pod
    - kubecl create deployment is doing the job instead

## kubectl get all
```
NAME                           READY   STATUS    RESTARTS   AGE
pod/pingpong-c849c8bbc-rsjnq   1/1     Running   0          69s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   23d

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pingpong   1/1     1            1           69s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/pingpong-c849c8bbc   1         1         1       69s
```

### How it works
- creation
    - **deployment** creates **replicaset**
    - **replicatset** creates **pod**
    - **pod** creates **container**
- deployment: high-level construct
    - allows scaling, rolling updates, rollbacks
    - multiple deployments can be used together to implement a canary deployment
    - delegates pods management to replica sets
- replica set: low-level construct
    - ensure that a given number of identical pods are running
    - allows scaling
    - rarely used directly
