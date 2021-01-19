# Kubernetes

> Kubernetes management on Linux environment

![screenshot](./app_screenshot.png)

Objectives:
- Knowledge about Kubernetes platform
- Good practices 
- Practical workflow

## Built With

- Major languages
- Frameworks
- Technologies used

## Test Application

[Dockercoins](/dockercoins/)


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

Kubernetes service types:

    - ClusterIP: a virtual IP is assigned to service
    - NodePort: a port is assigned to service
    - LoadBalancer: external balancer provided to service
    - ExternalName: DNS entrance managed by CoreDNS

Kubernetes endpoint: object that gets IP addresses of individual pods assigned to it. Then is referenced by a K8S service, so that the service has a record of the internal IPs of PODs, in order to be able to communicate with them.

Kubectl-proxy:

Healthchecks:

    - Liveness:
    - Readiness:
    - ExecAction: execute a command inside the container
    - TCPSocketAction: performs a TCP check against container's IP adress on a specified port
    - HTTPGetAction: performs an HTTP GET request to container's IP


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

[KIND](https://www.youtube.com/watch?v=4p4DqdTDqkk&ab_channel=JustmeandOpensource)

### Usage

- Using Minikube cluster

    `minikube start -p NAME`

    `minikube config`

    `minikube status`

    `minikube dashboard`

    `kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4` Execute container based on docker provided image

    `kubectl get deployments`

    `kubectl get pods -o wide`

    `kubectl describe node NAME`

- Exploring kubectl in kubeadm cluster

    `home/user/.kube/config: definition used by kubectl to connect to the cluster (context)`

    `kubectl describe node NAME`

    `kubectl get nodes -o yaml` Yaml node definition

    `kubectl explain node` Node specification

    `kubectl get secrets -n kube-public`

- Creating and using PODs

    `kubectl run PODNAME --image alpine ping 1.1.1.1` (Deprecated) Create deployment of alpine Docker image running a ping command

    `kubectl get all`

- Scale deployment

    `kubectl scale deploy/pingpong --replicas 8`

- Print POD specifications

    `kubectl run  --dry-run -o yaml PODNAME --image alpine ping 1.1.1.1`

- Access POD through service

    `kubectl create deployment httpenv --image jpetazzo/httpenv`

    `kubectl scale deployment httpenv --replicas=10`

    `curl http://PODIP:8888` // `curl http://PODIP:8888 | jq ""` Check exposed POD

    `kubectl expose deployment httpenv --port=8888` Create service for deployment (Default: ClusterIP)

    `kubectl get services`

    `curl http://SERVICEIP:8888` (Execute several times to check changes on the host)

    `for i in $(seq 10); do curl -s http://SERVICEIP:8888 | jq .HOSTNAME; done`

- Routing traffic using services

    `sudo iptables -t nat -L OUTPUT` 

    `sudo iptables -t nat -nL KUBE_SERVICES` Check routing rules for K8S

    `sudo iptables -t nat -nL SERVICENAME(KUBE_SVC)` List K8S service routing rules (node distribution)

    `sudo iptables -t nat -nL KUBEROUTINGNAME(KUBE-SEP)` Node routing rule

    `kubectl describe service httpenv`

    `kubectl describe endpoints httpenv`

    `kubectl get endpoints httpenv -o yaml`

### Application deployment in Kubernetes (dockercoins)

- Deploy services from images

    `kubectl create deployment redis --image redis`

    `kubectl create deployment hasher --image dockercoins/hasher:v0.1`

    `kubectl create deployment rng --image dockercoins/rng:v0.1`

    `kubectl create deployment webui --image dockercoins/webui:v0.1`

    `kubectl create deployment worker --image dockercoins/worker:v0.1`

    `kubectl logs deploy/DEPLOYNAME`

- Expose services

    `kubectl expose deployment redis --port 6379`

    `kubectl expose deployment rng --port 80`

    `kubectl expose deployment hasher --port 80`

    `kubectl get svc`

- Allow public access through port

    `kubectl expose deploy/webui --type=NodePort --port=80`

    `kubectl get svc`

    `curl http://PUBLICIP:PUBLICPORT/index.html`

- Configure kubectl-proxy

    `kubectl get svc kubernetes`

    `curl -k http://K8SSERVICEIP`

    `less .kube/config` Check certificate

    `kubectl proxy &` Use '&' to send process to background

    `curl -k http://localhost:8001` Access K8S API

    `kubectl port-forward svc/redis 10000:6379 &`

    `telnet localhost 10000`

- Kubernetes dashboard

    `cd /curso-kubernetes/k8s`

    `kubectl apply -f kubernetes-dashboard.yaml` Create K8S dashboard deployment, services

    `kubectl get svc -n kube-system` Check K8S dashboard service

    `curl https://DASHBOARDSERVICEIP:443 -k`

    `kubectl edit service kubernetes-dashboard -n kube-system` Open yaml definition of service for editting

    Edit file --> service 'type: ClusterIP' to 'type: NodePort'

    `kubectl get svc -n kube-system` Get reserved port

    In browser: https://PUBLICIP:DASHBOARDPORT

    `kubectl apply -f grant-admin-to-dashboard.yaml` Grant admin access on dashboard

- LoadBalancer & Daemon Sets

    `kubectl get deploy/rng -o yaml --export > rng.yaml`

    Edit rng.yaml --> 'kind: Deployment' to 'kind: DaemonSet'

    `kubectl apply -f rng.yaml --validate=false`

    `kubectl describe svc rng` Endpoints, labels

    `kubectl get pods --selector=app=rng`

- Controlled deployments: rolling upgrades

    `kubectl get deploy -o json | jq ".items[] | {name:.metadata.name} + .spec.strategy.rollingUpdate`

    `kubectl set image deploy worker worker=dockercoins/worker:v0.2`

    `kubectl rollout undo deploy worker` In case of error updating the deployment

    `kubectl edit deploy worker` Edit deploy configuration

- Healthchecks

    `kubectl edit deploy/redis`

    `Edit:`

        spec:
            containers:
                image: redis
                imagePullPolicy: Always
                name: redis
            --> livenessProbe:
                    exec:
                        command: ["redis-cli", "ping"]

### Bonus

- Managing stacks with Helm

    [Install Helm](https://helm.sh/docs/intro/install/)

    `helm init`

    `kubectl get pods -n kube-system` Check for 'tiller' POD

    `kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default` Give admin permision to Helm server side (tiller)

    `helm search`

    `helm search prometheus`

    `helm inspect stable/prometheus`

    `helm install stable/prometheus --set server.service.type=NodePort --set server.persistentVolume.enabled=false`

    Create Helm chart:

    `helm create dockercoin`

    `cd dockercoin`

    `mv templates/ templates-old`

    `mkdir templates`

    `kubectl get -o yaml --export deployment worker` Do this with all resources deployed for the application to /templates

    `helm install dockercoins`



## Authors

👤 **AiranGlez**

- GitHub: [@githubhandle](https://github.com/AiranGlez)
- LinkedIn: [LinkedIn](www.linkedin.com/in/airanglez)

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
