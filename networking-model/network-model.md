## In short
- our cluster is one big flat IP network

## Detail
- everything can reach everything
- No address translation
- No port translation
- No new protocol

## Limit
- if you want security, you need to add network policies
- Pods have level 3 (IP) connectivity, but Services are level 4 (TCP or UDP)
    - Services map to a single UDP or TCP port, no port ranges or arbitrary IP packets
- kube-proxy is not fast - relies on userland proxing or iptables

## 3rd party tools
- kubenet, Calico and so on
- kube-router would be alternative to kube-proxy
*kube-router can be set up to provide the pod network and/or network policies and/or replace kube-proxy*

## The Container Network Interface (CNI)
- Most kubenetes clusters use CNI 'plugins' to implement networking
- When a pod is created, Kubernetes delegates the network setup to these plugins
- Typically, CNI plugins will
    - allocate an IP address (by calling an IPAM plugin)
    - add a network interface into the pod's network namespace
    - configure the interface, routes, etc

## Multiple Moving Parts
- pod-to-pod network: generally implemented by CNI plugins
- pod-to-service network: load balancing, generally implemented by kube-proxy
- network policies: firewall, isolation