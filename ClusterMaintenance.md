### OS Upgrades
- pod eviction time out --> time for pod to back online `kube-controller-manger --pod-eviction-timeout=5m0s`
- Drain Nodes `kubcetl drain node01`
- Make node available --> `kubectl uncordon node01`
- Make node unscheduable --> `kubectl cordon node01`
- `kubectl drain node02 --ignore-daemonsets --force`

### Releases
[https://kubernetes.io/docs/concepts/overview/kubernetes-api/](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)


## Cluster Upgrade process
- `kube-apiserver`  server must be at higher version
- `controller-manager` and `kube-scheduler` can be same or one verson lower than `kube-apiserver`
- `kubelet` and `kube-procy` can be 2 version lower or same as `kube-apiserver`
- `kubectl` can be +1 or -1 version as kube-apiserver
- `kubeadm` doesnot install or upgrade `kubelet`
#### Master node Upgrade
-  `apt upgrade -y kubeadm=1.12.0-00`
-  `kubeadm upgrade apply v1.12.0`
- `apt upgrade kubelet=1.12.0-00` 
#### Upgrade worker nodes
- `kubectl drain node01`
- `apt upgrade -y kubeadm=1.12.0-00`
- `apt upgrade kubelet=1.12.0-00` and `systemctl restart kubelet`
- `kubeadm upgrade node config --kubelet-version v1.12.0`
-  `systemctl restart kubelet`
- `kubectl uncordon node01`
