*查看ip地址*
```bash
 ip addr
```
安装docker
$ sudo  yum -y update
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
$ sudo yum-config-manager    --add-repo     http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
//解决containerd.io版本问题
yum install -y https://mirrors.aliyun.com/docker-ce/linux/centos/7/x86_64/edge/Packages/containerd.io-1.2.13-3.1.el7.x86_64.rpm

$ sudo yum install docker-ce，$ sudo yum install docker-ce-18.09.9 docker-ce-cli-18.09.9 containerd.io
$ sudo systemctl start docker 
$ sudo docker run hello-world
$ docker version