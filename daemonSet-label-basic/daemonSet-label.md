## DaemonSet
- new resource type
- it is to ensure one pod per each node

### Create yaml file for daemon set
- dump rng resource into yaml
    ```
    kubectl get deploy/rng -o yaml >rng.yml
    ```
- edit yml
    - change type with "DaemonSet"
    - remove the replicas field
    - remove the strategy field (which defines the rollout mechanism for a deployment)
    - remove the progressDeadlineSeconds field (also used by the rollout mechanism)
    - remove the status: {} line at the end

- apply
    - normally...
    ```
    kubectl apply -f rng.yml
    ```
    - or skip validation
    ```
    kubectl apply -f rng.yml --validate=false
    ```

## Labels and Selectors
- rng service is load balancing requests to a set of pods
- that set of pods is defined by the selector of the rng service
- kubectl describe service rng
    - **Selector: app=rng**
- means all pods having the label **app=rng** 
- list of pods matching selector app=rng
    - kubectl get pods -l app=rng
    - kubectl get pods --selector app=rng
- it turns out that our DaemonSet has the same label 'app=rng'
- how did it happen?

### Where do labels come from?
- When we create a deployment with kubectl create deployment rng,
- this deployment gets the label app=rng
- The replica sets created by this deployment also get the label app=rng
- The pods created by these replica sets also get the label app=rng
- **When we created the daemon set from the deployment, we re-used the same spec**
- Therefore, the pods created by the daemon set get the same labels
- When we use kubectl run stuff, the label is run=stuff instead

### Selectors for replica sets and daemon sets
- The "mission" of a replica set is:
    "Make sure that there is the right number of pods matching this spec!"
- The "mission" of a daemon set is:
    "Make sure that there is a pod matching this spec on each node!"
- In fact, replica sets and daemon sets do not check pod specifications
    They merely have a selector, and they look for pods matching that selector
- Yes, we can fool them by manually creating pods with the "right" labels
- Bottom line: if we remove our app=rng label ...
... The pod "disappears" for its parent, which re-creates another pod to replace it
- Since both the rng daemon set and the rng replica set use app=rng ...
    ... Why don't they "find" each other's pods?
- Replica sets have a more specific selector, visible with kubectl describe
    (It looks like app=rng,pod-template-hash=abcd1234)
- Daemon sets also have a more specific selector, but it's invisible
    (It looks like app=rng,controller-revision-hash=abcd1234)
- As a result, each controller only "sees" the pods it manages

### Removing a pod from the load balancer
- Currently, the rng service is defined by the app=rng selector
- The only way to remove a pod is to remove or change the app label
    ... But that will cause another pod to be created instead!
- What's the solution?
    - We need to change the selector of the rng service!
    - Let's add another label to that selector (e.g. active=yes)

### The plan
- Add the label active=yes to all our rng pods
- Update the selector for the rng service to also include active=yes
- Toggle traffic to a pod by manually adding/removing the active label
- Note: if we swap steps 1 and 2, it will cause a short service disruption, because there will be a period of time during which the service selector won't match any pod. During that time, requests to the service will time out. By doing things in the order above, we guarantee that there won't be any interruption.

### Adding labels to pods
- We want to add the label active=yes to all pods that have app=rng
- We could edit each pod one by one with kubectl edit
- Or we could use kubectl label to label them all
- kubectl label can use selectors itself
- Add active=yes to all pods that have app=rng
    ```
    kubectl label pods -l app=rng active=yes
    ```

### Updating the service selector
- We need to edit the service specification
- Reminder: in the service definition, we will see app: rng in two places
    - the label of the service itself (we don't need to touch that one)
    - the selector of the service (that's the one we want to change)
- Update the service to add active: yes to its selector:
    - kubectl edit service rng
    ```
    selector:
        app: rng
        enabled: "yes"
    ```
    - make sure use "yes" not yes, since YAML parser recognizes it as boolean

### Removing a pod from the load balancer
- In one window, check the logs of that pod:
    ```
    POD=$(kubectl get pod -l app=rng,pod-template-hash -o name)
    kubectl logs --tail 1 --follow $POD
    ```
    (We should see a steady stream of HTTP logs)
- In another window, remove the label from the pod:
    ```
    kubectl label pod -l app=rng,pod-template-hash enabled-
    ```
    - *enabled-* means delete all labels 'enable'
    (The stream of HTTP logs should stop immediately)

###  Updating the daemon set
- If we scale up our cluster by adding new nodes, the daemon set will create more pods
- These pods won't have the enable=yes label
- If we want these pods to have that label, we need to edit the daemon set spec
- We can do that with e.g. kubectl edit daemonset rng
    ```
    spec:
        template:
            metadata:
                labels:
                    app: rng
                    enable: "yes"
    ```


### Summary
- Reminder: a daemon set is a resource that creates more resources!
- There is a difference between:
    - the label(s) of a resource (in the metadata block in the beginning)
    - the selector of a resource (in the spec block)
    - the label(s) of the resource(s) created by the first resource (in the template block)
- We would need to update the selector and the template
    (metadata labels are not mandatory)
- The template must match the selector
    (i.e. the resource will refuse to create resources that it will not select)

### Labels and debugging
- When a pod is misbehaving, we can delete it: another one will be recreated
- But we can also change its labels
- It will be removed from the load balancer (it won't receive traffic anymore)
- Another pod will be recreated immediately
- But the problematic pod is still here, and we can inspect and debug it
- We can even re-add it to the rotation if necessary

### Labels and advanced rollout control
- Conversely, we can add pods matching a service's selector
- These pods will then receive requests and serve traffic
- Examples:
    - one-shot pod with all debug flags enabled, to collect logs
    - pods created automatically, but added to rotation in a second step
        (by setting their label accordingly)
- This gives us building blocks for canary and blue/green deployments