## Shipping images with a registry
- For development using Docker, it has build, ship and run features
- When it comes to running a cluster, it's different
- Kubernetes doesn't have a build feature
- The way to pull images to Kubernetes is to use a registry

## How Docker registries work
- what happens when we execute **docker run alpine**?
- if the engine needs to pull the alpine image, it expands it into **library/alpine**
- **library/alpine** is expanded into **index.docker.io/libaray/alpine**
- The engine communicates with **index.docker.io** to retrieve **library/alpine:latest**
- To use other than **index.docker.io**, we specify it in the image name
- For example
    ```
    docker pull gcr.io/google-containers/alpine-with-bash:1.0
    docker build -t registry.mycompany.io:5000/myimage:awesome .
    docker push registry.mycompany.io:5000/myimage:aweful
    ```