# Kubernetes

> Kubernetes management on Linux environment

![screenshot](./app_screenshot.png)

Objectives:
- Knowledge about Kubernete's platform
- Good practices 
- Practical workflow

## Built With

- Major languages
- Frameworks
- Technologies used

## Live Demo

[Live Demo Link](https://livedemo.com)


## Getting Started: Concepts

Container: software package that contains everything the software needs to run. Containers run on top of "container platforms" like Docker. 

    - Namespaces: OS resource views
    - Cgroups: OS resource management
    - Chroot: changes process root directories

Pods: smallest execution unit in Kubernetes. Pods include one or more containers. Pods represent the processes running on a cluster. 

    - Unique IP address
    - Persistent storage volumes
    - Config information that determine how a container should run

![architecture](./architecture.png)

[Kubernetes components](https://kubernetes.io/es/docs/concepts/overview/components/)

Networking:

    - The cluster have the same network segment
    - All nodes must be connected (No NAT)
    - All PODS must have direct connection (No NAT)
    - Container networking interface: specification and libraries for writing plugins to configure network interfaces in Linux containers. Kubernetes uses CNI as an interface between network providers and Kubernetes pod networking.

[Container networking interface](https://rancher.com/docs/rancher/v2.x/en/faq/networking/cni-providers/#:~:text=CNI%20(Container%20Network%20Interface)%2C,with%20a%20number%20of%20plugins.&text=Kubernetes%20uses%20CNI%20as%20an,providers%20and%20Kubernetes%20pod%20networking.)

### Prerequisites

#### Hardware Requirements
One or more machines running one of:

- Ubuntu 16.04+
- Debian 9
- CentOS 7
- RHEL 7
- Fedora 25/26 (best-effort)
- HypriotOS v1.0.1+
- Container Linux (tested with 1800.6.0)

#----------------------------------------------------------------#

- 2 GB or more of RAM per machine
- 2 CPUs or more
- Full network connectivity between all machines in the cluster (public or private network is fine).
- Unique hostname, MAC address, and product_uuid for every node. See here for more details.
- Swap disabled. You MUST disable swap in order for the kubelet to work properly.
- [Certain open ports on your machines](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports)

### Setup

- Update: `sudo apt-get update`
- Install Docker: 
  - `sudo apt install curl vim net-tools openssh-server`
  - `sudo swapoff -a`
  - `sudo add-apt-repository \ "deb [arch=amd64] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) \ stable"`
  - `sudo apt-get update`
  - `sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu`
  - `sudo apt-mark hold docker-ce`
  - `sudo docker version`

### Install

[Minikube](https://minikube.sigs.k8s.io/docs/start/)

[Kubeadm, kubectl & kubelet](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

### Usage

### Run tests

### Deployment

## Authors

👤 **Author1**

- GitHub: [@githubhandle](https://github.com/githubhandle)
- Twitter: [@twitterhandle](https://twitter.com/twitterhandle)
- LinkedIn: [LinkedIn](https://linkedin.com/linkedinhandle)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

Feel free to check the [issues page](issues/).

## Show your support

Give a ⭐️ if you like this project!

## Acknowledgments

- Hat tip to anyone whose code was used
- Inspiration
- etc

## 📝 License

This project is [MIT](lic.url) licensed.
