## Services are layer 4 constructs
- a service is not an IP address, it's an IP address + protocol + port
- This is because of **kube-proxy**, which doesn't support layer 3
- You need to indicate the port number for your service
- Running services with arbitrary port requires hacks