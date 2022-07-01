# CORE CONCEPTS
-------------------
### Delete pod using kubectl command 
<details><summary>show</summary>
  
  ```bash
  kubectl delete pod frontend
  ```
  
</details>

### Delete pod created by specific YAML file 
<details><summary>show</summary>  
  ```bash
  kubectl delete -f pod.yaml
  ```  
</details>

### Use Hybrid approach to create a pod 
<details><summary>show</summary>
  <p>
  ```bash
  kubectl run frontend --image=nginx --restart=Never --port=80 \
  -o yaml --dry-run=client > pod.yaml
  ```
  ```bash
  kubectl create -f pod.yaml
  ```
  </p>
</details>


### Delete specific pod
<details><summary>show</summary>
  <p>
  ```bash
  kubectl delete pod frontend
  ```
  </p>
</details>

### Delete a pod using declarative approach
<details><summary>show</summary>
  <p>
  ```bash
  kubectl delete -f pod.yaml
  ```
  </p>
</details>

### Edit a live object
<details><summary>show</summary>
  <p>
  ```bash
  kubectl edit pod frontend
  ```
  </p>
</details>

### Replace a live object
<details><summary>show</summary>
  <p>
  ```bash
  kubectl replace -f pod.yaml
  ```
  </p>
</details>

### Update a live object
<details><summary>show</summary>
  <p>
  ```bash
  kubectl apply -f pod.yaml
  ```
  </p>
</details>

### Translate this kubectl command into a YAML file:
<p>
```bash
  kubectl run hazelcast --image=hazelcast/hazelcast --restart=Never --port=5701 --env="DNS_DOMAIN=cluster" --labels="app=hazelcast,env=prod"
```
  
</p>
<details><summary>show</summary>
  <p>
  ```bash
  apiVersion: v1
  kind: Pod
  metadata:
  name: hazelcast
  labels:
    app: hazelcast
    env: prod
  spec:
  containers:
  - env:
    - name: DNS_DOMAIN
      value: cluster
  image: hazelcast/hazelcast
  name: hazelcast
  ports:
  - containerPort: 5701
  restartPolicy: Never
  ```
  </p>
</details>

### List pods
<details><summary>show</summary>
  <p>
  ```bash
  kubectl get pods
  ```
  </p>
</details>

### List a specific pod
<details><summary>show</summary>
  <p>
  ```bash
  kubectl get pods hazelcast
  ```
  </p>
</details>

### Render pod details
<details><summary>show</summary>
  <p>
  ```bash
  kubectl describe pods hazelcast
  ```
  </p>
</details>

### Render specific POD details using grep
<details><summary>show</summary>
  <p>
  ```bash
  kubectl describe pods hazelcast | grep Image:
  ```
  </p>
</details>

### Modify this YAML manifest for a Pod by adding the following environment variables:
```bash
SPRING_PROFILES_ACTIVE=prod
VERSION='1.5.3'
```

Initial YAML

```bash
apiVersion: v1
kind: Pod
metadata:
  name: spring-boot-app
spec:
  containers:
  - image: bmuschko/spring-boot-app:1.5.3
    name: spring-boot-app
```

<details><summary>show</summary>
  <p>
  ```bash
  apiVersion: v1
  kind: Pod
  metadata:
    name: spring-boot-app
  spec:
    containers:
    - image: bmuschko/spring-boot-app:1.5.3
      name: spring-boot-app
      env:
      - name: SPRING_PROFILES_ACTIVE
        value: prod
      - name: VERSION
        value: '1.5.3'
  ```
  </p>
</details>

### Run this command using the YAML manifest and the args attribute: "while true; do date; sleep 10; done"
<details><summary>show</summary>
  <p>
  ```bash
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
    - args:
    - /bin/sh
    - -c
    - while true; do date; sleep 10; done
    image: busybox
    name: mypod
  restartPolicy: Never
  ```
  </p>
</details>

### Run this command using the YAML manifest and command and args attribute: "while true; do date; sleep 10; done"
<details><summary>show</summary>
  <p>
  ```bash
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
    - command: ["/bin/sh"]
    args: ["-c", "while true; do date; sleep 10; done"]
    image: busybox
    name: mypod
  restartPolicy: Never
  ```
  To validate
  ```bash
  kubectl create -f pod.yaml
  kubectl logs mypod -f
  ```
  </p>
</details>

### List all namespaces
<details><summary>show</summary>
  <p>
  ```bash
  kubectl get namespaces
  ```
  </p>
</details>

### Create a namespace
<details><summary>show</summary>
  <p>
  ```bash
  kubectl create namespace code-red
  ```
  </p>
</details>

### Delete a namespace
<details><summary>show</summary>
  <p>
  ```bash
  kubectl delete namespace code-red
  ```
  </p>
</details>

### Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace
<details><summary>show</summary>
  <p>
  ```bash
  kubectl create namespace mynamespace
  kubectl run nginx --image=nginx --restart=Never -n mynamespace
  ```
  </p>
</details>

### Create the pod that was just described using YAML
<details><summary>show</summary>
  <p>
  Easily generate YAML with:
  ```bash
  kubectl run nginx --image=nginx --restart=Never --dry-run=client -n mynamespace -o yaml > pod.yaml
  ```

  ```bash
  cat pod.yaml
  ```
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: nginx
    name: nginx
    namespace: mynamespace
  spec:
    containers:
    - image: nginx
      imagePullPolicy: IfNotPresent
      name: nginx
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Never
  status: {}
  ```

  ```bash
  kubectl create -f pod.yaml
  ```
  Alternatively, you can run in one line
  ```bash
  kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml | kubectl create -n mynamespace -f -
  ```
  </p>
</details>

### Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output
<details><summary>show</summary>
  <p>
  ```bash
  kubectl run busybox --image=busybox --command --restart=Never -it --rm -- env # -it will help in seeing the output, --rm will immediately delete the pod after it exits
  # or, just run it without -it
  kubectl run busybox --image=busybox --command --restart=Never -- env
  # and then, check its logs
  kubectl logs busybox
  ```
  </p>
</details>

### Create a busybox pod (using YAML) that runs the command "env". Run it and see the output
<details><summary>show</summary>
<p>
```bash
# create a  YAML template with this command
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- env > envpod.yaml
# see it
cat envpod.yaml
```

```YAML
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - command:
    - env
    image: busybox
    name: busybox
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```bash
# apply it and then see the logs
kubectl apply -f envpod.yaml
kubectl logs busybox
```
</p>
</details>

### Get the YAML for a new namespace called 'myns' without creating it
<details><summary>show</summary>
  <p>
  ```bash
  kubectl create namespace myns -o yaml --dry-run=client
  ```
  </p>
</details>

### Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it
<details><summary>show</summary>
  <p>
  ```bash
  kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run=client -o yaml
  ```
  </p>
</details>

### Get pods on all namespaces
<details><summary>show</summary>
  <p>
  ```bash
  kubectl get po --all-namespaces
  ```
  Alternatively 

  ```bash
  kubectl get po -A
  ```
  </p>
</details>

### Create a pod with image nginx called nginx and expose traffic on port 80
<details><summary>show</summary>
  <p>
  ```bash
  kubectl run nginx --image=nginx --restart=Never --port=80
  ```
  </p>
</details>

### Change pod's image to nginx:1.7.1. Observe that the container will be restarted as soon as the image gets pulled
<details><summary>show</summary>
<p>

*Note*: The `RESTARTS` column should contain 0 initially (ideally - it could be any number)

```bash
# kubectl set image POD/POD_NAME CONTAINER_NAME=IMAGE_NAME:TAG
kubectl set image pod/nginx nginx=nginx:1.7.1
kubectl describe po nginx # you will see an event 'Container will be killed and recreated'
kubectl get po nginx -w # watch it
```

*Note*: some time after changing the image, you should see that the value in the `RESTARTS` column has been increased by 1, because the container has been restarted, as stated in the events shown at the bottom of the `kubectl describe pod` command:

```
Events:
  Type    Reason     Age                  From               Message
  ----    ------     ----                 ----               -------
[...]
  Normal  Killing    100s                 kubelet, node3     Container pod1 definition changed, will be restarted
  Normal  Pulling    100s                 kubelet, node3     Pulling image "nginx:1.7.1"
  Normal  Pulled     41s                  kubelet, node3     Successfully pulled image "nginx:1.7.1"
  Normal  Created    36s (x2 over 9m43s)  kubelet, node3     Created container pod1
  Normal  Started    36s (x2 over 9m43s)  kubelet, node3     Started container pod1
```

*Note*: you can check pod's image by running

```bash
kubectl get po nginx -o jsonpath='{.spec.containers[].image}{"\n"}'
```

</p>
</details>

### Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'
<details><summary>show</summary>
<p>

```bash
kubectl get po -o wide # get the IP, will be something like '10.1.1.131'
# create a temp busybox pod
kubectl run busybox --image=busybox --rm -it --restart=Never -- wget -O- 10.1.1.131:80
```

Alternatively you can also try a more advanced option:

```bash
# Get IP of the nginx pod
NGINX_IP=$(kubectl get pod nginx -o jsonpath='{.status.podIP}')
# create a temp busybox pod
kubectl run busybox --image=busybox --env="NGINX_IP=$NGINX_IP" --rm -it --restart=Never -- sh -c 'wget -O- $NGINX_IP:80'
``` 

Or just in one line:

```bash
kubectl run busybox --image=busybox --rm -it --restart=Never -- wget -O- $(kubectl get pod nginx -o jsonpath='{.status.podIP}:{.spec.containers[0].ports[0].containerPort}')
```

</p>
</details>

### Get pod's YAML
<details><summary>show</summary>
  <p>
  ```bash
  kubectl get po nginx -o yaml
  # or
  kubectl get po nginx -oyaml
  # or
  kubectl get po nginx --output yaml
  # or
  kubectl get po nginx --output=yaml
  ```
  </p>
</details>

### Get information about the pod, including details about potential issues (e.g. pod hasn't started)
<details><summary>show</summary>
  <p>
  ```bash
  kubectl describe po nginx
  ```
  </p>
</details>

### Get pod logs
<details><summary>show</summary>
  <p>
  ```bash
  kubectl logs nginx
  ```
  </p>
</details>

### If pod crashed and restarted, get logs about the previous instance
<details><summary>show</summary>
  <p>
  ```bash
  kubectl logs nginx -p
  # or
  kubectl logs nginx --previous
  ```
  </p>
</details>

### Execute a simple shell on the nginx pod
<details><summary>show</summary>
  <p>
  ```bash
  kubectl exec -it nginx -- /bin/sh
  ```
  </p>
</details>

### Create a busybox pod that echoes 'hello world' and then exits
<details><summary>show</summary>
  <p>
  ```bash
  kubectl run busybox --image=busybox -it --restart=Never -- echo 'hello world'
  # or
  kubectl run busybox --image=busybox -it --restart=Never -- /bin/sh -c 'echo hello world'
  ```
  </p>
</details>

### Do the same, but have the pod deleted automatically when it's completed
<details><summary>show</summary>
  <p>
  ```bash
  kubectl run busybox --image=busybox -it --rm --restart=Never -- /bin/sh -c 'echo hello world'
  kubectl get po # nowhere to be found :)
  ```
  </p>
</details>

### Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod
<details><summary>show</summary>
<p>

```bash
kubectl run nginx --image=nginx --restart=Never --env=var1=val1
# then
kubectl exec -it nginx -- env
# or
kubectl exec -it nginx -- sh -c 'echo $var1'
# or
kubectl describe po nginx | grep val1
# or
kubectl run nginx --restart=Never --image=nginx --env=var1=val1 -it --rm -- env
```

</p>
</details>
