## path
```
kubectl patch deployment worker -p "
spec:
  template:
    spec:
      containers:
      - name: worker
        image: dockercoins/worker:v0.1
  strategy:
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 1
  minReadySeconds: 10
"
```