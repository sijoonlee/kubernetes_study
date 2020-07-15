## Baking custom images
- put the configuration in the image
- it could be file, ENV, CMD
- downside
    - multiplication of images
    - different images for dev, staging, prod
    - minor reconfigurations requires a whole build/push/pull cycle
- avoid this as much as you can

## Command-line arguments
- works great when there aren't too many parameters
- not ideal when we need to pass a real config file

## Environment variables
- pass options through the env map in the container specification
    ```
    env:
    - name: ADMIN_PORT
    value: "8080"
    ```
## Downward API
- Downward API allows exposing pod or container information
    - either through special files
    - or through environment variables
- the value of these environment variables is computed when the container starts
    - env variables can't be changed after container starts
- Exposing the pod's namespace
    ```
    - name: MY_POD_NAMESPACE
      valueFrom:
        fieldRef:
          fieldPath: metadata.namespace
    ```
- Exposing the pod's IP address
    ```
    - name: MY_POD_IP
      valueFrom:
        fieldRef:
          fieldPath: status.podIP
    ```
- Exposing the container's resource limit
    ```
    - name: MY_MEM_LIMIT
      valueFrom:
        resourceFieldRef:
          containerName: test-container
          resource: limits.memory
    ```

## ConfigMap
- ConfigMap is a kubernetes resource