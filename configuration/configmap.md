## how to expose ConfigMap to container
- ConfigMap can be exposed as plain files in the container
    - declaring a volume and mounting it in the container

- ConfigMap can be exposed as environment variables in the container
    - using Downward API