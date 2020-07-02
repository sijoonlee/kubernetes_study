## What if we want to run differently
- create jobs/pods instead of deployment
    - kubectl run --restart=OnFailure
    - kubectl run --restart=Never
- **kubectl run** invokes **generators** under the hood to create resource descriptions
    - From 1.18, run only creats a pod, no longer supports generators
    - we can write resource descriptions (typically in yaml format)
    - and use them with **kubectl apply -f**
- **kubectl run --schedule=** allows to create **cronjobs**

## Cron job
- documentation: https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/
- cron job: a job that will be executed at specific intervals
- it's not the cron from linux or unix (just name)
- it requires a schedule, consisting of 5 space-separated fields
    - minute [0, 59]
    - hour [0, 23]
    - day of the month [1, 31]
    - month of the year [1, 12]
    - day of the week [0, 6] // 0 is Sunday
- * means all valid values
- /N means every N
- example: */3 * * * * means "every three minutes"

## Demo
- kubectl run --schedule="*/3 * * * *" --restart=OnFailure --image=alpine sleep 10
    - From 1.18, use **kubectl create cronjob** instead of **kubectl run**
    - --schedule="*/3 * * * *" : start every three minutes
    - --restart=OnFailure : restart on failure, if it ends without errors, it doesn't restart till the scheduled time
    ```
    kubectl run --generator=cronjob/v1beta1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
    cronjob.batch/sleep created
    ```
- kubectl get cronjobs
    ```
    NAME    SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
    sleep   */3 * * * *   False     2        31s             4m9s
    ```
- kubectl get jobs
    ```
    NAME               COMPLETIONS   DURATION   AGE
    sleep-1593701460   0/1           4m9s       4m9s
    sleep-1593701640   0/1           69s        69s
    ```