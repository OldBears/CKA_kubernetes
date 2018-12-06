一、关于kubectl命令的使用

 1、kubectl --help
    
      kubectl controls the Kubernetes cluster manager. 

      Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

      Basic Commands (Beginner):
        create         Create a resource from a file or from stdin.
        expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
        run            Run a particular image on the cluster
        set            Set specific features on objects

      Basic Commands (Intermediate):
        explain        Documentation of resources
        get            Display one or many resources
        edit           Edit a resource on the server
        delete         Delete resources by filenames, stdin, resources and names, or by resources and label selector

      Deploy Commands:
        rollout        Manage the rollout of a resource
        scale          Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job
        autoscale      Auto-scale a Deployment, ReplicaSet, or ReplicationController

      Cluster Management Commands:
        certificate    Modify certificate resources.
        cluster-info   Display cluster info
        top            Display Resource (CPU/Memory/Storage) usage.
        cordon         Mark node as unschedulable
        uncordon       Mark node as schedulable
        drain          Drain node in preparation for maintenance
        taint          Update the taints on one or more nodes

      Troubleshooting and Debugging Commands:
        describe       Show details of a specific resource or group of resources
        logs           Print the logs for a container in a pod
        attach         Attach to a running container
        exec           Execute a command in a container
        port-forward   Forward one or more local ports to a pod
        proxy          Run a proxy to the Kubernetes API server
        cp             Copy files and directories to and from containers.
        auth           Inspect authorization

      Advanced Commands:
        apply          Apply a configuration to a resource by filename or stdin
        patch          Update field(s) of a resource using strategic merge patch
        replace        Replace a resource by filename or stdin
        wait           Experimental: Wait for a specific condition on one or many resources.
        convert        Convert config files between different API versions

      Settings Commands:
        label          Update the labels on a resource
        annotate       Update the annotations on a resource
        completion     Output shell completion code for the specified shell (bash or zsh)

      Other Commands:
        alpha          Commands for features in alpha
        api-resources  Print the supported API resources on the server
        api-versions   Print the supported API versions on the server, in the form of "group/version"
        config         Modify kubeconfig files
        plugin         Provides utilities for interacting with plugins.
        version        Print the client and server version information

      Usage:
        kubectl [flags] [options]

      Use "kubectl <command> --help" for more information about a given command.
      Use "kubectl options" for a list of global command-line options (applies to all commands).
      
2、kubectl describe node master

    Name:               master
    Roles:              master
    Labels:             beta.kubernetes.io/arch=amd64
                        beta.kubernetes.io/os=linux
                        kubernetes.io/hostname=master
                        node-role.kubernetes.io/master=
    Annotations:        flannel.alpha.coreos.com/backend-data: {"VtepMAC":"2e:ea:51:3b:60:82"}
                        flannel.alpha.coreos.com/backend-type: vxlan
                        flannel.alpha.coreos.com/kube-subnet-manager: true
                        flannel.alpha.coreos.com/public-ip: 192.168.60.120
                        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                        node.alpha.kubernetes.io/ttl: 0
                        volumes.kubernetes.io/controller-managed-attach-detach: true
    CreationTimestamp:  Thu, 06 Dec 2018 10:28:31 +0800
    Taints:             node-role.kubernetes.io/master:NoSchedule
    Unschedulable:      false
    Conditions:
      Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
      ----             ------  -----------------                 ------------------                ------                       -------
      OutOfDisk        False   Thu, 06 Dec 2018 18:46:52 +0800   Thu, 06 Dec 2018 10:28:15 +0800   KubeletHasSufficientDisk     kubelet has sufficient disk space available
      MemoryPressure   False   Thu, 06 Dec 2018 18:46:52 +0800   Thu, 06 Dec 2018 10:28:15 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
      DiskPressure     False   Thu, 06 Dec 2018 18:46:52 +0800   Thu, 06 Dec 2018 10:28:15 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
      PIDPressure      False   Thu, 06 Dec 2018 18:46:52 +0800   Thu, 06 Dec 2018 10:28:15 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
      Ready            True    Thu, 06 Dec 2018 18:46:52 +0800   Thu, 06 Dec 2018 11:31:12 +0800   KubeletReady                 kubelet is posting ready status
    Addresses:
      InternalIP:  192.168.60.120
      Hostname:    master
    Capacity:
     cpu:                1
     ephemeral-storage:  38770180Ki
     hugepages-2Mi:      0
     memory:             1016480Ki
     pods:               110
    Allocatable:
     cpu:                1
     ephemeral-storage:  35730597829
     hugepages-2Mi:      0
     memory:             914080Ki
     pods:               110
    System Info:
     Machine ID:                 a689a21ab34c4adc8325e026a29bfd2f
     System UUID:                A689A21A-B34C-4ADC-8325-E026A29BFD2F
     Boot ID:                    f954c063-f5a9-4183-b997-af5bec1b48da
     Kernel Version:             3.10.0-514.el7.x86_64
     OS Image:                   CentOS Linux 7 (Core)
     Operating System:           linux
     Architecture:               amd64
     Container Runtime Version:  docker://18.6.1
     Kubelet Version:            v1.12.3
     Kube-Proxy Version:         v1.12.3
    PodCIDR:                     10.244.0.0/24
    Non-terminated Pods:         (8 in total)
      Namespace                  Name                              CPU Requests  CPU Limits  Memory Requests  Memory Limits
      ---------                  ----                              ------------  ----------  ---------------  -------------
      kube-system                coredns-576cbf47c7-58j5g          100m (10%)    0 (0%)      70Mi (7%)        170Mi (19%)
      kube-system                coredns-576cbf47c7-tcxd4          100m (10%)    0 (0%)      70Mi (7%)        170Mi (19%)
      kube-system                etcd-master                       0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                kube-apiserver-master             250m (25%)    0 (0%)      0 (0%)           0 (0%)
      kube-system                kube-controller-manager-master    200m (20%)    0 (0%)      0 (0%)           0 (0%)
      kube-system                kube-flannel-ds-amd64-79z4g       100m (10%)    100m (10%)  50Mi (5%)        50Mi (5%)
      kube-system                kube-proxy-ccpps                  0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                kube-scheduler-master             100m (10%)    0 (0%)      0 (0%)           0 (0%)
    Allocated resources:
      (Total limits may be over 100 percent, i.e., overcommitted.)
      Resource  Requests     Limits
      --------  --------     ------
      cpu       850m (85%)   100m (10%)
      memory    190Mi (21%)  390Mi (43%)
    Events:
      Type    Reason                   Age                From                Message
      ----    ------                   ----               ----                -------
      Normal  Starting                 27m                kubelet, master     Starting kubelet.
      Normal  NodeAllocatableEnforced  27m                kubelet, master     Updated Node Allocatable limit across pods
      Normal  NodeHasSufficientDisk    27m (x6 over 27m)  kubelet, master     Node master status is now: NodeHasSufficientDisk
      Normal  NodeHasSufficientMemory  27m (x6 over 27m)  kubelet, master     Node master status is now: NodeHasSufficientMemory
      Normal  NodeHasNoDiskPressure    27m (x5 over 27m)  kubelet, master     Node master status is now: NodeHasNoDiskPressure
      Normal  NodeHasSufficientPID     27m (x6 over 27m)  kubelet, master     Node master status is now: NodeHasSufficientPID
      Normal  Starting                 25m                kube-proxy, master  Starting kube-proxy.

2、kubectl run --help

      # kubectl run --help
      Create and run a particular image, possibly replicated.
      Creates a deployment or job to manage the created container(s).
      Examples:
        # Start a single instance of nginx.
        kubectl run nginx --image=nginx

        # Start a single instance of hazelcast and let the container expose port 5701 .
        kubectl run hazelcast --image=hazelcast --port=5701

        # Start a single instance of hazelcast and set environment variables "DNS_DOMAIN=cluster" and "POD_NAMESPACE=default"
      in the container.
        kubectl run hazelcast --image=hazelcast --env="DNS_DOMAIN=cluster" --env="POD_NAMESPACE=default"

        # Start a single instance of hazelcast and set labels "app=hazelcast" and "env=prod" in the container.
        kubectl run hazelcast --image=hazelcast --labels="app=hazelcast,env=prod"

        # Start a replicated instance of nginx.
        kubectl run nginx --image=nginx --replicas=5

        # Dry run. Print the corresponding API objects without creating them.
        kubectl run nginx --image=nginx --dry-run

        # Start a single instance of nginx, but overload the spec of the deployment with a partial set of values parsed from
      JSON.
        kubectl run nginx --image=nginx --overrides='{ "apiVersion": "v1", "spec": { ... } }'

        # Start a pod of busybox and keep it in the foreground, don't restart it if it exits.
        kubectl run -i -t busybox --image=busybox --restart=Never

        # Start the nginx container using the default command, but use custom arguments (arg1 .. argN) for that command.
        kubectl run nginx --image=nginx -- <arg1> <arg2> ... <argN>

        # Start the nginx container using a different command and custom arguments.
        kubectl run nginx --image=nginx --command -- <cmd> <arg1> ... <argN>

        # Start the perl container to compute π to 2000 places and print it out.
        kubectl run pi --image=perl --restart=OnFailure -- perl -Mbignum=bpi -wle 'print bpi(2000)'

        # Start the cron job to compute π to 2000 places and print it out every 5 minutes.
        kubectl run pi --schedule="0/5 * * * ?" --image=perl --restart=OnFailure -- perl -Mbignum=bpi -wle 'print bpi(2000)'

      Options:
            --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in
      the template. Only applies to golang and jsonpath output formats.
            --attach=false: If true, wait for the Pod to start running, and then attach to the Pod as if 'kubectl attach ...'
      were called.  Default false, unless '-i/--stdin' is set, in which case the default is true. With '--restart=Never' the
      exit code of the container process is returned.
            --cascade=true: If true, cascade the deletion of the resources managed by this resource (e.g. Pods created by a
      ReplicationController).  Default true.
            --command=false: If true and extra arguments are present, use them as the 'command' field in the container, rather
      than the 'args' field which is the default.
            --dry-run=false: If true, only print the object that would be sent, without sending it.
            --env=[]: Environment variables to set in the container
            --expose=false: If true, a public, external service is created for the container(s) which are run
        -f, --filename=[]: to use to replace the resource.
            --force=false: Only used when grace-period=0. If true, immediately remove resources from API and bypass graceful
      deletion. Note that immediate deletion of some resources may result in inconsistency or data loss and requires
      confirmation.
            --generator='': 使用 API generator 的名字, 在
      http://kubernetes.io/docs/user-guide/kubectl-conventions/#generators 查看列表.
            --grace-period=-1: Period of time in seconds given to the resource to terminate gracefully. Ignored if negative.
      Set to 1 for immediate shutdown. Can only be set to 0 when --force is true (force deletion).
            --hostport=-1: The host port mapping for the container port. To demonstrate a single-machine container.
            --image='': 指定容器要运行的镜像.
            --image-pull-policy='': 容器的镜像拉取策略. 如果为空, 这个值将不会 被 client 指定且使用
      server 端的默认值
        -l, --labels='': Comma separated labels to apply to the pod(s). Will override previous values.
            --leave-stdin-open=false: If the pod is started in interactive mode or with stdin, leave stdin open after the
      first attach completes. By default, stdin will be closed after the first attach completes.
            --limits='': The resource requirement limits for this container.  For example, 'cpu=200m,memory=512Mi'.  Note that
      server side components may assign limits depending on the server configuration, such as limit ranges.
        -o, --output='': Output format. One of:
      json|yaml|name|templatefile|template|go-template|go-template-file|jsonpath|jsonpath-file.
            --overrides='': An inline JSON override for the generated object. If this is non-empty, it is used to override the
      generated object. Requires that the object supply a valid apiVersion field.
            --pod-running-timeout=1m0s: The length of time (like 5s, 2m, or 3h, higher than zero) to wait until at least one
      pod is running
            --port='': The port that this container exposes.  If --expose is true, this is also the port used by the service
      that is created.
            --quiet=false: If true, suppress prompt messages.
            --record=false: Record current kubectl command in the resource annotation. If set to false, do not record the
      command. If set to true, record the command. If not set, default to updating the existing annotation value only if one
      already exists.
        -R, --recursive=false: Process the directory used in -f, --filename recursively. Useful when you want to manage
      related manifests organized within the same directory.
        -r, --replicas=1: Number of replicas to create for this container. Default is 1.
            --requests='': 资源为 container 请求 requests . 例如, 'cpu=100m,memory=256Mi'.
      注意服务端组件也许会赋予 requests, 这决定于服务器端配置, 比如 limit ranges.
            --restart='Always': 这个 Pod 的 restart policy.  Legal values [Always, OnFailure, Never]. 如果设置为
      'Always' 一个 deployment 被创建, 如果设置为 ’OnFailure' 一个 job 被创建, 如果设置为 'Never',
      一个普通的 pod 被创建. 对于后面两个 --replicas 必须为 1.  默认 'Always', 为 CronJobs 设置为
      `Never`.
            --rm=false: If true, delete resources created in this command for attached containers.
            --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the
      annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
            --schedule='': A schedule in the Cron format the job should be run with.
            --service-generator='service/v2': 使用 gnerator 的名称创建一个 service.  只有在 --expose 为 true
      的时候使用
            --service-overrides='': An inline JSON override for the generated service object. If this is non-empty, it is used
      to override the generated object. Requires that the object supply a valid apiVersion field.  Only used if --expose is
      true.
            --serviceaccount='': Service account to set in the pod spec
        -i, --stdin=false: Keep stdin open on the container(s) in the pod, even if nothing is attached.
            --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The
      template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
            --timeout=0s: The length of time to wait before giving up on a delete, zero means determine a timeout from the
      size of the object
        -t, --tty=false: Allocated a TTY for each container in the pod.
            --wait=false: If true, wait for resources to be gone before returning. This waits for finalizers.

      Usage:
        kubectl run NAME --image=image [--env="key=value"] [--port=port] [--replicas=replicas] [--dry-run=bool]
      [--overrides=inline-json] [--command] -- [COMMAND] [args...] [options]

      Use "kubectl options" for a list of global command-line options (applies to all commands).
      
3、使用kubectl 创建nginx镜像

 (1)创建
 
      #kubectl run nginx-deploy --image=nginx:latest --port 80 --replicas=1 --dry-run=true
      
      创建名称为:nginx-deploy
      创建镜像源：nginx:latest
      暴露端口： 80
      --replicas=1，创建几个pod
      --dry-run=true，只创建不运行，干跑。
    
 （2）查询pods
    
      # kubectl get pods -o wide
      NAME                            READY   STATUS    RESTARTS   AGE     IP           NODE    NOMINATED NODE
      mynginx-556f94ccdd-kncmf        1/1     Running   0          14m     10.244.2.4   node2   <none>
      mynginx1-5f9577d8c4-27c9t       1/1     Running   0          11m     10.244.1.6   node1   <none>
      mynginx1-5f9577d8c4-6899m       1/1     Running   0          9m56s   10.244.2.6   node2   <none>
      mynginx1-5f9577d8c4-vm8b7       1/1     Running   0          11m     10.244.1.5   node1   <none>
      nginx-deploy-587f5959db-dkjfv   1/1     Running   0          32m     10.244.1.3   node1   <none>

 （3）查询deployment
     
      # kubectl get deployment -o wide
      NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS     IMAGES         SELECTOR
      mynginx        1         1         1            1           13m     mynginx        nginx          run=mynginx
      nginx-deploy   1         1         1            1           7m34s   nginx-deploy   nginx:latest   run=nginx-deploy

 （4）删除一个pods
 
      # kubectl delete pods mynginx-556f94ccdd-sghmf
      pod "mynginx-556f94ccdd-sghmf" deleted
      
      # kubectl get pods -o wide
      NAME                            READY   STATUS    RESTARTS   AGE   IP           NODE    NOMINATED NODE
      mynginx-556f94ccdd-kncmf        1/1     Running   0          12s   10.244.2.4   node2   <none>
      nginx-deploy-587f5959db-dkjfv   1/1     Running   0          18m   10.244.1.3   node1   <none>
  
 （5）expose
 
      #kubectl expose deployment nginx-deploy --name=nginx --port=80 --target-port=80 --protocol=TCP
      [root@master ~]# kubectl get service -o wide
      NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE    SELECTOR
      kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   9h     <none>
      nginx        ClusterIP   10.96.183.124   <none>        80/TCP    105s   run=nginx-deploy
      
      #kubectl expose deployment mynginx1 --name=mynginx1 --port=80 --target-port=80 --protocol=TCP
      # kubectl get svc -o wide
      NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE     SELECTOR
      kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   9h      <none>
      mynginx1     ClusterIP   10.96.134.107   <none>        80/TCP    80s     run=mynginx1
      nginx        ClusterIP   10.96.183.124   <none>        80/TCP    4m38s   run=nginx-deploy

     mynginx1为pods名称
     --name-mynginx1,为新的svc名称
     --port=8081,为svc的IP及端口
     --target-port=80，为pods的实际端口
     --protocol=TCP，为协议，另外还支持UDP、SCTP
     
     # kubectl get svc -o wide
      NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE     SELECTOR
      kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP    9h      <none>
      mynginx      ClusterIP   10.96.112.46    <none>        8081/TCP   3m48s   run=mynginx
      mynginx1     ClusterIP   10.96.134.107   <none>        80/TCP     8m13s   run=mynginx1
      nginx        ClusterIP   10.96.183.124   <none>        80/TCP     11m     run=nginx-deploy

     
     


      
