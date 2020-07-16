## Example Demo - ConfigMap with Downward API
1. single key setting
    ```
    kubectl create configmap registry --from-literal=http.addr=0.0.0.0:80
    kubectl get configmap registry -o yaml
    ```
2. yaml looks like
    ```
    apiVersion: v1
    data:
        http.addr: 0.0.0.0:80
    kind: ConfigMap
    metadata:
        creationTimestamp: "2020-07-15T20:12:30Z"
        name: registry
        namespace: default
        resourceVersion: "3326923"
        selfLink: /api/v1/namespaces/default/configmaps/registry
        uid: 5df477f5-efd0-4f2a-bd2c-45a8b45a458e
    ````
3. pod definition
    ```
    apiVersion: v1
    kind: Pod
    metadata:
    name: registry
    spec:
    containers:
    - name: registry
        image: registry
        env:
        - name: REGISTRY_HTTP_ADDR
        valueFrom:
            configMapKeyRef:
            name: registry
            key: http.addr
    ```