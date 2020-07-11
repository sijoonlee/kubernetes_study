## Rolling updates
- with rolling updates, when a Deployment is updated, it happends progressively
- the deployment controls multiple ReplicaSets
    - Each ReplicaSet is a group of identical Pods
    - During the rolling update, we have at least two ReplicaSets
        - new set
        - old set
    - we can have multiple 'old' sets
        - in case we start another update before the previous one is done

## Update strategy
- two parameters of determing the pace of the rollout
    - *maxUnavailable*
    - *maxSurge*
- can be specified as absolute number of pods or percentage of the replicas count
- rollout will be performed on conditions below
    - there will always be at least *replicas* - *maxUnavailable* pods available
    - there will never be more than *replicas* + *maxSurge* pods in total
    - there will therefore be up to *maxUnavailable* + *maxSurge* pods being updated

## Show the rollout plan for our deployments
- using jq processor
    - kubectl get deploy -o json |
        jq ".items[] | {name:.metadata.name} + .spec.strategy.rollingUpdate"

    ```
    {
        "name": "queue-worker",
        "maxSurge": "25%",
        "maxUnavailable": "25%"
    }
    ```

## Rolling updates in practice
1. watch
    ```
    kubectl get pods -w
    kubectl get deployments -w
    kubectl get replicasets -w
    ```
1. kubectl apply -f https://k8smastery.com/dockercoins.yaml

1. check rolling update plan
    - kubectl get deploy -o json |
        jq ".items[] | {name:.metadata.name} + .spec.strategy.rollingUpdate"

    ```
    {
        "name": "hasher",
        "maxSurge": "25%",
        "maxUnavailable": "25%"
    }
    {
        "name": "redis",
        "maxSurge": "25%",
        "maxUnavailable": "25%"
    }
    {
        "name": "rng",
        "maxSurge": "25%",
        "maxUnavailable": "25%"
    }
    {
        "name": "webui",
        "maxSurge": "25%",
        "maxUnavailable": "25%"
    }
    {
        "name": "worker",
        "maxSurge": "25%",
        "maxUnavailable": "25%"
    }
    ```
1. Update worker either with kubectl edit, or by running:
    kubectl set image deploy worker worker=dockercoins/worker:v0.2

1. Track the log: there are 10 workers

1. Update worker either with kubectl edit, or by running:
    kubectl set image deploy worker worker=dockercoins/worker:v0.3 
    *note* v0.3 doesn't exist

1. Track the log: there are 10 workers Running + 5 workers showing error "ErrImagePull" or "ImagePullBackOff"
    - maxUnavailable=25% -> the rollout terminated 2 replicas out of 10 (rounded down)
    - maxSurge=25% -> the rollout added 3 replicas
    - to sum them up, it's 5

