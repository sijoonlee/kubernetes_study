## Running DockerCoins on Kubernetes
- create one deployment for each component
- expose deployments that need to accept connections
    - hasher, redis, rng, webui

## Deploy
- redis
    ```
    kubectl create deployment redis --image=redis
    ```
- Deploy everything else
    ```
    kubectl create deployment hasher --image=dockercoins/hasher:v0.1
    kubectl create deployment rng --image=dockercoins/rng:v0.1
    kubectl create deployment webui --image=dockercoins/webui:v0.1
    kubectl create deployment worker --image=dockercoins/worker:v0.1
    ```

## Expose
- Three deployments need to be reachable by others: hasher, redis, rng
    ```
    kubectl expose deployment redis --port 6379
    kubectl expose deployment rng --port 80
    kubectl expose deployment hasher --port 80
    ```
- webui needs to be exposed with a NodePort
    ```
    kubectl expose deploy/webui --type=NodePort --port=80
    ```
- access the node
    - http://localhost:3xxxx


## Checking
- see if they are working 
    ```
    kubectl get svc 
    kubectl get deploy -w
    kubectl logs deploy/rng
    kubectl logs deploy/worker
    kubectl logs deploy/worker --follow
    ```

## Scaling up
```
kubectl scale deployment worker --replicas=2
kubectl scale deployment worker --replicas=3
kubectl scale deployment worker --replicas=4
kubectl scale deployment worker --replicas= ...
```
- is the task done linear to the number of workers? No
    - when replicas over 10, the performance doesn't increase

## Benchmarking web services
- Datadog, Honeycomb, New Relic, statsd, Sumologic, ...
- or simple tool like ab, httping

## Using Httping
- just like ping, but using HTTP GET request
- will check hasher and rng
- usage
    ```
    httping [-c count] http://host:port/path
    httping ip.add.re.ss
    ```
- will use httping on the ClusterIP address

## run httping
- use shpod
    ```
    kubectl apply -f https://bret.run/shpod.yml
    kubectl attach --namespace=shpod -ti shpod
    ```
- get ClusterIP address - kubectl get services or programmtically
    ```
    HASHER=$(kubectl get svc hasher -o go-template={{.spec.clusterIP}})
    RNG=$(kubectl get svc rng -o go-template={{.spec.clusterIP}})
- run httping
    ```
    httping -c 3 $HASHER
    httping -c 3 $RNG
    ```
- it turned out rng is the bottleneck (fiction)