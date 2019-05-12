
# Kubernetes Cluster

쿠버네티스 클러스터를 구성하기 위하여 VM에 있는 minikube를 사용하도록 한다.

먼저 클러스터가 구성되어 있는지 살펴본다.

~~~
$ kubectl cluster-info

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
The connection to the server 10.0.2.15:8443 was refused - did you specify the right host or port?
~~~

현재 클러스터 구성이 되어있지 않음을 알 수 있다. 그래서 minikube를 이용하여 쿠버네티스 클러스터를 구성한다.

~~~
$ sudo -E minikube start --vm-driver=none

[sudo] password for user1: 
😄  minikube v1.0.1 on linux (amd64)
💡  Tip: Use 'minikube start -p <name>' to create a new cluster, or 'minikube delete' to delete this one.
🔄  Restarting existing none VM for "minikube" ...
⌛  Waiting for SSH access ...
📶  "minikube" IP address is 10.0.2.15
🐳  Configuring Docker as the container runtime ...
🐳  Version of container runtime is 18.09.6
✨  Preparing Kubernetes environment ...
❌  Unable to load cached images: loading cached images: loading image /home/user1/.minikube/cache/images/k8s.gcr.io/kube-proxy_v1.14.1: stat /home/user1/.minikube/cache/images/k8s.gcr.io/kube-proxy_v1.14.1: no such file or directory
🚜  Pulling images required by Kubernetes v1.14.1 ...
🔄  Relaunching Kubernetes v1.14.1 using kubeadm ... 
⌛  Waiting for pods: apiserver proxy etcd scheduler controller dns
📯  Updating kube-proxy configuration ...
🤔  Verifying component health ......
🤹  Configuring local host environment ...

⚠️  The 'none' driver provides limited isolation and may reduce system security and reliability.
⚠️  For more information, see:
👉  https://github.com/kubernetes/minikube/blob/master/docs/vmdriver-none.md

💗  kubectl is now configured to use "minikube"
🏄  Done! Thank you for using minikube!
~~~

잘 구성되었는지 살펴본다.
~~~
$ kubectl cluster-info

Kubernetes master is running at https://10.0.2.15:8443
KubeDNS is running at https://10.0.2.15:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
~~~

위와 같이 잘 구성되었음을 알 수 있다.

# kubectl get nodes

현재 쿠버네티스 클러스터의 노드를 살펴보는 명령어이다.
~~~
$ kubectl get nodes

NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   2d10h   v1.14.1
~~~

현재 한개의 노드가 존재한다.  
minikube는 단일 노드의 쿠버네티스 환경이다.  만약 클라우드 벤더를 사용하거나 멀티노드로 구성을 한다면 노드의 리스트가 여려개 보여진다.



# kubectl describe nodes

자세한 정보를 보려면 describe 옵션을 준다.
~~~
$ kubectl describe node minikube

Name:               minikube
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=minikube
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Fri, 10 May 2019 01:44:18 +0900
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Sun, 12 May 2019 12:11:13 +0900   Fri, 10 May 2019 01:48:28 +0900   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Sun, 12 May 2019 12:11:13 +0900   Fri, 10 May 2019 01:48:28 +0900   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Sun, 12 May 2019 12:11:13 +0900   Fri, 10 May 2019 01:48:28 +0900   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Sun, 12 May 2019 12:11:13 +0900   Fri, 10 May 2019 01:48:38 +0900   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  InternalIP:  10.0.2.15
  Hostname:    minikube
Capacity:
 cpu:                2
 ephemeral-storage:  32961296Ki
 hugepages-2Mi:      0
 memory:             8168088Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  30377130344
 hugepages-2Mi:      0
 memory:             8065688Ki
 pods:               110
System Info:
 Machine ID:                 2f1155c546ff4a35b4cb86e074ebb62e
 System UUID:                94ae9e50-1ea9-4272-bdb2-8c8e0b263fe7
 Boot ID:                    39810bad-7053-49bc-b4a6-346816a7b228
 Kernel Version:             4.18.0-18-generic
 OS Image:                   Ubuntu 18.04.2 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.9.6
 Kubelet Version:            v1.14.1
 Kube-Proxy Version:         v1.14.1
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                ------------  ----------  ---------------  -------------  ---
  kube-system                coredns-fb8b8dccf-x85pq             100m (5%)     0 (0%)      70Mi (0%)        170Mi (2%)     2d10h
  kube-system                coredns-fb8b8dccf-zlzls             100m (5%)     0 (0%)      70Mi (0%)        170Mi (2%)     2d10h
  kube-system                etcd-minikube                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         2d10h
  kube-system                kube-addon-manager-minikube         5m (0%)       0 (0%)      50Mi (0%)        0 (0%)         2d10h
  kube-system                kube-apiserver-minikube             250m (12%)    0 (0%)      0 (0%)           0 (0%)         2d10h
  kube-system                kube-controller-manager-minikube    200m (10%)    0 (0%)      0 (0%)           0 (0%)         2d10h
  kube-system                kube-proxy-vtczr                    0 (0%)        0 (0%)      0 (0%)           0 (0%)         46h
  kube-system                kube-scheduler-minikube             100m (5%)     0 (0%)      0 (0%)           0 (0%)         2d10h
  kube-system                storage-provisioner                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         2d10h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                755m (37%)  0 (0%)
  memory             190Mi (2%)  340Mi (4%)
  ephemeral-storage  0 (0%)      0 (0%)
Events:
  Type    Reason                   Age                    From                  Message
  ----    ------                   ----                   ----                  -------
  Normal  Starting                 2d10h                  kubelet, minikube     Starting kubelet.
  Normal  NodeAllocatableEnforced  2d10h                  kubelet, minikube     Updated Node Allocatable limit across pods
  Normal  NodeHasSufficientMemory  2d10h (x8 over 2d10h)  kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    2d10h (x8 over 2d10h)  kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     2d10h (x7 over 2d10h)  kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  Starting                 2d10h                  kube-proxy, minikube  Starting kube-proxy.
  Normal  Starting                 2d10h                  kubelet, minikube     Starting kubelet.
  Normal  NodeHasSufficientMemory  2d10h (x2 over 2d10h)  kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    2d10h (x2 over 2d10h)  kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     2d10h (x2 over 2d10h)  kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  NodeNotReady             2d10h                  kubelet, minikube     Node minikube status is now: NodeNotReady
  Normal  NodeAllocatableEnforced  2d10h                  kubelet, minikube     Updated Node Allocatable limit across pods
  Normal  Starting                 2d10h                  kube-proxy, minikube  Starting kube-proxy.
  Normal  NodeReady                2d10h                  kubelet, minikube     Node minikube status is now: NodeReady
  Normal  Starting                 46h                    kubelet, minikube     Starting kubelet.
  Normal  NodeAllocatableEnforced  46h                    kubelet, minikube     Updated Node Allocatable limit across pods
  Normal  NodeHasSufficientMemory  46h (x8 over 46h)      kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    46h (x8 over 46h)      kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     46h (x7 over 46h)      kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  Starting                 46h                    kube-proxy, minikube  Starting kube-proxy.
  Normal  Starting                 10m                    kubelet, minikube     Starting kubelet.
  Normal  NodeHasSufficientMemory  10m (x8 over 10m)      kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    10m (x8 over 10m)      kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     10m (x7 over 10m)      kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  NodeAllocatableEnforced  10m                    kubelet, minikube     Updated Node Allocatable limit across pods
  Normal  Starting                 9m43s                  kube-proxy, minikube  Starting kube-proxy.
~~~


# kubectl run {앱이름} -image={이미지명}
docker 와 마찬가지로 가장쉽게 컨테이너를 쉽게 실행하는 방법은 run 옵션이다.

~~~
$ kubectl run hello --image=hello-world

kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello created
~~~

hello 라는 이름의 애플리케이션으로 hello-world 라는 도커 이미지를 실행한 것이라는 의미이다.  
그러나 실제로 hello-world 문구가 출력이 되지 않았다. stdout의 출력은 logs 옵션으로 살펴볼 수 있다.

# kubectl logs {앱이름}
~~~
$ docker logs hello

hello world
~~~

# kubectl get pods **or** kubectl get po

컨테이너는 pod 이라는 가장 작은 단위로 담겨서 쿠버네티스에서 수행이 된다. 현재 pod 을 살펴보자.
~~~
$ kubectl get po

NAME                     READY   STATUS             RESTARTS   AGE
hello-56cd858d87-gbbpc   0/1     CrashLoopBackOff   4          3m6s
~~~

현재 이름이 hello-56cd858d87-gbbpc 라는 이름으로 pod 이 생성되었으나  hello-world만 출력하고 죽는 컨테이너라 running중은 아니다.

# kubectl describe pod {POD이름}

특정 pod에 대한 자세한 내용을 보려면 describe 옵션을 사용한다.

~~~
$ kubectl describe pod hello-56cd858d87-gbbpc 

Name:               hello-56cd858d87-gbbpc
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               minikube/10.0.2.15
Start Time:         Sun, 12 May 2019 12:21:05 +0900
Labels:             pod-template-hash=56cd858d87
                    run=hello
Annotations:        <none>
Status:             Running
IP:                 172.17.0.4
Controlled By:      ReplicaSet/hello-56cd858d87
Containers:
  hello:
    Container ID:   docker://3564e3f49da312aaed7f55fd9d037ae90b0096a74dfb30c5a202b2f14f2983e2
    Image:          hello-world
    Image ID:       docker-pullable://hello-world@sha256:5f179596a7335398b805f036f7e8561b6f0e32cd30a32f5e19d17a3cda6cc33d
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Sun, 12 May 2019 12:37:45 +0900
      Finished:     Sun, 12 May 2019 12:37:45 +0900
    Ready:          False
    Restart Count:  8
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-pf9pl (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  default-token-pf9pl:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-pf9pl
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  17m                   default-scheduler  Successfully assigned default/hello-56cd858d87-gbbpc to minikube
  Normal   Pulling    16m (x4 over 17m)     kubelet, minikube  Pulling image "hello-world"
  Normal   Pulled     16m (x4 over 17m)     kubelet, minikube  Successfully pulled image "hello-world"
  Normal   Created    16m (x4 over 17m)     kubelet, minikube  Created container hello
  Normal   Started    16m (x4 over 17m)     kubelet, minikube  Started container hello
  Warning  BackOff    2m11s (x70 over 17m)  kubelet, minikube  Back-off restarting failed container
~~~

출력된 정보로 알 수 있는 내용들이다.

- 수행중인 컨테이너의 내부 아이피는 172.17.0.4 이다.
- ReplicaSet으로 애플리케이션이 컨트롤 된다. 이름은 hello-56cd858d87이다.
- 이미지는 hello-world 이다.
- 그 외 다수


# kubectl get deployments

pod은 실제로 수행중인 컨테이너이고, 이를 수행하기 위해서 배포된 앱을 살펴보자

~~~
$ kubectl get deployments

NAME    READY   UP-TO-DATE   AVAILABLE   AGE
hello   0/1     1            0           14m
~~~

# kubectl describe deployment {앱이름}

~~~
$ kubectl describe deployment hello

Name:                   hello
Namespace:              default
CreationTimestamp:      Sun, 12 May 2019 12:21:05 +0900
Labels:                 run=hello
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               run=hello
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  run=hello
  Containers:
   hello:
    Image:        hello-world
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    False   ProgressDeadlineExceeded
OldReplicaSets:  <none>
NewReplicaSet:   hello-56cd858d87 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  25m   deployment-controller  Scaled up replica set hello-56cd858d87 to 1
~~~

# kubectl get replicaset **or** kubectl get rs

ReplicaSet으로 등록된 리스트가 나타난다.  기본적으로 run 옵션을 사용하여 수행하면 ReplicaSet 으로 등록된다.
~~~
$ kubectl get rs

NAME               DESIRED   CURRENT   READY   AGE
hello-56cd858d87   1         1         0       19m
~~~

# kubectl describe rs {RC이름}

ReplicaSet에 대한 자세한 정보가 나타난다.
~~~
$ kubectl describe rs hello-56cd858d87

Name:           hello-56cd858d87
Namespace:      default
Selector:       pod-template-hash=56cd858d87,run=hello
Labels:         pod-template-hash=56cd858d87
                run=hello
Annotations:    deployment.kubernetes.io/desired-replicas: 1
                deployment.kubernetes.io/max-replicas: 2
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/hello
Replicas:       1 current / 1 desired
Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  pod-template-hash=56cd858d87
           run=hello
  Containers:
   hello:
    Image:        hello-world
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  22m   replicaset-controller  Created pod: hello-56cd858d87-gbbpc
~~~

# kubectl delete po {POD이름}

POD을 삭제하기 위해서는 delete po 를 사용한다. 하지만,  PO는 컨테이너가 수행되는 주체이고 이는 상세정보에서도 알 수 있듯이 ReplicaSet 의해서 관리된다.

~~~
$ kubectl get po
NAME                     READY   STATUS             RESTARTS   AGE
hello-56cd858d87-gbbpc   0/1     CrashLoopBackOff   10         29m

$ kubectl delete po hello-56cd858d87-gbbpc
pod "hello-56cd858d87-gbbpc" deleted

$ kubectl get po
NAME                     READY   STATUS      RESTARTS   AGE
hello-56cd858d87-rmb7s   0/1     Completed   0          10s
~~~

위와 같이 pod를 삭제해도 또 생긴다. 이것은 ReplicaSet 에 의해서 1개의 pod을 계속적으로 유지하기 때문에 삭제된 pod를 대신해서 새로운 pod이 생성된다.

# kubectl delete deployment {앱이름}

실제 애플리케이션을 삭제하기 위해서는 deployment 에서 삭제를 한다.
~~~
$ kubectl delete deployment hello
deployment.extensions "hello" deleted
~~~

삭제되었는지 확인한다.
~~~
$ kubectl get deployments
No resources found.
~~~

다시 pod 을 확인한다.
~~~
$ kubectl get po
NAME                     READY   STATUS        RESTARTS   AGE
hello-56cd858d87-rmb7s   0/1     Terminating   11         37m
~~~

아직 존재한다. 하지만 몇초후에 확인을 해 보면 삭제되었음을 알 수 있다.
~~~
$ kubectl get pod
No resources found.
~~~


# kubectl delete deployment --all

애플리케이션 전체를 삭제하고자 할 때 사용한다.  
먼저 3개의 애플리케이션을 실행한다.
~~~
$ kubectl run hello1 --image=hello-world
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello1 created

$ kubectl run hello2 --image=hello-world
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello2 created

$ kubectl run hello3 --image=hello-world
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello3 created
~~~

현재 수행중인 pod을 확인한다.
~~~
$ kubectl get pods
NAME                      READY   STATUS             RESTARTS   AGE
hello1-f65699859-9k8hs    0/1     CrashLoopBackOff   1          32s
hello2-6699d87f4f-65h6c   0/1     CrashLoopBackOff   1          28s
hello3-67c967b5f5-829rt   0/1     CrashLoopBackOff   1          23s
~~~

현재 배포된 상태를 확인한다.
~~~
$ kubectl get deployments
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
hello1   0/1     1            0           18s
hello2   0/1     1            0           14s
hello3   0/1     1            0           10s
~~~

전체 애플리케이션을 삭제한다.
~~~
$ kubectl delete deployment --all
deployment.extensions "hello1" deleted
deployment.extensions "hello2" deleted
deployment.extensions "hello3" deleted
~~~

삭제가 완료되었는지 확인한다.
~~~
$ kubectl get deployments
No resources found.
$ kubectl get pods
No resources found.
~~~

# kubectl get po --show-labels
# kubectl get po -l app,mode
# kubectl get po -l app=account
# kubectl get po -l '!mode'

# kubectl label po hello-world app=ui
# kubectl label po hello-world app=ui --overwrite
# kubectl label node oke-hellow-123-481274-hokf gpu=true
# kubectl get nodes -l gpu=true

# kubectl create -f hello-world-gpu.yaml
# kubectl delete po -l mode=dev
# kubectl get ns
# kubectl get po --namespace myns
# kubectl get po -n myns

# kubectl create namespace myns
# kubectl delete ns myns
# kubectl create -f hello-world.yaml -n test-namespace
# kubectl get po -n test
# kubectl delete all --all
# kubectl delete all --all -n myns

# kubectl create -f myrc.yaml
# kubectl get rc
# kubectl edit rc myrc
# kubectl scale rc myrc --replicas=10
# kubectl delete rc myrc
# kubectl delete rc myrc --cascade=false


# kubectl get rs

# kubectl create -f myservice.yaml
# kubectl get svc
# kubectl get endpoints myservice
# kubectl delete svc myservice

