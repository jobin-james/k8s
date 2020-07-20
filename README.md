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

export ETCDCTL_API=3


When API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don't work. When API version is set to version 3, version 2 commands listed above don't work.


Apart from that, you must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. 
    --cacert /etc/kubernetes/pki/etcd/ca.crt     
    --cert /etc/kubernetes/pki/etcd/server.crt     
    --key /etc/kubernetes/pki/etcd/server.key

Below is the final form:


    kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
