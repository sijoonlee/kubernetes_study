## Healthchecks
- Healthchecks are key for providing built-in lifecycle automation
- Healthchecks are **probes** that apply to **containers** (not pods)
- Kubernetes will take action on containers that fail healthchecks
- Each container can have three(optional) probes
    - liveness : dead or alive ?
    - readliness : ready to serve traffic ? (only needed if a service)
    - startup : is it starting up ?
- different probe handlers are available (HTTP, TCP, program execution)

## Liveness Probe
- if the liveness probe fails, the container is killed
- what happens next depends on the pod's **restartPolicy**
    - **Never** : never restart
    - **OnFailure** or **Always** : the container will restart
- whe to use liveness probe
    - to indicate failures that can't be recovered
        - deadlocks ( causing all requests to timeout )
        - internal corruption ( causing all requests error )

## Readiness probe
- if a container becomes 'unready', it might be ready again soon
- if the readiness probe fails
    - the container is not killed
    - if the pod is a member of a service, it is temporarily removed
    - it is re-added as soon as the readiness prove passes again
- when to use
    - to indicate failure of external causes
        - database is down/unreachable
        - mandatory auth or other backend service unavaiable
    - to indicate temporary failure or unavailablity
        - application can only service N parallel connections
        - runtime is busy doing garbage collection or initial data load

## Startup probe
- it it to indicate if container is ready or not
    - process is still starting
    - loading external data, priming caches
- Before Kubernetes 1.16 (when startupProbe was introduced first time)
    - **initialDelaySeconds** parameter was used
    - **initialDelaySeconds** is a rigid delay
    - **startupProbe** works better if start time varies a lot