# Scheduling

### Manual scheduling
- what to schedule
- which node to schedule
- bind pod to node(Sheduling) `nodeName: node 01` Property on definition file
- Bind an already created pod to node
  - create a `pod-binding-definition.yml` file
    ```ymml
    apiVersion: v1
    kind: Binding
    metadata:
      name: nginx
    target"
      apiVersion: v1
      kind: Node
      name: node02
    ```
  
  - Then post the binding information to binding api
  ` curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1", "Binding":"....}' http://$SERVER/api/v1/namespaces/default/pod/$PODNAME/binding/`
  
  ### Labels and selectors
  - selectors helps to filter the items
   ```yml
    labels:
      app: app1
      function: front-end
   ```
- view pods with specific lables `kubectl get pods --selector app=App1`
### Taint and tolerations
- Taint applies to node `kubectl taint nodes node-name key=value:taint-effect`
  - taint- effects
    - `NoSchedule`
    - `PreferNoSchedule`
    - `NoExecute`
- Toleration applies to pods
  - Add section in spec called tolerations: All values must be included with doublequotes
    ```yml
    tolerations:
      - key: "app"
        operators: "Equal"
        value: "blue"
        effect: "NoSchedule"
    ```
 
### Node selector
- Decide where to place the pod
- Make sure that the node is already labeled 

```yml
apiVersion: 
kind: Pod
metadata:
  name: myapp
spec:
  containers:
  - name: data-processor
    image: nginx
  nodeSelector:
    size: Large
```

### Node Affinity
```yml
apiVersion: 
kind: Pod
metadata:
  name: myapp
spec:
  containers:
  - name: data-processor
    image: nginx
  affinity:
    requiredDuringSchedulingIgnoreDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: size
          operator: In
          value:
          - Large
```
[Reference](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
- Two types of node affinity
  - `requiredDuringSchedulingIgnoreDuringExecution`
  - `preferredDuringSchedulingIgnoreDuringExecution`


### Resources requirements and limit
- When a pod is created the containers are assigned a default CPU request of .5 and memory of 256Mi". For the POD to pick up those defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.
  - CPU --> 0.5
  - MEM --> 256 Mi
  - 1 count of cpu means 1 core
  - 0.1 count means 100m
  - A countainer cannot use more CPU than its limit --> THROTTLE
  - A container uses more mo=emory than its limit, kubernates terminate the container
  [Reference - Memory Limit](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)
  [Reference - Memory](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource)
  [Reference - CPU Limit](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/)
```yml
    apiVersion: v1
    kind: LimitRange
    metadata:
      name: mem-limit-range
    spec:
      limits:
      - default:
          memory: 512Mi
        defaultRequest:
          memory: 256Mi
        type: Container
```
```
    apiVersion: v1
    kind: LimitRange
    metadata:
      name: cpu-limit-range
    spec:
      limits:
      - default:
          cpu: 1
        defaultRequest:
          cpu: 0.5
        type: Container
```
  
 
### Editing pods and deployments

Remember, you CANNOT edit specifications of an existing POD other than the below.

    spec.containers[*].image

    spec.initContainers[*].image

    spec.activeDeadlineSeconds

    spec.tolerations
    
    
    
- With Deployments you can easily edit any field/property of the POD template. Since the pod template is a child of the deployment specification,  with every change the deployment will automatically delete and create a new pod with the new changes. So if you are asked to edit a property of a POD part of a deployment you may do that simply by running the command

`kubectl edit deployment my-deployment`
