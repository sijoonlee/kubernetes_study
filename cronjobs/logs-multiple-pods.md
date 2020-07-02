## kubectl logs -l run=pingpong --tail 1 -f
    -l : selector, select based upon labels

## if there's over 5 pods
- kubectl scale deployment pingpong --replicas=8
- kubectl logs -l run=pingpong --tail 1 -f
    ```
    error: you are attempting to follow 8 log streams, 
    but maximum allowed concurrency is 5, 
    use --max-log-requests to increase the limit
    ```

## Why limited?
- kubectl opens one connection to the API server per pod
- The API server opens one extra connection to the corresponding kubelet for each pod
- For example, for 100 pods, there are 100 inbound + 100 outbound connections on API server
- This could easily put a lot of stress on the API server

## Shortcomings of kubectl logs
- can't figure out which pod sent the log line
- when pod restarts/replaced, the log stream stops
- when new pod added, log doesn't include
- External tools to get over with these problem : ex) Stern