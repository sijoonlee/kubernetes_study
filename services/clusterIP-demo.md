## kubectl get pods -w  (--watch)
```
NAME                       READY   STATUS    RESTARTS   AGE
httpenv-6bf64f7c4f-g7cgd   0/1     Pending   0          0s           <- kubectl create deployment httpenv --image=bretfisher/httpenv
httpenv-6bf64f7c4f-g7cgd   0/1     Pending   0          0s
httpenv-6bf64f7c4f-g7cgd   0/1     ContainerCreating   0          0s
httpenv-6bf64f7c4f-g7cgd   1/1     Running             0          4s   

httpenv-6bf64f7c4f-pxjbw   0/1     Pending             0          0s <- kubectl scale deployment httpenv --replicas 3
httpenv-6bf64f7c4f-pxjbw   0/1     Pending             0          0s
httpenv-6bf64f7c4f-xhbq2   0/1     Pending             0          0s
httpenv-6bf64f7c4f-xhbq2   0/1     Pending             0          0s
httpenv-6bf64f7c4f-pxjbw   0/1     ContainerCreating   0          0s
httpenv-6bf64f7c4f-xhbq2   0/1     ContainerCreating   0          0s
httpenv-6bf64f7c4f-xhbq2   1/1     Running             0          1s
httpenv-6bf64f7c4f-pxjbw   1/1     Running             0          3s

nothing changed                                                      <-kubectl expose deployment httpenv --port 8888


```

## kubectl get services
```
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
httpenv      ClusterIP   10.152.183.174   <none>        8888/TCP   70s
kubernetes   ClusterIP   10.152.183.1     <none>        443/TCP    23d
```