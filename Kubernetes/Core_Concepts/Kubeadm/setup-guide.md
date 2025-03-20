- launch as many instances as required node in cluster 
- expose port 6443 in Security Group to allow worker nodes to join the cluster

<!-- Commands to exeute on both Master and Worker nodes -->
- sudo swapoff -a

- Load necessary Kernel Modules required for kubernetes networking:
    cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
    overlay
    br_netfilter
    EOF
    sudo modprobe overlay
    sudo modprobe br_netfilter 

- Set sysctl parameters that helps with networking:
    cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-iptables = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    net.ipv4.ip_forward = 1
    EOF
    sudo sysctl --system
    lsmod | grep br_netfilter
    lsmod | grep overlay

- Install containerd
- sudo systemctl restart containerd
- sudo systemctl status containerd
- Install kubelet, kubeadm, kubectl

<!-- Execute only on Master Node -->
- sudo kubeadm init
- Setup Local kubeconfig
    mkdir -p "$HOME"/.kube
    sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
    sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config
- Install a network plugin (Calico):
    kubectl apply -f https://raw.githubusercontent.com/projectcalico/v3.26.0/manifests/calico.yaml
- Generate join command:
    kubeadm token create --print-join-command

<!-- Execute on Worker Nodes -->
- sudo kubeadm reset pre-flight checks
- Paste the join command from the master node with --v=5 at the end