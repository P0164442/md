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

$ sudo yum install docker-ce docker-ce-cli containerd.io
//上面代码经常装不上
 yum install docker-ce-20.10.0-3.el8 docker-ce-cli-20.10.0-3.el8 containerd.io
$ sudo systemctl start docker 
$ sudo docker run hello-world
$ docker version


一键安装
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun