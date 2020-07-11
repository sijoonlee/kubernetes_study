## basics
- spaces(not tab) set the structure
- two spaces is the standard
- each file contains one or more manifests
- each manifest describes an API object (deployment, service, etc)
- each manifest needs four parts (root key:values)
    - apiVersion
    - kind
    - metadata
    - spec

## more than CLI
- Advanced features
    - pods with multiple containers
    - resource limits
    - health check
- not in CLI
    - DaemonSets
    - StatefulSets

## Generating YAML
- Examples: https://github.com/kubernetes/website/tree/master/content/en/examples
- generating YAML without creating resources
    - create with -o yaml --dry-run
    - ex
        ```
        kubectl create deployment web --image nginx -o yaml --dry-run
        ```
        ```
        apiVersion: apps/v1
        kind: Deployment
        metadata:
        creationTimestamp: null
        labels:
            app: web
        name: web
        spec:
        replicas: 1
        selector:
            matchLabels:
            app: web
        strategy: {}
        template:
            metadata:
            creationTimestamp: null
            labels:
                app: web
            spec:
            containers:
            - image: nginx
                name: nginx
                resources: {}
        status: {}
        ```
        ```
        kubectl create namespace awesome-app -o yaml --dry-run
        ```
        ```
        apiVersion: v1
        kind: Namespace
        metadata:
        creationTimestamp: null
        name: awesome-app
        spec: {}
        status: {}
        ```
