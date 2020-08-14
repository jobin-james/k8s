## Application lifecycle management

### Rolling updates and rollbacks
  - Deployment stratergy
    - Recreate
    - Rolling updates -- default deployment stratergy
    - create `kubectl create -f <deployment file>`
    - get `kubectl get deployments`
    - Update `kubectl apply -f <deployment file>`
    - status `kubectl rollout status deployment/deployment name`
    - rollback `kubectl rollout history deployment/deployment name`
    - `kubectl rollout undo deployment/deployment name`
    
    
### Configure Applications
- Configure commands and arguments on application
- configure environment variables
- configure secrets

## ConfigMaps
- ConfiMaps are used to pass configuration data in the form of key value format
  - `kubectl create configmap <config-name> --from-literal=<key>=<value>`
  
## Secrets
- `kubectl create secret generic <secret-name> --from-literal=DB_HOST=password`
- `kubect get secrets`
- `kubectl describe secrets`
- see the outputs of data
  - `kubectl get secret app-secret -o yaml`
  ```yml
  apiVersion: v1
    kind: Pod
    metadata:
      labels:
        name: webapp-pod
      name: webapp-pod
      namespace: default
    spec:
      containers:
      - image: kodekloud/simple-webapp-mysql
        imagePullPolicy: Always
        name: webapp
        envFrom:
        - secretRef:
            name: db-secretx`
  ```
 
### Multi container pod
- side car pattern, adapter and the ambassador pattern. 
  ```yml
  apiVersion: v1
    kind: Pod
    metadata:
      labels:
        name: webapp-pod
      name: webapp-pod
    spec:
      containers:
      - name: app-1
        image: kodekloud/simple-webapp-mysql
      - name: app-1
        image: kodekloud/simple-webapp-mysql
   ```
## InitContainers
 - In a multi-container pod, each container is expected to run a process that stays alive as long as the POD's lifecycle. For example in the multi-container pod that we talked about earlier that has a web application and logging agent, both the containers are expected to stay alive at all times. The process running in the log agent container is expected to stay alive as long as the web application is running. If any of them fails, the POD restarts.
- An initContainer is configured in a pod like all other containers, except that it is specified inside a initContainers section,  like this:
```yml
    apiVersion: v1
    kind: Pod
    metadata:
      name: myapp-pod
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: busybox:1.28
        command: ['sh', '-c', 'echo The app is running! && sleep 3600']
      initContainers:
      - name: init-myservice
        image: busybox
        command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ; done;']
```
[initContainers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

