一、系统配置

  1、配置主机名
  
    #hostnamectl set-hostname master
    #hostnamectl set-hostname node1
    #hostnamectl set-hostname node2
    
  2、配置hosts解析
    
   （1）编辑配置文件
    
    vi /etc/hosts
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    192.168.0.200	master
    192.168.0.201	node1
    192.168.0.202	node2
    
   （2）同步三台节点的hosts文件
    
    #scp /etc/hosts 192.168.0.200:/etc/
    #scp /etc/hosts 192.168.0.200:/etc/

  3、防火墙配置
  
    #systemctl stop firewalld
    #systemtl disable firewalld
    
  4、禁用SELINUX(需要重启)
  
    #vi /etc/selinux/config
    SELINUX=disabled
    
  5、/etc/sysctl.d/k8s.conf 内核参数配置
  
    #vi /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    net.ipv4.ip_forward = 1
    
   （1）执行命令使修改生效。

    #modprobe br_netfilter
    #sysctl -p /etc/sysctl.d/k8s.conf

  6、修改系统时间，同步时间
  
   （1）修改时区
    
      rm -rf /etc/localtime
      ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
      vim /etc/sysconfig/clock
      ZONE="Asia/Shanghai"
      UTC=false
      ARC=false
      
  （2）安装并设置开机自启
    
    yum install -y ntp
    systemctl start ntpd
    systemctl enable ntpd
    
  （3）配置开机启动校验

    vim /etc/rc.d/rc.local
    /usr/sbin/ntpdate ntp1.aliyun.com > /dev/null 2>&1; /sbin/hwclock -w

  （4）配置定时任务

    crontab -e
    0 */1 * * * ntpdate ntp1.aliyun.com > /dev/null 2>&1; /sbin/hwclock -w
 
 二、安装docker及kubernetes
 
  1、获取docker源（所有节点）
  
      #wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  
  2、获取kubernetes源
  
      #cat /etc/yum.repos.d/kubernetes.repo 
      [kubernetes]
      name=Kubernetes Repo
      baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
      gpgcheck=1
      gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
      enabled=1
  
  3、安装docker（所有节点都要安装）
  
      #yum list docker-ce.x86_64  --showduplicates |sort -r查询相关版本
      #yum install -y --setopt=obsoletes=0 docker-ce-18.06.1.ce-3.el7
   
  4、设置docker开机启动
  
      #systemctl start docker
      #systemctl enable docker
  
三、master主节点修改配置

  0、关于kubeadm
 
      https://github.com/kubernetes/kubeadm/blob/master/docs/design/design_v1.10.md
      
  1、安装kubectl kubeadm kubelet（所有节点都要安装）
  
      #rpm --import https://mirrors.aliyun.com/kubernetes/apt/doc/rpm-package-key.gpg
      #yum install -y kubelet-1.12.3 kubeadm-1.12.3 kubectl-1.12.3
      #默认会安装最新版
      
  2、修改/etc/sysconfig/kublet
  
      KUBELET_EXTRA_ARGS=--fail-swap-on=false
      
  3、设置kubelet开机自启动
  
      #systemctl enable kubelet
  
  4、启动systemctl start kubelet(报错信息)
  
  	Dec  6 10:16:52 master systemd: kubelet.service holdoff time over, scheduling restart.
	Dec  6 10:16:52 master systemd: Started kubelet: The Kubernetes Node Agent.
	Dec  6 10:16:52 master systemd: Starting kubelet: The Kubernetes Node Agent...
	Dec  6 10:16:52 master kubelet: Flag --fail-swap-on has been deprecated, This parameter should be set via the config file specified by the Kubelet's --config flag. See https://kubernetes.io/docs/tasks/administer-cluster/kubelet-config-file/ for more information.
	Dec  6 10:16:52 master kubelet: F1206 10:16:52.172532    7663 server.go:190] failed to load Kubelet config file /var/lib/kubelet/config.yaml, error failed to read kubelet config file "/var/lib/kubelet/config.yaml", error: open /var/lib/kubelet/config.yaml: no such file or directory
	Dec  6 10:16:52 master systemd: kubelet.service: main process exited, code=exited, status=255/n/a
	Dec  6 10:16:52 master systemd: Unit kubelet.service entered failed state.
	Dec  6 10:16:52 master systemd: kubelet.service failed.

	#由于此时还没有/var/lib/kubelet/config.yaml文件，可以先不用启动kubelet
      
  5、初始化集群
  
      #kubeadm init --kubernetes-version=v1.12.3 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/16
      报错如下：
      [init] using Kubernetes version: v1.12.3
	[preflight] running pre-flight checks
	[preflight] Some fatal errors occurred:
		[ERROR Swap]: running with swap on is not supported. Please disable swap
	[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`

      
      再次执行：
      #kubeadm init --kubernetes-version=v1.12.3 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/16 --ignore-preflight-errors=Swap
      
      报错如下：
      
	[preflight] Some fatal errors occurred:
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-apiserver:v1.12.3: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-controller-manager:v1.12.3: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-scheduler:v1.12.3: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-proxy:v1.12.3: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/pause:3.1: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/etcd:3.2.24: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[ERROR ImagePull]: failed to pull image k8s.gcr.io/coredns:1.2.2: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`

 

    此报错为为下载image报错，我们可以自行下载使用，见下脚本：
    
	    docker pull mirrorgooglecontainers/kube-apiserver:v1.12.3
	    docker pull mirrorgooglecontainers/kube-controller-manager:v1.12.3
	    docker pull mirrorgooglecontainers/kube-scheduler:v1.12.3
	    docker pull mirrorgooglecontainers/kube-proxy:v1.12.3
	    docker pull mirrorgooglecontainers/pause:3.1
	    docker pull mirrorgooglecontainers/etcd:3.2.24
	    docker pull coredns/coredns:1.2.2

	    docker tag mirrorgooglecontainers/kube-proxy:v1.12.3  k8s.gcr.io/kube-proxy:v1.12.3
	    docker tag mirrorgooglecontainers/kube-scheduler:v1.12.3 k8s.gcr.io/kube-scheduler:v1.12.3
	    docker tag mirrorgooglecontainers/kube-apiserver:v1.12.3 k8s.gcr.io/kube-apiserver:v1.12.3
	    docker tag mirrorgooglecontainers/kube-controller-manager:v1.12.3 k8s.gcr.io/kube-controller-manager:v1.12.3
	    docker tag mirrorgooglecontainers/etcd:3.2.24  k8s.gcr.io/etcd:3.2.24
	    docker tag coredns/coredns:1.2.2 k8s.gcr.io/coredns:1.2.2
	    docker tag mirrorgooglecontainers/pause:3.1  k8s.gcr.io/pause:3.1


	    docker rmi mirrorgooglecontainers/kube-apiserver:v1.12.3
	    docker rmi mirrorgooglecontainers/kube-controller-manager:v1.12.3
	    docker rmi mirrorgooglecontainers/kube-scheduler:v1.12.3
	    docker rmi mirrorgooglecontainers/kube-proxy:v1.12.3
	    docker rmi mirrorgooglecontainers/pause:3.1
	    docker rmi mirrorgooglecontainers/etcd:3.2.24
	    docker rmi coredns/coredns:1.2.2

 5、重新初始化集群

	#kubeadm init --kubernetes-version=v1.12.3 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/16 --ignore-preflight-errors=Swap
	[init] using Kubernetes version: v1.12.3
	[preflight] running pre-flight checks
	[WARNING Swap]: running with swap on is not supported. Please disable swap
	[preflight/images] Pulling images required for setting up a Kubernetes cluster
	[preflight/images] This might take a minute or two, depending on the speed of your internet connection
	[preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
	[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
	[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
	[preflight] Activating the kubelet service
	[certificates] Generated ca certificate and key.
	[certificates] Generated apiserver-kubelet-client certificate and key.
	[certificates] Generated apiserver certificate and key.
	[certificates] apiserver serving cert is signed for DNS names [master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.60.120]
	[certificates] Generated front-proxy-ca certificate and key.
	[certificates] Generated front-proxy-client certificate and key.
	[certificates] Generated etcd/ca certificate and key.
	[certificates] Generated etcd/healthcheck-client certificate and key.
	[certificates] Generated apiserver-etcd-client certificate and key.
	[certificates] Generated etcd/server certificate and key.
	[certificates] etcd/server serving cert is signed for DNS names [master localhost] and IPs [127.0.0.1 ::1]
	[certificates] Generated etcd/peer certificate and key.
	[certificates] etcd/peer serving cert is signed for DNS names [master localhost] and IPs [192.168.60.120 127.0.0.1 ::1]
	[certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
	[certificates] Generated sa key and public key.
	[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
	[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
	[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
	[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
	[controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
	[controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
	[controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
	[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
	[init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests" 
	[init] this might take a minute or longer if the control plane images have to be pulled
	[apiclient] All control plane components are healthy after 62.502538 seconds
	[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
	[kubelet] Creating a ConfigMap "kubelet-config-1.12" in namespace kube-system with the configuration for the kubelets in the cluster
	[markmaster] Marking the node master as master by adding the label "node-role.kubernetes.io/master=''"
	[markmaster] Marking the node master as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
	[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "master" as an annotation
	[bootstraptoken] using token: wbqp81.aol6fvxawiim59ct
	[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
	[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
	[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
	[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
	[addons] Applied essential addon: CoreDNS
	[addons] Applied essential addon: kube-proxy

	Your Kubernetes master has initialized successfully!

	To start using your cluster, you need to run the following as a regular user:

	  mkdir -p $HOME/.kube
	  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	  sudo chown $(id -u):$(id -g) $HOME/.kube/config

	You should now deploy a pod network to the cluster.
	Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
	  https://kubernetes.io/docs/concepts/cluster-administration/addons/

	You can now join any number of machines by running the following on each node
	as root:

	  kubeadm join 192.168.60.120:6443 --token wbqp81.aol6fvxawiim59ct --discovery-token-ca-cert-hash sha256:e054410cff470505146ab7447a887e4308e6e903ccae83f0133c2a1cfe5d3069

  
  

      

