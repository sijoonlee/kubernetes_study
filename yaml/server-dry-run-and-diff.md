## the limits of *kubectl apply --dry-run*
- *--dry-run* is a client-only
- example
    1. create deployment
    ```
    kubectl create deployment web --image=nginx -o yaml > web.yaml
    ```
    2. change the *kind* to *DaemonSet* in web.yaml
    3. apply the yaml file
    ```
    kubectl apply -f web.yaml --dry-run --validate=false -o yaml > web-daemon-set.yaml
    ```
    4. see the file, it includes *replicas: 1*, which is not part of *DaemonSet*
    ```
    spec:
      progressDeadlineSeconds: 600
      replicas: 1
    ```
    5. apply the yaml file with --server-dry-run
    ```
    kubectl apply -f web.yaml --server-dry-run --validate=false -o yaml > web-daemon-set.yaml
    ```
    6. now there is no *replicas: 1*
    ```
    spec:
      revisionHistoryLimit: 10
    ```
    7. this is becasue the server api throws away what it doesn't know


## kubectl diff
- it does server-side dry run, but only shows differences
- example
    1) kubectl apply -f web.yaml
    2) change something in web.yaml
    3) kubectl diff -f web.yaml