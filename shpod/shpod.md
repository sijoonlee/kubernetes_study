## shpod
- provides a shell running in a pod on the cluster
- provides pre-installed tools - helm, stern, curl, jq, ...
- Docker image:  https://hub.docker.com/r/bretfisher/shpod/dockerfile

## How to use
- kubectl apply -f https://k8smastery.com/shpod.yaml
    - create
- kubectl get pods --namespace=shpod
- kubectl get pods --all-namespaces
- kubectl get pods -A
    - check the pod
- kubectl attach --namespace=shpod -ti shpod
    - attach to shell
- kubectl delete -f https://k8smastery.com/shpod.yaml
    - finish

## Q&A
error: unable to upgrade connection: container shpod not found in pod shpod_shpod

- Bret: Typically I get that error when I try to attach to shpod while the image is still downloading and the container isn't yet running.