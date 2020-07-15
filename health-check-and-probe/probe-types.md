## Different types of probe handlers
- HTTP request
    - specifying URL of the request (and optional headers)
    - any status code between 200 and 399 indicates success
- TCP connection
    - the probe succeeds if the TCP port is open
- Arbitrary exec
    - a command is executed in the container
    - exit status of zero means success

## Timing and thresolds
- Probes are executed at intervals of **periodSeconds** (default: 10)
- The timeout for a probe is set with **timeoutSeconds** (default: 1)
    - if a probe takes longer than that, it is considered as FAIL
- A probe is considered successful after **successThreshold** successes (default: 1)
- A probe is considered failure after **failureThreshold** failures (default: 3)
- A probe can have an initialDelaySeconds parameter (default: 0)
- Kuber