### Kubernetes Pre-requisites and Requirements

    Physical Processor Core : minimum 2 core
    $ cat /proc/cpuinfo | grep -i processor
    Physical Memory: 4 GB minimum
    $ free -h
    Trun Off Swap memory
    $ sudo swapoff -a
    Add Host entry (master, nodeone & nodesecond)
    $ vi /etc/hosts
    zahir@master:~$ cat /etc/hosts
    127.0.0.1 localhost
    172.16.X.X master
    172.16.X.X nodeone
    172.16.X.X nodesecond

### >>>Kubernetes Installation Steps<<<

    $ sudo apt-get update && sudo apt-get install -y apt-transport-https curl
    $ wget https://packages.cloud.google.com/apt/doc/apt-key.gpg
    $ sudo mv apt-key.gpg /etc/apt/trusted.gpg.d/
    $ sudo apt-add-repository 'deb http://apt.kubernetes.io/ kubernetes-xenial main'
    $ sudo apt-get update && sudo apt-get install -y kubelet=1.20.0-00 kubeadm=1.20.0-00 kubectl=1.20.0-00 docker.io
    $ sudo systemctl start docker && sudo systemctl enable docker
    $ sudo kubeadm init --apiserver-advertise-address=172.16.5.17 --pod-network-cidr=10.0.0.0/16
    Your Kubernetes control-plane has initialized successfully!
    To start using your cluster, you need to run the following as a regular user:
    $ mkdir -p $HOME/.kube
    $ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    $ sudo chown $(id -u):$(id -g) $HOME/.kube/config
    Alternatively, if you are the root user, you can run:
    $ export KUBECONFIG=/etc/kubernetes/admin.conf

    You should now deploy a pod network to the cluster. - Run this command in Master Node:
    Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:https://kubernetes.io/docs/concepts/cluster-administration/addons/
    $ kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml

##### Run this command in worker nodes
    Then you can join any number of worker nodes by running the following on each as root:
    $ kubeadm join 172.16.5.17:6443 --token amz0ii.1kpvdcfjyd5we7xa \
        --discovery-token-ca-cert-hash sha256:0b5631cdb96e4973c997cf49e19f8d4168b8c341e8b7c6f615c628d3e

##### To get node status
    $ kubectl get nodes

##### To get node details    
    $ kubectl describe node nodesecond

