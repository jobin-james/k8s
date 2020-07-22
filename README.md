## Kubernetes Certified Administrator Exam preparation.
-------------------------------***********************--------------------------------
### Core Concetps..
*******************
## 1. Cluster Architecture
kubernates cluster consist of nodes, that host applications in the form of containers. --> worker nodes
Managing infirmation regarding the cluster, manage, plan, schedule, monitor nodes  --> control plane --> manager node

- etcd --> a databse, which store information in a key value format
- Scheduler --> identifies the right node to place the continer
- Controllers --> Node controllers --> Resoionsible for managing nodes
- Replication-controller --> Desired number of containers are running.
- Kube-Api server is the primary management component, which orchestrate all operations in the cluster
- Container runtime engine.
- Kubelet --> An agent runs on each node on the cluster listens for instructions from kube api server and deploy or distroy the containers on node as required.
- kube-proxy --> ensure necessary rules in place on the worker nodes and ensure communication between services between nodes
## ETCD - Commands

ETCDCTL is the CLI tool used to interact with ETCD.
ETCDCTL can interact with ETCD Server using 2 API versions - Version 2 and Version 3.  By default its set to use Version 2. Each version has different sets of commands.
For example ETCDCTL version 2 supports the following commands:

    etcdctl backup
    etcdctl cluster-health
    etcdctl mk
    etcdctl mkdir
    etcdctl set


Whereas the commands are different in version 3

    etcdctl snapshot save 
    etcdctl endpoint health
    etcdctl get
    etcdctl put


To set the right version of API set the environment variable ETCDCTL_API command

`export ETCDCTL_API=3`


When API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don't work. When API version is set to version 3, version 2 commands listed above don't work.


Apart from that, you must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. 
    --cacert /etc/kubernetes/pki/etcd/ca.crt     
    --cert /etc/kubernetes/pki/etcd/server.crt     
    --key /etc/kubernetes/pki/etcd/server.key

Below is the final form:


    kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
    
## Kube-api server
- Authenticate user
- validate requests
- Retrive data
- Update ETCD - kube api is the only service inteact with etcd datastore
- Scheduler
- Kubelet
- View api-server - kubeadm  `kubectl get pods -n kube-system`
- View API server Options - kubeadm `cat /etc/kubernates/manifests/kube-apiserver.yaml`
- View API server Options - non kubeadm setup `cat /etc/system/system/kube-apiserver.service`
- View api-server running process to see the effective running options `ps -aux | grep kube-apiserver`

## Kube controller manager
- Manages verious components in kubernates cluster 
- controller is the process if continuiously monitoring the state and bringing the system towords the desired state.
    - Node controller - Monitor the nodes and takes necessary actions --> through kube-api server
        - Node monitor period = 5 seconds
        - Node minitor grace period = 40 seconds
        - POD eviction timeout = 5 minutes (Time for come back online)
    - Replication controller - Resposnible for the state of replicasets and ensure desired number of pords are available all times
- View kube-controller-manager - kubeadm  `kubectl get pods -n kube-system`
- View kube-controller-manager Options - kubeadm `cat /etc/kubernates/manifests/kube-controller-manager.yaml`
- View kube-controller-manager Options - non kubeadm `cat etc/system/system/kube-ontroller-manager.service`
- View kube-controller-manager running process to see the effective running options `ps -aux | grep kube-controller-manager`

## Kube Scheduler
- responsible for deciding which pode goes on which node. Its actually don't place the pod on node. that is the job of kubelet.
    - Filter nodes
    - Rank nodes
- Scheduler config file - `cat /etc/kubernates/config/kube-scheduler.yaml`
- View kube scheduler running process to see the effective running options `ps -aux | grep kube-scheduler`

## Kubelet
- Lead all activity in the node
- Register nodes
- Create POSs
- Monitor nodes and PODs
- `kubeadm doesnot deploy kubelet` Always manually install kubeletes.
- View kubelet  running process to see the effective running options `ps -aux | grep kubelet`

## Kube proxy
- Is a service runs on each node in a cluster
- Looks for new service in a node and create appropriate rule to forword triffic to backend pods if new services are created.
- using ip tables rules
- Deployed as deamonset -> service on each node

## Pod
- Single instance of an application
- smallest object you can create in kubernates
- can run multiple containers in a pod
- get list of pods in a cluster `kubectl get pods`

## Kubernates yaml
Top level properties
```yml
apiVersion:
kind:
metadata


spec:
```

- `apiVersion` -- Depending on what we are trying to create, this will be set (String)
- `kind` -- Type of object (String)
- `metadata` -- Data about the object (Dictionary)
- `spec` -- specification for the object we are going to create.


# Kubernates controllers
- controllers are the brain behind kubernates
- Replication controller
    - replication controller ensures the container high availability
    - `Replication controller` is replcaed by `replica set`
- Replication controller definition file sample

```yml
apiVersion: v1
kind: ReplicationController
metadata:
  name : my-app
  labels:
    app: myapp
    type: web
spec:
 template:
   metadata:
    name: my-app-pod
    labels:
      app: myapp
      environament: staging

  spec:
    containers:
      - name: nginx-container
        image: nginx
 replicas: 3
  
```
- get the replication `kubectl get replicationcontroller`

### Replicaset example
- replicaset can also manage pods which are created without the part of replicaset
