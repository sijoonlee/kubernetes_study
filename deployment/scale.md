## kubectl scale deploy/pingpong --replicas 3
- or kubectl scale deployment pingpong --replicas 3
```
deployment.apps/pingpong scaled
```

### kubectl get all
```
NAME                           READY   STATUS    RESTARTS   AGE
pod/pingpong-c849c8bbc-g9hfc   1/1     Running   0          17s
pod/pingpong-c849c8bbc-rsjnq   1/1     Running   0          13m
pod/pingpong-c849c8bbc-tgr7t   1/1     Running   0          17s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   23d

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pingpong   3/3     3            3           13m

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/pingpong-c849c8bbc   3         3         3       13m
```
