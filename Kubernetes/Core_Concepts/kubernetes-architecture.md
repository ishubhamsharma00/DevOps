Master-Slave Architecture

1. Master node (control-plane) has: It manages the work
- API Server
    => Acts as a cluster gate-way
    => Does Authentications
    => Responsible for over-all intra-communication between processes.
- Scheduler
    => Schedules pods on nodes. Decides where to put pod among various nodes.
    => It actually requests kubelet to schedule pod/s.
- Controller Manager
    => Keeps in check if everything is working fine or not.
    => Reschedules died/terminated pods.
    => Detects cluster state changes.
- etcd
    => key-value store of cluster state.
    => All cluster info is stored in it.

2. Worker node has: It does the real work
- kubelet
    => Interacts with both container and node
    => Actually starts the pod with a container inside
    => Interacts with API Server
- Container Runtime/Docker containers
- Service Proxy/ kube-proxy
    => Forwards the request from services to pods

3. Kubectl
    => kube-controller.
    => Users use kubectl to give commands to cluster

4. CNI - Container Network Interface