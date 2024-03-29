
# <ul> Kubernetes Master and Worker node Setup </ul>

:octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat:

# <ul> 1)- Master Node Setup </ul>

#### Below ports must be open 

# Installation of Kubernets on AWS EC2 Single Node cluster 

## EC2 size must have 2CPU and 2GB ram

## Below are the steps to install the Kubernets on EC2


### Ports that need to enable 

| Protocal | Direction | Port-Range  | Purpose                 | Used By               |
|----------|-----------|-------------|-------------------------|-----------------------|
| TCP      | Inbound   | 6443*       | Kubernetes API server   | All                   |
| TCP      | Inbound   | 2379- 2380  | etcd server client api  | kube-apiserver , etcd |
| TCP      | Inbound   | 10250       | Kubelet API             | Self control,plane    |
| TCP      | Inbound   | 10251       | Kube-Schedular          | Self                  |
| TCP      | Inbound   | 30000-32767 | Nodeport-services+      | All                   |
| TCP      | Inbound   | 10252       | Kube-controller manager | Self                  |
| TCP      | Ibound    | 22          | SSH                     | Self                  |
| TCP      | Ibnound   | 80          | HTTP                    | Self                  |
| TCP      | Inbound   | 443         | HTTPS                   | Self                  |
| TCP      | Inbound   | 5432        | Postgres                | postgres              |



### Below are the commands to Setup the Master Nodes

#### Step 1. To deactivate a swap space, use the command swapoff

```sh
swapoff -a

````

#### Step 2. Ubuntu 16.04 has reported issues with traffic being routed incorrectly due to iptables being bypassed. Ensure net.bridge.bridge-nf-call-iptables is set to 1 in the sysctl config,

```sh
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

#### Step 3: Installation Of Docker 

```sh
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 
 sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo nano /etc/docker/daemon.json

```
{
"exec-opts":["native.cgroupdriver=systemd"]
}
```

sudo systemctl daemon-reload

sudo systemctl restart docker

```

#### Step 4: Dependecy for kubernetes

```sh

apt-get install -y apt-transport-https curl

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

```

#### Step 5: Installation of Kubelet Kubeadam kubctl

```sh

sudo apt-get update -y

sudo apt-get install -y kubelet kubeadm kubectl

sudo systemctl enable kubelet

sudo systemctl start kubelet

sudo kubeadm init --pod-network-cidr=192.168.0.0/16 /// range of Ip to be given
or
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

mkdir -p $HOME/.kube 

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config


// Copy the token


```

#### Step 6: Setup calico network

```sh

kubectl apply -f https://docs.projectcalico.org/v3.9/manifests/calico.yaml
or
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

```


:octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat:


# <ul> 2)- Worker Node Setup </ul>

#### Below ports must be open 

# Installation of Kubernets on AWS EC2 Single Node cluster 

## EC2 size can be any even free tier.

## Below are the steps to install the Kubernets on EC2


### Ports that need to enable 

| Protocal | Direction | Port-Range  | Purpose                 | Used By               |
|----------|-----------|-------------|-------------------------|-----------------------|
| TCP      | Inbound   | 6443*       | Kubernetes API server   | All                   |
| TCP      | Inbound   | 2379- 2380  | etcd server client api  | kube-apiserver , etcd |
| TCP      | Inbound   | 10250       | Kubelet API             | Self control,plane    |
| TCP      | Inbound   | 10251       | Kube-Schedular          | Self                  |
| TCP      | Inbound   | 30000-32767 | Nodeport-services+      | All                   |
| TCP      | Inbound   | 10252       | Kube-controller manager | Self                  |
| TCP      | Ibound    | 22          | SSH                     | Self                  |
| TCP      | Ibnound   | 80          | HTTP                    | Self                  |
| TCP      | Inbound   | 443         | HTTPS                   | Self                  |
| TCP      | Inbound   | 5432        | Postgres                | postgres              |



### Below are the commands to Setup the Master Nodes

#### Step 1. To deactivate a swap space, use the command swapoff

```sh

swapoff -a

````

#### Step 2. Ubuntu 16.04 has reported issues with traffic being routed incorrectly due to iptables being bypassed. Ensure net.bridge.bridge-nf-call-iptables is set to 1 in the sysctl config,

```sh

cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

#### Step 3: Installation Of Docker 

```sh
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 
 sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo nano /etc/docker/daemon.json

```
{
"exec-opts":["native.cgroupdriver=systemd"]
}
```

sudo systemctl daemon-reload

sudo systemctl restart docker

```

#### Step 4: Dependecy for kubernetes

```sh

sudo apt-get install -y apt-transport-https curl

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

```

#### Step 5: Installation of Kubelet Kubeadam kubctl

```sh

sudo apt-get update -y

sudo apt-get install -y kubelet kubeadm kubectl

sudo systemctl enable kubelet

```

#### Step 6: Paste the token from master node and you will get connected.


:octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat::octocat:

