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
 
