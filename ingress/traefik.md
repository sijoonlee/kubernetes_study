## Traefik
- Traefik is a proxy with built-in Kubernetes Ingress support
- Documentation: https://docs.traefik.io/v1.7/user-guide/kubernetes/#deploy-traefik-using-a-deployment-or-daemonset
- Deployment or DaemonSet
- Demo uses DaemonSet

## yaml
- https://github.com/containous/traefik/blob/v1.7/examples/k8s/traefik-ds.yaml
- the yaml was modified to use **hostNetwork**

## apply
- kubectl apply -f https://k8smastery.com/ic-traefik-hn.yaml
- kubectl describe -n kube-system ds/traefik-ingress-controller

## Traefik web ui
- default: localhost:8080 - unfortunately, microk8 using this port

## CRD
- Traefik 2.x supports Custom Resource Definition