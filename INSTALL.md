一、系统配置

  1、配置主机名
  
    #hostnamectl set-hostname master
    #hostnamectl set-hostname node1
    #hostnamectl set-hostname node2
    
  2、配置hosts解析
    
   2.1 编辑配置文件
    
    vi /etc/hosts
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    192.168.0.200	master
    192.168.0.201	node1
    192.168.0.202	node2
    
   2.2 同步三台节点的hosts文件
    
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
    
   5.1 执行命令使修改生效。

    #modprobe br_netfilter
    #sysctl -p /etc/sysctl.d/k8s.conf
