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
  
  2、获取kubernetes源（仅master）
  
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

  1、修改/etc/sysconfig/kublet
  
      KUBELET_EXTRA_ARGS=--fail-swap-on=false
      
  2、安装kubectl kubeadm kubelet（仅master安装）
  
      #rpm --import https://mirrors.aliyun.com/kubernetes/apt/doc/rpm-package-key.gpg
      #yum install -y kubelet-1.12.1 kubeadm-1.12.1 kubectl-1.12.1
      #默认会安装最新版
      
  3、设置kubelet开机自启动
  
      #systemctl enable kubelet
      
  4、初始化集群
  
      #kubeadm init --kubernetes-version=v1.12.1 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/16
      报错如下：
      init] Using Kubernetes version: v1.12.1
      [preflight] Running pre-flight checks
      error execution phase preflight: [preflight] Some fatal errors occurred:
	    [ERROR Swap]: running with swap on is not supported. Please disable swap
      [preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
      
      再次执行：
      #kubeadm init --kubernetes-version=v1.12.1 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/16 --ignore-preflight-errors=Swap
      
      报错如下：
      
      [preflight] Some fatal errors occurred:
    	[ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-apiserver:v1.12.1: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	    [ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-controller-manager:v1.12.1: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	    [ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-scheduler:v1.12.1: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	    [ERROR ImagePull]: failed to pull image k8s.gcr.io/kube-proxy:v1.12.1: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	    [ERROR ImagePull]: failed to pull image k8s.gcr.io/pause:3.1: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	    [ERROR ImagePull]: failed to pull image k8s.gcr.io/etcd:3.2.24: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
	    [ERROR ImagePull]: failed to pull image k8s.gcr.io/coredns:1.2.2: output: Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers), error: exit status 1
      [preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
 

    此报错为为下载image报错，我们可以自行下载使用，见下脚本：
    
    docker pull mirrorgooglecontainers/kube-apiserver:v1.12.1
    docker pull mirrorgooglecontainers/kube-controller-manager:v1.12.1
    docker pull mirrorgooglecontainers/kube-scheduler:v1.12.1
    docker pull mirrorgooglecontainers/kube-proxy:v1.12.1
    docker pull mirrorgooglecontainers/pause:3.1
    docker pull mirrorgooglecontainers/etcd:3.2.24
    docker pull coredns/coredns:1.2.2

    docker tag mirrorgooglecontainers/kube-proxy:v1.12.1  k8s.gcr.io/kube-proxy:v1.12.1
    docker tag mirrorgooglecontainers/kube-scheduler:v1.12.1 k8s.gcr.io/kube-scheduler:v1.12.1
    docker tag mirrorgooglecontainers/kube-apiserver:v1.12.1 k8s.gcr.io/kube-apiserver:v1.12.1
    docker tag mirrorgooglecontainers/kube-controller-manager:v1.12.1 k8s.gcr.io/kube-controller-manager:v1.12.1
    docker tag mirrorgooglecontainers/etcd:3.2.24  k8s.gcr.io/etcd:3.2.24
    docker tag coredns/coredns:1.2.2 k8s.gcr.io/coredns:1.2.2
    docker tag mirrorgooglecontainers/pause:3.1  k8s.gcr.io/pause:3.1


    docker rmi mirrorgooglecontainers/kube-apiserver:v1.12.1
    docker rmi mirrorgooglecontainers/kube-controller-manager:v1.12.1
    docker rmi mirrorgooglecontainers/kube-scheduler:v1.12.1
    docker rmi mirrorgooglecontainers/kube-proxy:v1.12.1
    docker rmi mirrorgooglecontainers/pause:3.1
    docker rmi mirrorgooglecontainers/etcd:3.2.24
    docker rmi coredns/coredns:1.2.2

  
  
  

      

