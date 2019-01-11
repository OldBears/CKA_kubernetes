一、安装docker基础

1、配置yum

wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum list docker-ce.x86_64  --showduplicates |sort -r
yum install -y --setopt=obsoletes=0 docker-ce-18.06.1.ce-3.el7

2、启动docker

systemctl start docker
systemctl enable docker

3、配置镜像加速

vim /etc/docker/daemon.json 
{
   "registry-mirrors":["https://registry.docker-cn.com"]
}
systemctl restart docker

二、安装docker-compose

1、docker-compose链接

  https://github.com/docker/compose/releases
  
2、安装docker-compose

  #curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  #chmod +x /usr/local/bin/docker-compose

三、VMware Harbor

1、相关链接

  https://github.com/goharbor/harbor/releases
  
2、离线下载软件包

  https://storage.googleapis.com/harbor-releases/release-1.7.0/harbor-offline-installer-v1.7.0.tgz
  
3、安装harbor

  tar xvf harbor-offline-installer-v1.7.0.tgz
  
4、修改相关配置文件

  #vim harbor.cfg
  hostname = 192.168.0.103
  
5、运行harbor
  
  ./install
  
  [Step 0]: checking installation environment ...

  Note: docker version: 18.06.1
  Note: docker-compose version: 1.23.2
  [Step 1]: loading Harbor images ...
  93e951af0370: Loading layer  32.92MB/32.92MB
  b07c3b7617d1: Loading layer  8.955MB/8.955MB
  8a648a517967: Loading layer   15.6MB/15.6MB
  779c814058b2: Loading layer  17.41kB/17.41kB
  9e20a0be79e4: Loading layer   15.6MB/15.6MB
  Loaded image: goharbor/harbor-adminserver:v1.7.0
  4297ec306d6c: Loading layer  8.955MB/8.955MB
  a12cde073f45: Loading layer   22.8MB/22.8MB
  0c1bffe1be38: Loading layer  3.072kB/3.072kB
  fed6dbfa94fa: Loading layer  7.465MB/7.465MB
  d6eb3348ffbe: Loading layer  30.26MB/30.26MB
  Loaded image: goharbor/harbor-registryctl:v1.7.0
  c13145c62cf8: Loading layer  64.84MB/64.84MB
  4c99c8ba2cde: Loading layer  3.072kB/3.072kB
  44a1362c9b8e: Loading layer   59.9kB/59.9kB
  8ce0285d24fc: Loading layer  61.95kB/61.95kB
  Loaded image: goharbor/redis-photon:v1.7.0
  d4fcd72d2acd: Loading layer   8.96MB/8.96MB
  b08aeb2eb53f: Loading layer  35.08MB/35.08MB
  ae47b31c410d: Loading layer  2.048kB/2.048kB
  e441c5110655: Loading layer  3.072kB/3.072kB
  2fd30180e5e5: Loading layer  35.08MB/35.08MB
  Loaded image: goharbor/chartmuseum-photon:v0.7.1-v1.7.0
  136482b2f6f3: Loading layer   3.39MB/3.39MB
  039eae17292e: Loading layer  4.716MB/4.716MB
  dc5eee7908d6: Loading layer  3.584kB/3.584kB
  Loaded image: goharbor/harbor-portal:v1.7.0
  0e74496c6950: Loading layer  8.955MB/8.955MB
  15c9a151202f: Loading layer  21.51MB/21.51MB
  953c969e39ad: Loading layer  21.51MB/21.51MB
  Loaded image: goharbor/harbor-jobservice:v1.7.0
  eb5a3a0dbc61: Loading layer   3.39MB/3.39MB
  Loaded image: goharbor/nginx-photon:v1.7.0
  bc23790eef84: Loading layer  63.31MB/63.31MB
  5ca06da9ad20: Loading layer  40.57MB/40.57MB
  4f32f5a7c4c9: Loading layer  6.656kB/6.656kB
  37ae4614bffc: Loading layer  2.048kB/2.048kB
  36f46ea7b71d: Loading layer   7.68kB/7.68kB
  99bf27d6d0cc: Loading layer   2.56kB/2.56kB
  13e21e5fbfc6: Loading layer   2.56kB/2.56kB
  35d4106fa26d: Loading layer   2.56kB/2.56kB
  Loaded image: goharbor/harbor-db:v1.7.0
  3ebe32b61098: Loading layer  8.954MB/8.954MB
  8667645d95c3: Loading layer  13.43MB/13.43MB
  cade3b441a2a: Loading layer   17.3MB/17.3MB
  9fb1d77ff057: Loading layer  11.26kB/11.26kB
  25a21beb926b: Loading layer  3.072kB/3.072kB
  ba2ec726786e: Loading layer  30.72MB/30.72MB
  Loaded image: goharbor/notary-server-photon:v0.6.1-v1.7.0
  5090c10b14d0: Loading layer  12.11MB/12.11MB
  4b1d5b0385f5: Loading layer   17.3MB/17.3MB
  f9f418bec695: Loading layer  11.26kB/11.26kB
  da1e63f4a31b: Loading layer  3.072kB/3.072kB
  3f8b984303e8: Loading layer  29.41MB/29.41MB
  Loaded image: goharbor/notary-signer-photon:v0.6.1-v1.7.0
  31675abcf456: Loading layer    113MB/113MB
  5094a1326364: Loading layer  11.46MB/11.46MB
  b324c0e7a2d3: Loading layer  2.048kB/2.048kB
  a16d9ae02855: Loading layer  48.13kB/48.13kB
  a8362a2377a9: Loading layer  3.072kB/3.072kB
  8dd4465b785e: Loading layer  11.51MB/11.51MB
  Loaded image: goharbor/clair-photon:v2.0.7-v1.7.0
  4de51055f30c: Loading layer  133.2MB/133.2MB
  a3dbf62e8962: Loading layer    684MB/684MB
  042663fd852c: Loading layer   7.68kB/7.68kB
  a8704e62e5a5: Loading layer    212kB/212kB
  Loaded image: goharbor/harbor-migrator:v1.7.0
  d071182f0989: Loading layer  8.955MB/8.955MB
  f3612283b0e7: Loading layer  27.24MB/27.24MB
  36eb37ad53c8: Loading layer  5.632kB/5.632kB
  ab115d85bed7: Loading layer  27.24MB/27.24MB
  Loaded image: goharbor/harbor-core:v1.7.0
  4b0f921bbad9: Loading layer  50.39MB/50.39MB
  b7135566a59e: Loading layer  3.584kB/3.584kB
  c396d105f9d0: Loading layer  3.072kB/3.072kB
  e13ef0c2ab14: Loading layer  4.096kB/4.096kB
  fd02ffa15f0c: Loading layer  3.584kB/3.584kB
  c7a5e5579a32: Loading layer  10.24kB/10.24kB
  Loaded image: goharbor/harbor-log:v1.7.0
  1a6785975e13: Loading layer  8.955MB/8.955MB
  7211858db3fc: Loading layer  3.072kB/3.072kB
  bd91fc754ebc: Loading layer   2.56kB/2.56kB
  cdafbc058a04: Loading layer   2.56kB/2.56kB
  3fa42220b2aa: Loading layer  2.048kB/2.048kB
  044cc5016d5f: Loading layer   22.8MB/22.8MB
  1fc77ba8533d: Loading layer   22.8MB/22.8MB
  Loaded image: goharbor/registry-photon:v2.6.2-v1.7.0


  [Step 2]: preparing environment ...
  Generated and saved secret to file: /data/secretkey
  Generated configuration file: ./common/config/nginx/nginx.conf
  Generated configuration file: ./common/config/adminserver/env
  Generated configuration file: ./common/config/core/env
  Generated configuration file: ./common/config/registry/config.yml
  Generated configuration file: ./common/config/db/env
  Generated configuration file: ./common/config/jobservice/env
  Generated configuration file: ./common/config/jobservice/config.yml
  Generated configuration file: ./common/config/log/logrotate.conf
  Generated configuration file: ./common/config/registryctl/env
  Generated configuration file: ./common/config/core/app.conf
  Generated certificate, key file: ./common/config/core/private_key.pem, cert file: ./common/config/registry/root.crt
  The configuration files are ready, please use docker-compose to start the service.

  [Step 3]: checking existing instance of Harbor ...

  [Step 4]: starting Harbor ...
  Creating network "harbor_harbor" with the default driver
  Creating harbor-log ... done
  Creating harbor-adminserver ... done
  Creating registry           ... done
  Creating harbor-db          ... done
  Creating redis              ... done
  Creating registryctl        ... done
  Creating harbor-core        ... done
  Creating harbor-portal      ... done
  Creating harbor-jobservice  ... done
  Creating nginx              ... done

  ✔ ----Harbor has been installed and started successfully.----

  Now you should be able to visit the admin portal at http://192.168.0.103. 
  For more details, please visit https://github.com/goharbor/harbor .

6、访问
   http://192.168.0.103
   

二、上传镜像至仓库

1、在harbor中创建public仓库、gfxu用户名

2、下载相关容器

   #docker pull busybox:latest
   
3、登陆registry服务器
   docker login 192.168.0.103:80
   username:gfxu
   passwd 

4、重新tag容器包(注意加上80，默认是443端口)

   docker tag SOURCE_IMAGE[:TAG] 192.168.0.103:80/public/IMAGE[:TAG]
   docker push 192.168.0.103:80/public/IMAGE[:TAG]
   
5、参考文档

   https://github.com/goharbor/harbor/blob/master/docs/installation_guide.md
 
6、docker-compose日常操作

   docker-compose stop/start
   
三、常见问题

1、push镜像到本地仓库时报错

    Error: Invalid registry endpoint https://10.10.3.67:5000/v2/: Get https://10.10.3.67:5000/v2/_ping: dial tcp 10.10.3.67:5000: connection refused. If this private registry supports only HTTP or HTTPS with an unknown CA certificate, please add `--insecure-registry 10.10.3.67:5000` to the daemon's arguments. In the case of HTTPS, if you have access to the registry's CA certificate, no need for the flag; simply place the CA certificate at /etc/docker/certs.d/10.10.3.67:5000/ca.crt 
   或者
   http: server gave HTTP response to HTTPS client
   因为Docker从1.3.X之后，与docker registry交互默认使用的是https，然而此处搭建的私有仓库只提供http服务，所以当与私有仓库交互时就会报上面的错误。为了解决这个问题需要在启动docker server时增加启动参数为默认使用http访问。centos7下修改docker启动配置文件

   [root@docker shell]# cat /usr/lib/systemd/system/docker.service 
   [Unit]
   Description=Docker Application Container Engine
   Documentation=https://docs.docker.com
   After=network-online.target firewalld.service
   Wants=network-online.target

   [Service]
   Type=notify
   ExecStart=/usr/bin/dockerd --insecure-registry 192.168.0.103：80
   再修改
   [root@docker shell]# cat /etc/docker/daemon.json 

   {
     "registry-mirrors": [
       "https://registry.docker-cn.com"
     ]
   }
   {
     "insecure-registry":["192.169.0.103：80"]
   }
   然后重启服务
   systemctl daemon-reload
   systemctl restart docker
   
2、docker ps 报错
  
  ERROR: 
        Can't find a suitable configuration file in this directory or any
        parent. Are you in the right directory?
        Supported filenames: docker-compose.yml, docker-compose.yaml
  切换到docker-compose.yaml所在目录下 启动成功
  
  #cd /root/harbor
  
3、ERROE:Cannot locate specified Dockerfile: Dockerfile
   需要在docker-compose.yml里面指定Dockerfile路径
   [root@docker data]# tree compose-test/
   compose-test/
   ├── docker
   │ ├── docker-compose.yml
   │ ├── requirement.txt
   │ └── web
   │ └── Dockerfile
   └── src
       └── app.py
   在docker-compose.yml文件添加如下字段
   app:
       build:
         context: web
         dockerfile: Dockerfile
         
4、Docker默认的配置文件/etc/default/docker或者/etc/sysconfig/docker都不起作用，查看了一下/lib/systemd/system/docker.service文件，发现里面没有加载默认配置文件，一些配置不知道要怎么弄了~~~

   解决办法是：

   $ vi /lib/systemd/system/docker.service
   #添加一行
   $ EnvironmentFile=-/etc/default/docker
   或者
   $ EnvironmentFile=-/etc/sysconfig/docker
   #-代表ignore error

   #并修改
   $ ExecStart=/usr/bin/docker daemon -H fd://
   #改成
   $ ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS

   这样才能使用/etc/default/docker里定义的DOCKER_OPTS参数

   $ systemctl daemon-reload 重载
   $ sudo service docker restart
  
5、上传dockerfile失败

   docker push 192.168.0.103:80/public/centos:2019011
   
   
