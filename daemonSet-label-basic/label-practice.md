## 1
1. Create a deployment running one pod using the official NGINX image.
    ```
    kubectl create deployment v1-nginx --image=nginx
    ```
2. Expose that deployment.
    ```
    kubectl expose deployment v1-nginx --port=80 
    ```
    ```
    or kubectl create service v1-nginx --tcp=80
    ```
3. Check that you can successfully connect to the exposed service.
    - If you are using `shpod`, or if you are running directly on the cluster:
        ```
        kubectl get svc v1-nginx
        curl http://A.B.C.D
        ```
    - You can also run a program like `curl` in a container:
        ```
        kubectl run --restart=Never --image=alpine -ti --rm testcontainer
        ### Then, once you get a prompt, install curl
        apk add curl
        curl v1-nginx
        ```

4. Change (edit) the service definition to use a label/value of myapp: web
    Edit the YAML manifest of the service with kubectl edit service v1-nginx. Look for the selector: section, and change app: v1-nginx to myapp: web. Make sure to change the selector: section, not the labels: section! After making the change, save and quit.

5. Check that you cannot connect to the exposed service anymore.

6. Change (edit) the deployment definition to add that label/value to the pod template.
    The curl command should now time out. The service can't find a pod with that label yet.

7. Check that you *can* connect to the exposed service again.
    - Edit the YAML manifest of the deployment with kubectl edit deployment v1-nginx. Look for the labels: section within the template: section, as we want to change the labels of the pods created by the deployment, not of the deployment itself. Ignore the matchLabels: one. Add myapp: web just below app: v1-nginx, with the same indentation level. After making the change, save and quit. We need both labels here, unlike the service selector. The app label keeps the pod "linked" to the deployment/replicaset, and the new one will cause the service to match to this pod.
    - The `curl` command should now work again. (It might need a minute, since changing the label will trigger a rolling update and create a new pod.)

## 2
1. Create a deployment running one pod using the official Apache image (httpd).
    ```
    kubectl create deployment v2-apache --image=httpd
    ```
2. Change (edit) the deployment definition to add the label/value picked previously.
    Same as previously: kubectl edit deployment v2-apache, then add the label myapp: web below app: v2-apache. Again, make sure to change the labels in the pod template, not of the deployment itself.
3. Connect to the exposed service again.
    The curl command show now yield responses from NGINX and Apache.
    (Note: you won't see a perfect round-robin, i.e. NGINX/Apache/NGINX/Apache etc., but on average, Apache and NGINX should serve approximately 50% of the requests each.)