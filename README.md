## Kubernetes Certified Administrator Exam preparation.
-------------------------------***********************--------------------------------
### Core Concetps..
*******************
## 1. Cluster Architecture
kubernates cluster consist of nodes, that host applications in the form of containers. --> worker nodes
Managing infirmation regarding the cluster, manage, plan, schedule, monitor nodes  --> control plane --> manager node

etcd --> a databse, which store information in a key value format

Scheduler --> identifies the right node to place the continer

Controllers --> Node controllers --> Resoionsible for managing nodes
Replication-controller --> Desired number of containers are running.


Kube-Api server is the primary management component, which orchestrate all operations in the cluster

Container runtime engine.

Kubelet --> An agent runs on each node on the cluster listens for instructions from kube api server and deploy or distroy the containers on node as required.

kube-proxy --> ensure necessary rules in place on the worker nodes and ensure communication between services between nodes
