## Exposing HTTP services
- NodePort
    - clients have to specifiy port numbers ex) http://asdf:11111
- LoadBalancer
    - only available in some environments
    - carrys an additional cost
    - often work at OSI layer 4 (IP+Port) and not Layer 7 (HTTP/s)
    - requires one extra setup for DNS integration
        - waiting for the LoadBalancer to be provisioned; then adding it to DNS
- We could build our own reverse proxy