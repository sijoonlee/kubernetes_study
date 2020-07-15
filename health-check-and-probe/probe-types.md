## Different types of probe handlers
- HTTP request
    - specifying URL of the request (and optional headers)
    - any status code between 200 and 399 indicates success
- TCP connection
    - the probe succeeds if the TCP port is open
- Arbitrary exec
    - a command is executed in the container
    - exit status of zero means success

## Timing and thresolds
- Probes are executed at intervals of **periodSeconds** (default: 10)
- The timeout for a probe is set with **timeoutSeconds** (default: 1)
    - if a probe takes longer than that, it is considered as FAIL
- A probe is considered successful after **successThreshold** successes (default: 1)
- A probe is considered failure after **failureThreshold** failures (default: 3)
- A probe can have an initialDelaySeconds parameter (default: 0)
    - Kubernetes will wait that amount of time before running the probe for the first time

## Example
- liveness probe with HTTP request
    ```
    containers:
    - name: ...
      image: ...
      livenessProbe:
        httpGet:
        path: /
        port: 80
        initialDelaySeconds: 30
        periodSeconds: 5
    ```
- liveness probe with exec
    ```
    apiVersion: v1
    kind: Pod
    metadata:
        name: redis-with-liveness
    spec:
        containers:
        - name: redis
          image: redis
          livenessProbe:
            exec:
              command: ["redis-cli", "ping"]
    ```

## testing the liveness probe
- kubectl get svc rng
    - get <ClusterIP>
- kubectl get events -w
- kubect get pods -w 

- Apache Bench to send concurrent request to rng
    ```
    kubectl attach --namespace=shpod -ti shpod
    ab -c 10 -n 1000 http://<ClusterIP>/1
    ```
    - c means the number of concurrent requests
