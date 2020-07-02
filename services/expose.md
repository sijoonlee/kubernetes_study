## kubectl expose
- creates a service for existing pods
- service is a stable address for a pod
- to connect to pods, we need service
- once a service is created, CoreDNS will allow us to resolve it by name

## different types of services
- ClusterIP
    - a virtual IP address is allocated for the service
    - the address is only reachable within the cluster(nodes, pods)
    - our code can connect to the service using the original port number
- NodePort
    - a port is allocated for the service
    - the port is avabilable on all our nodes, anyone can connect to it
    - our code should use the new port number
- LoadBalancer
    - an external load balancer is allocated for the service
    - not available in vanilla kubernetes
    - supported by 3rd part tool, AWS, Azure, ...
- ExternalName
    - the DNS entry managed by CoreDNS wilil just be a CNAME to a provided record
    - sustain an external name to avoid restarting pods and services again just because a name on the internet changes
    - no port, no IP address is allocated