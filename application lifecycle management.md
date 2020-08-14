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
