1. Deploy
```
kubectl create deployment db --image=bretfisher/wordsmith-db
kubectl create deployment web --image=bretfisher/wordsmith-web
kubectl create deployment words --image=bretfisher/wordsmith-words
```

2. Expose
```
kubectl expose deployment db --port=5432
kubectl expose deployment web --port=80 --type=NodePort
kubectl expose deployment words --port=8080
```
or 
```
kubectl create service clusterip db --tcp=5432
kubectl create service nodeport web --tcp=80
kubectl create service clusterip words --tcp=8080
```

3. Get the address
```
kubectl get service web
```

4. Scale up
```
kubectl scale deployment words --replicas=5
```