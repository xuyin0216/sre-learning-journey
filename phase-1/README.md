# 阶段一：基础巩固与思维重塑

## 学习目标
- 阅读SRE书籍
- 学习Kubernetes和Docker基础知识

## 任务清单
- [ ] 阅读SRE书籍
- [ ] 完成Kubernetes课程
- [ ] 编写可靠性日志

## 时间线

| 时间线 | 支柱一：知识 (SRE书籍) | 支柱二：技能 (Kubernetes课程与CKA) | 支柱三：作品集 (GitHub与博客) | 支柱四：应用实践 (问题驱动与可靠性日志) |
|--------|-------------------------|----------------------------------|-----------------------------|----------------------------------|
| 第1周  | 购买书籍，阅读序言及第1-2章 (介绍SRE方法论)。 | 报名课程，预约CKA考试。完成前2个模块 (介绍、核心概念)。 | 创建GitHub仓库及README.md。建立博客并发布“我的旅程”文章。首次提交课程笔记。 | 启动“可靠性日志”：在私人笔记中开始记录日常遇到的环境问题 (问题、影响、时长、根因)。识别1-2个当前工作中的手动部署服务，作为“问题驱动”学习的候选目标。 |
| 第2周  | 阅读第3-4章 (SLO、消除琐事)。 | 完成关于Pod、ReplicaSet、Deployment的模块。 | 推送本周所有实验文件/YAML。撰写并发布第二篇博客：“理解Kubernetes Pods”。 | 持续更新“可靠性日志”。针对候选目标，开始研究并编写初版Dockerfile。将此Dockerfile作为实验代码提交至GitHub的week-2目录。 |
| 第3周  | 阅读第5-6章 (监控、自动化)。 | 完成关于Service、网络、ConfigMap的模块。 | 推送所有实验文件/YAML。撰写并发布第三篇博客：“Kubernetes Service如何暴露应用”。 | 持续更新“可靠性日志”。尝试在本地构建候选服务的Docker镜像并成功运行。在周博文中，可以探讨如何将本周学习的K8s网络概念应用于暴露容器化服务。 |
| 第4周  | 阅读第7-8章 (发布工程、简化)。 | 完成关于存储、Volume、StatefulSet的模块。 | 推送所有实验文件/YAML。撰写并发布第四篇博客：“K8s持久化存储入门指南”。 | 持续更新“可靠性日志”。基于本周学习的知识，为候选服务编写一个基础的Kubernetes Deployment和Service的YAML文件。在本地 (如Minikube/Docker Desktop)尝试部署。将此YAML文件提交至GitHub。 |

## 学习笔记
### SRE 书籍阅读
#### 学习网址
- book: [Site Reliability Engineering](https://sre.google/sre-book/table-of-contents/)
- book: [The Site Reliability Workbook](https://sre.google/workbook/table-of-contents/)
- book: [Seeking SRE](https://www.amazon.com/Seeking-SRE-Practical-Implementing-Services/dp/149197865X)
- book: [The Phoenix Project](https://www.amazon.com/Phoenix-Project-DevOps-Helping-Business/dp/1942788290)
- book: [The DevOps Handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002)
- book: [Accelerate](https://www.amazon.com/Accelerate-Software-Performing-Technology-Organizations/dp/1942788339)
#### 学习笔记
- SRE (Site Reliability Engineering)
### Kubernetes & Docker 学习笔记
#### 学习网址: 
- course lecture: [udemy.com](https://www.udemy.com/course/learn-kubernetes/learn/lecture/22030420#overview) 
- 练习网址: [KodeKcloud](https://uklabs.kodekloud.com/courses/labs-kubernetes-for-the-absolute-beginners-hands-on/) 
#### 学习笔记
#### Containerization
- what is container
    - just like virtual machines, except they all share the same OS kernel, and isolate the application processes from the rest of the system.
    - docker containers sharing the kernel of the host operating system.
        - ** Docker can run any flavor of OS on top of it, as long as they are all based on the same kernel **
- Containers vs Virtual Machines
    - Containers share the host OS kernel, while VMs run a full OS stack.
- How is Containerization done
    - ```docker run ansible```
- Container vs Image
    - Container is a running instance of an image.
    - Image is a read-only template with instructions for creating a container.
#### Container Orchestration
- kubernetes
    - orchestrate (deploy, manage, scale) containerized applications.
    - originally designed by Google, now maintained by CNCF.
- Node
    - A worker machine in Kubernetes, which can be a VM or a physical machine, depending on the cluster. also called a minion.
- Cluster
    - A set of nodes that grouped together.
- Components of Kubernetes
    - Master Node
        - API Server
        - Scheduler
        - Controller Manager
        - etcd
    - Worker Node (hosts container)
        - Kubelet
        - Kube-proxy
        - Container Runtime (Docker, containerd)
- kubectl
    - Command line tool to interact with Kubernetes cluster.
    - deploy applications: ```kubectl run hello-minikube```
    - get cluster info: ```kubectl cluster-info```
    - get nodes: ```kubectl get nodes```
    - get pods: ```kubectl get pods```
#### Docker vs Containerd
- Container Runtime Interface (CRI)
    - An interface that enables the kubelet to use a wide variety of container runtimes, without the need to recompile.
    - container runtimes need to adhere to the OCI (Open Container Iniqtiative) standards.
        - consists of two specifications: the Runtime Specification and the Image Specification.
- Docker shim
    - An adapter that allows Kubernetes to use Docker as a container runtime via the CRI.
    - Deprecated in Kubernetes v1.20 and removed in v1.24.
- containerd (a part of docker)
    - A high-level container runtime that manages the complete container lifecycle of its host system.
    - ctr (used for debugging purpose)
        - A CLI tool for interacting with containerd.
    - nerdctl
        - A Docker-like CLI for containerd.
    - crictl (used for debugging purpose)
        - A CLI tool for CRI-compatible container runtimes.
#### Setting up Kubernetes
- Docker
- minikube (need to have kubectl installed and have virtualization enabled ```grep -E --color 'vmx|svm' /proc/cpuinfo```)
    - A tool that makes it easy to run Kubernetes locally.
    - ```minikube start --driver=docker```
- kubeadm
    - A tool that helps you bootstrap a minimum viable Kubernetes cluster that conforms to best practices.

#### Pod
- The smallest and simplest Kubernetes object. A Pod represents a set of running containers on your cluster
- how to create pod
    - imperative command: ```kubectl run nginx --image=nginx```
    - declarative command: create a YAML file and apply it: ```kubectl apply -f pod.yaml```
    - firstly create a pod and then create a instance of the nginx docker image
- List pods
    - ```kubectl get pods```
    - ```kubectl get po```
    - ```kubectl get pods -o wide```
    - ```kubectl get pods --all-namespaces```
    - ```kubectl get pods -n kube-system```
#### YAML
- dash: indicates a list item
- Dictionary vs List vs List of Dictionaries
    - Dictionary: key-value pairs
    - List: an ordered collection of items
    - List of Dictionaries: a list where each item is a dictionary
- yaml in kubernetes:
    - pod:
        ```
        apiVersion: v1
        kind: Pod
        metadata:   
          name: my-pod
          labels:
            app: my-app
            type: frontend
        spec:
            containers:
            - name: my-container
              image: nginx
        ```
        ```
        kubectl apply -f pod.yaml
        ```
    - ```kubectl run redis --image=redis --dry-run=client -o yaml > redis-pod.yaml```
    - ```kubectl create -f redis-pod.yaml```
- yaml plugin
    - install ```YAML``` plugin in vscode for better experience
    - add ```"yaml.schemas": {
        "kubernetes": "*.yaml"
    },``` in settings.json which means use kubernetes schema to validate all yaml files
#### ReplicaSet
- why ReplicaSet
    - - providing **high availability** by ensuring that a specified number of pod replicas are running at any given time.
    - **load balancing and scaling**: distributing traffic across multiple pod instances and scaling the number of replicas up or down based on demand.
- what is ReplicaSet
    - A ReplicaSet ensures that a specified number of pod replicas are running at any given time.
    - If there are too many pods, it will terminate the extra pods. If there are too few, it will start more pods.
- how to create ReplicaSet
    - create a YAML file and apply it:
        ```apiVersion: apps/v1
        kind: ReplicaSet
        metadata:
          name: my-replicaset
        labels:
          app: my-app
          tye: frontend
        spec:
            replicas: 3
          selector:
            matchLabels:
              app: my-app
          template:
            metadata:
              labels:
                app: my-app
            spec:   
              containers:
              - name: my-container
                image: nginx
        ``` 
    - use kubectl to scale the number of replicas:
        ```
        kubectl scale rs my-replicaset --replicas=5
        kubectl scale --replicas=6 -f replicaset.yaml
        ```
    - commands:
        ```
        kubectl get rs
        kubectl get pods
        kubectl describe rs my-replicaset
        kubectl delete rs my-replicaset
        kubectl delete -f replicaset.yaml
        kubectl replace -f replicaset.yaml
        kubectl scale --replicas=2 -f replicaset.yaml
        ```
#### Deployment
- ```kubctl get all```
- rollout commands
    - ```kubectl rollout status deployment/my-deployment```
    - ```kubectl rollout history deployment/my-deployment```
    - ```kubectl rollout undo deployment/my-deployment --to-revision=2```
- 2 strategies
    - Recreate: kills all existing pods before creating new ones.
    - Rolling Update (default): gradually replaces old pods with new ones, ensuring that some pods are always available during the update process.
- rollback:
    - ```kubectl rollout undo deployment/my-deployment```
    - ```kubectl rollout undo deployment/my-deployment --to-revision=2```
- set image:
    - ```kubectl set image deployment/my-deployment my-container=my-image:tag```
    - ```kubectl set image deployment/my-deployment my-container=my-image:tag --record```
#### Service
- NodePort service:
  - listen to a port on the node and forward traffic to the service.
    - can be accessed from outside the cluster using `<NodeIP>:<NodePort>`.
- ClusterIP service:
  - default type.
  - exposes the service on a cluster-internal IP.
    - can only be accessed from within the cluster.
- LoadBalancer service:
  - exposes the service externally using a cloud provider's load balancer.
    - can be accessed from outside the cluster using the load balancer's IP address.
- how to create service
    - create a YAML file and apply it:
        ```apiVersion: v1
        kind: Service
        metadata:
          name: my-service  
        labels:
            app: my-app
            type: frontend
        spec:
            type: NodePort
            selector:
              app: my-app
            ports:
              - protocol: TCP
                port: 80
                targetPort: 80
                nodePort: 30007
        ```
    - endpoints
        - ```kubectl get endpoints my-service```
        - def: a list of IP addresses and ports that are associated the service will direct traffic to. a sepecifiction of all the pods that are selected by the service's selector.
##### Microservices
- ```kubectl -f . ```
- https://github.com/kodekloudhub/example-voting-app/tree/master/k8s-specifications
#### Kubeadm
- what is kubeadm
    - A tool that helps you bootstrap a minimum viable Kubernetes cluster that conforms to best practices.
    - It performs the actions necessary to get a minimum viable cluster up and running in a user-friendly way.
- Steps
    - create vms using virtualbox or any other hypervisor
        - in windows
            - hypervisor: manages all of the different VMs -> use virtualbox
            - vagrant: manages the lifecycle of VMs -> use vagrant
            - install virtualbox and vagrant
            - create a vagrantfile to define the VMs
            - run ```vagrant up``` to create the VMs
            - run ```vagrant ssh <machine name> ``` to connect to vm
            - run ```vagrant destroy -f``` to tear down
        - in mac os
            - install [multipass](https://canonical.com/multipass)
            - verify installation: ```multipass --version```
            - [github repo](https://github.com/kodekloudhub/certified-kubernetes-administrator-course)
                - ```git clone .```
                - ```cd ~/Documents/Repository/certified-kubernetes-administrator-course/kubeadm-clusters/apple-silicon```
                - ```./deploy-virtual-machines.sh ```

              

    - install docker on all nodes
    - install kubeadm, kubelet and kubectl on all nodes
    - on master node, run ```kubeadm init --pod-network-cidr=10.244.0.0/16```
    - set up pod network
        - flannel: ```kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml```
        - calico: ```kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml```
    - on worker nodes, run the command output from ```kubeadm init``` on master node to join the cluster
    - verify the cluster is up and running: ```kubectl get nodes```

#### etcd
- what is etcd
    - A distributed key-value store that provides a reliable way to store data across a cluster of machines.
    - Used by Kubernetes to store all of its cluster data, including configuration data, state data, and metadata.
- what is a key-value store
    - A simple database that uses a key-value pair to store data.
    - Each key is unique and is used to retrieve the corresponding value.
- how to get started quickly
    - install etcd
    - start etcd server: ```etcd```
- how to interact with etcd
    - use etcdctl CLI tool
    - set a key-value pair: ```etcdctl put mykey "Hello etcd"```
- etcd in kubernetes
    - etcd is used as the primary data store for Kubernetes.
    - stores all of the cluster's state data, including information about nodes, pods, services, and configurations.
    - set up from kubeadm during cluster initialization.
        - ``` kubectl get pods -n kube-system ```
        - ``` kubectl exec etcd-master -n kube-system etcdctl get / --prefix --keys-only   ```
    - set up from scratch
        - download binary
        - create a systemd service file
- Distributed system
- How etcd Operates
    - Consistency
    - Availability
    - Partition Tolerance
- RAFT Protocol
    - Leader Election
    - Log Replication
    - Safety
- Best practices on number of nodes
    - odd number of nodes (3, 5, 7)
    - minimum 3 nodes for production
    - avoid single point of failure
#### kube-api server
- workflow when creating a pod
    - authenticate user
    - validate request
    - retrieve data
    - update etcd
    - notify scheduler
        - schedule pod to a node
    - notify kubelet
        - kubelet pulls image and starts container
- how to view apiserver - kubeadm
    - ```kubectl -n kube-system get pods```
    - ```kubectl -n kube-system describe pod <apiserver-pod-name>```
    - ```kubectl -n kube-system logs <apiserver-pod-name>```
    - ``` cat /etc/kubernetes/manifests/kube-apiserver.yaml ``` or ``` cat /etc/systemd/system/kube-apiserver.service ``` or ps -aux | grep kube-apiserver
#### kube-controller-manager
- what is kube-controller-manager
    - A component of the Kubernetes control plane that runs controller processes.
    - Each controller is a separate process that watches the shared state of the cluster through the API server and makes changes to move the current state towards the desired state.
- types of controllers
    - Node Controller: responsible for noticing and responding when nodes go down.
        - watch status
        - remediate situation
        - node monitor period = 5s
        - node monitor grace period = 40s
        - pod eviction timeout = 5m
    - Replication Controller: ensures that a specified number of pod replicas are running at any given time.
    - Endpoints Controller: populates the Endpoints object (that is, joins Services &
        Pods).
- view kube-controller-manager - kubeadm
    - ```kubectl -n kube-system get pods```
    - ```kubectl -n kube-system describe pod <kube-controller-manager-pod-name>```
    - ```kubectl -n kube-system logs <kube-controller-manager-pod-name>```
    - ``` cat /etc/kubernetes/manifests/kube-controller-manager.yaml ``` or ``` cat /etc/systemd/system/kube-controller-manager.service ``` or ps -aux | grep kube-controller-manager
#### kubelet
    - The kubelet is an agent that runs on each node in the cluster.
    - works by:
        - Register Node
        - Create Pods
        - Monitor Pod and Node Status
    - ** kubeadmin does not deploy kubelets ** 
#### NameSpace
- what is namespace
    - A way to divide cluster resources between multiple users (via resource quota).
    - provides a scope for names. Names of resources need to be unique within a namespace, but not across namespaces.
- types of namespace
    - Default: the default namespace for objects with no other namespace.
    - kube-system: the namespace for objects created by the Kubernetes system.
    - kube-public: a special namespace that is readable by all users (including those not authenticated
        - used for cluster info).
- DNS for Services and Pods
    - Every Service gets a DNS name.
    - Every Pod gets a DNS name.
- DNS resolution format
    - <service-name>.<namespace>.svc.cluster.local
        - cluster.local is the default cluster domain; it can be different based on cluster configuration.
        - svc: indicates that it's a service.
    - <pod-ip-address>.<namespace>.pod.cluster.local
- commands
    - ```kubectl get namespaces```
    - ```kubectl create namespace my-namespace```
    - ```kubectl delete namespace my-namespace```
    - ```kubectl get pods -n my-namespace```
    - ```kubectl run my-pod --image=nginx -n my-namespace   ```
    - ```kubectl get pods -A- ```
    - ```kubectl config set-context --current --namespace=my-namespace```
    - ```kubectl config set-context $(kubectl config current-context) --namespace=my-namespace```

### Tips for CKA Certification
#### kubectl run to create yamls
- You can use the `kubectl run` command to quickly create a Pod and generate the corresponding YAML manifest.
- Example for Pod:
    ```bash
    kubectl run mypod --image=nginx --restart=Never --dry-run=client -o yaml > mypod.yaml
    ```
- This command creates a YAML file (`mypod.yaml`) for a Pod named `mypod` with the Nginx image, without actually deploying it.
- Example for Deployment:
    ```bash
    kubectl create deployment mydeployment --image=nginx --replicas=3 --dry-run=client -o yaml > mydeployment.yaml
    ```
- This command creates a YAML file (`mydeployment.yaml`) for a Deployment named `mydeployment` with 3 replicas of the Nginx image.
- Example for Service:
    ```bash
    kubectl create service nodeport webapp-service --tcp=8080:8080 --node-port=30080  --dry-run=client -o yaml > service-definition-1.yaml
    ```
- This command creates a YAML file (`service-definition-1.yaml`) for a NodePort Service named `webapp-service` that maps port 8080 on the Service to port 8080 on the Pods, with a NodePort of 30080.
- Example for exposing a Pod as a Service:
    ```bash
    kubectl expose pod mypod --type=NodePort --port=80 --dry-run=client -o yaml > myservice.yaml
    kubectl expose pod redis --name=redis-service --port=6379 --target-port=6379 --type=ClusterIP
    ```
- This command creates a YAML file (`myservice.yaml`) for a Service that exposes the Pod `mypod` on port 80 as a NodePort service.
### Scheduler
- what is scheduler
    - A component of the Kubernetes control plane that assigns Pods to Nodes.
    - It watches for newly created Pods that have no Node assigned, and selects a Node for them to run on.
- how scheduler works
    - Filter: the scheduler filters out Nodes that are not suitable for the Pod based on resource requirements, taints, and other constraints.
    - Score: the scheduler scores the remaining Nodes based on various criteria, such as resource availability, affinity/anti-affinity rules, and custom policies.
    - Bind: the scheduler selects the Node with the highest score and binds the Pod to that Node by updating the Pod's specification in the API server.
#### Labels and Selectors
- labels
    - key-value pairs that are attached to objects, such as Pods.
    - used to organize and select subsets of objects.
- selectors
    - used to query and filter objects based on their labels.
    - two types: equality-based and set-based.
    - example:
        - ``` kubectl get pods -l app=nginx```
        - ``` kubectl get pods -l 'env in (production, qa)' ```
        - ``` kubectl get pods -l 'env notin (production, qa)' ```
        - ``` kubectl get pods --selector app=App1,tier=frontend ```
#### Taints and Tolerations
- it only tells nodes to accept pods with matching tolerations. It does not force pods to avoid nodes without matching tolerations.
- taints
    - applied to Nodes to repel Pods that do not tolerate the taint.
    - consists of three parts: key, value, and effect (NoSchedule, PreferNoSchedule, NoExecute).
    - ```kubectl taint nodes node1 key=value:NoSchedule```
- tolerations
    - applied to Pods to allow them to be scheduled on Nodes with matching taints.
    - ```yaml
      tolerations:
      - key: "key"
        operator: "Equal"
        value: "value"
        effect: "NoSchedule"
      ```
#### Node Selectors
- A simple way to constrain Pods to run on specific Nodes based on labels.
- Example:
    - Label a Node:
        ```bash
        kubectl label nodes node1 disktype=ssd
        ```
    - Use Node Selector in Pod spec:
        ```yaml
        spec:
          nodeSelector:     
            disktype: ssd
        ```
- This Pod will only be scheduled on Nodes that have the label `disktype=ssd`.
#### Affinity and Anti-Affinity
- Purpose
    - Affinity: to prefer or require that Pods are scheduled on Nodes with specific characteristics.
    - Anti-Affinity: to avoid scheduling Pods on Nodes with specific characteristics.
- Types
    - Node Affinity/Anti-Affinity: based on Node labels.
    - Pod Affinity/Anti-Affinity: based on labels of other Pods.
- Example of Node Affinity:
    ```yaml
    spec:
      affinity:
        nodeAffinity:       
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: disktype
                operator: In
                values:
                - ssd
    ```
- Example of Pod Anti-Affinity:
    ```yaml
    spec:
      affinity:      
        podAntiAffinity:       
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values:
                - frontend
            topologyKey: "kubernetes.io/hostname"
    ``` 
    - Node Affinity Types
        - requiredDuringSchedulingIgnoredDuringExecution: the Pod must be scheduled on a Node that meets the criteria.
        - preferredDuringSchedulingIgnoredDuringExecution: the scheduler will try to place the Pod on a Node that meets the criteria, but it's not mandatory.
        - requiredDuringSchedulingRequiredDuringExecution: the Pod must be scheduled on a Node that meets the criteria, and if the Node becomes unsuitable, the Pod will be evicted.
#### Resource Requests and Limits
- Resource Requests