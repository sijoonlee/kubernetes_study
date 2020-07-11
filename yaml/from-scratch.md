## Reference
https://github.com/kelseyhightower/kubernetes-the-hard-way

## Each manifest needs four part
- apiVersion: find with **kubectl api-versions**
    - example
        ```
        apps/v1
        batch/v1
        batch/v1beta1
        ```
    - consist of two parts: APIGROUP/version-of-the-api-group
        - for example, find APIGROUP first using **kubectl api-resources**
        - and find the APIGROUP with the version using **kubectl api-versions**
        - it would be **apps/v1** for deamonsets

- kind
    - find with **kubectl api-resources**
        ```
        NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
        configmaps                        cm                                          true         ConfigMap
        pods                              po                                          true         Pod
        daemonsets                        ds           apps                           true         DaemonSet
        deployments                       deploy       apps                           true         Deployment
        replicasets                       rs           apps                           true         ReplicaSet
        statefulsets                      sts          apps                           true         StatefulSet
        cronjobs                          cj           batch                          true         CronJob
        jobs                                           batch                          true         Job
        ```
        *Note* 'KIND' has capitalized character(s) unlike 'NAME'
    - dive into the *spec* of that *kind*
        ```
        kubectl explain <kind>.spec
        kubectl explain <kind> --recursive
        ```
        - kubectl explain pod
            ```
            KIND:     Pod
            VERSION:  v1

            DESCRIPTION:
                Pod is a collection of containers that can run on a host. This resource is
                created by clients and scheduled onto hosts.

            FIELDS:
            apiVersion   <string>
                APIVersion defines the versioned schema of this representation of an
                object. Servers should convert recognized schemas to the latest internal
                value, and may reject unrecognized values. More info:
                https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

            kind <string>
                Kind is a string value representing the REST resource this object
                represents. Servers may infer this from the endpoint the client submits
                requests to. Cannot be updated. In CamelCase. More info:
                https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

            metadata     <Object>
                Standard object's metadata. More info:
                https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

            spec <Object>
                Specification of the desired behavior of the pod. More info:
                https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

            status       <Object>
                Most recently observed status of the pod. This data may not be up to date.
                Populated by the system. Read-only. More info:
                https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
            ```
        - kubectl explain pods.spec
            ```
            KIND:     Pod
            VERSION:  v1

            RESOURCE: spec <Object>

            DESCRIPTION:
                Specification of the desired behavior of the pod. More info:
                https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

                PodSpec is a description of a pod.

            FIELDS:
            activeDeadlineSeconds        <integer>
                Optional duration in seconds the pod may be active on the node relative to
                StartTime before the system will actively try to mark it failed and kill
                associated containers. Value must be a positive integer.

            affinity     <Object>
                If specified, the pod's scheduling constraints

            automountServiceAccountToken <boolean>
                AutomountServiceAccountToken indicates whether a service account token
                should be automatically mounted.

            containers   <[]Object> -required-
                List of containers belonging to the pod. Containers cannot currently be
                added or removed. There must be at least one container in a Pod. Cannot be
                updated.
            <MORE>
            ```
        -  kubectl explain pods.spec.containers
            ```
            KIND:     Pod
            VERSION:  v1

            RESOURCE: containers <[]Object>

            DESCRIPTION:
                List of containers belonging to the pod. Containers cannot currently be
                added or removed. There must be at least one container in a Pod. Cannot be
                updated.

                A single application container that you want to run within a pod.

            FIELDS:
            args <[]string>
                Arguments to the entrypoint. The docker image's CMD is used if this is not
                provided. Variable references $(VAR_NAME) are expanded using the
                container's environment. If a variable cannot be resolved, the reference in
                the input string will be unchanged. The $(VAR_NAME) syntax can be escaped
                with a double $$, ie: $$(VAR_NAME). Escaped references will never be
                expanded, regardless of whether the variable exists or not. Cannot be
                updated. More info:
                https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#running-a-command-in-a-shell

            command      <[]string>
                Entrypoint array. Not executed within a shell. The docker image's
                ENTRYPOINT is used if this is not provided. Variable references $(VAR_NAME)
                are expanded using the container's environment. If a variable cannot be
                resolved, the reference in the input string will be unchanged. The
                $(VAR_NAME) syntax can be escaped with a double $$, ie: $$(VAR_NAME).
                Escaped references will never be expanded, regardless of whether the
                variable exists or not. Cannot be updated. More info:
                https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#running-a-command-in-a-shell

            env  <[]Object>
                List of environment variables to set in the container. Cannot be updated.

            envFrom      <[]Object>
                List of sources to populate environment variables in the container. The
                keys defined within a source must be a C_IDENTIFIER. All invalid keys will
                be reported as an event when the container is starting. When a key exists
                in multiple sources, the value associated with the last source will take
                precedence. Values defined by an Env with a duplicate key will take
                precedence. Cannot be updated.
            <MORE>
            ```

- metadata: give *name* (minimum req)
- spec: find with **kubectl describe <pod>**

## test
- use *--dry-run* and *--server-dry-run*
- use YAML linter: pip install yamllint (github.com/adrienverge/yamllint)
- use Kubernetes validator: kubeval (github.com/instrumenta/kubeval)
