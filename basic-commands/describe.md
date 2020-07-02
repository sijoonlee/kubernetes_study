## kubectl describe

- kubectl describe node/<node-name>
- kubectl describe node <node-name>  // in below example, kubectl describe node/ubuntu
    ```
    Name:               ubuntu
    Roles:              <none>
    Labels:             beta.kubernetes.io/arch=amd64
                        beta.kubernetes.io/os=linux
                        kubernetes.io/arch=amd64
                        kubernetes.io/hostname=ubuntu
                        kubernetes.io/os=linux
                        microk8s.io/cluster=true
    Annotations:        node.alpha.kubernetes.io/ttl: 0
                        volumes.kubernetes.io/controller-managed-attach-detach: true
    CreationTimestamp:  Tue, 09 Jun 2020 13:14:31 +0000

    ---MORE---
    ```