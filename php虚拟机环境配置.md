# 本地开发环境

- [本地开发环境](#本地开发环境)
  - [安装（Windows）](#安装windows)
  - [新项目部署实例](#新项目部署实例)
  - [外网访问虚拟机](#外网访问虚拟机)
  - [命令行管理 CentOS](#命令行管理-centos)

这是一台开箱即用的 CentOS 8.x 虚拟机（VMware），内部集成了 `Nginx-1.14.1` / `PHP-7.3` / `MySQL-5.7` 套件，倾向于 `Lareval` 框架开发，但也可以支持其它PHP框架。目的是标准化PHP项目组开发环境、统一管理机制、简化搭建过程。

- 虚拟机默认 CPU*2+内存1G+硬盘20G，可以根据项目情况自行调整
- CentOS 的 `root` 密码是 `bootdev`
- MySQL 的 `root` 密码是 `bootdev`

## 安装（Windows）

1、从网上邻居下载 `VMware Player` 并安装。

*10.113.8.116\setups\VMware-player-16.0.0-16894299.exe*

2、从网上邻居下载虚拟机文件，解压到任意文件夹，并使用 VMware Player 打开虚拟机。

*10.113.8.116\projects\dev-php\dev.vmwarevm.7z*

该文件会根据项目进展不断更新，届时使用新文件覆盖安装即可。

3、启动虚拟机并通过账号 `root` 和密码 `bootdev` 登录 CentOS，输入 `ip addr` 命令得到 CentOS 的 `虚拟机IP`。

*虚拟机第一次启动可能会出现“此虚拟机可能已被移动或复制”的提示，选择“我已复制该虚拟机”即可*
*如果提示 VMware Tools for Linux 下载，可以忽略*

![](images/getip.png)

4、回到 Windows，打开 CMD 或者 PowerShell，通过 `ssh` 命令行登录虚拟机，之后就可以通过 [命令行管理 CentOS](命令行管理CentOS) 了。

也可以通过 [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 更方便的管理登录 `ssh`。

```bash
$ ssh root@虚拟机IP
# 密码输入 bootdev
```

至此开发环境安装成功！

> `macOS` 和 `Linux Desktop` 也是同样的安装方式，本文不再重复举例。

## 新项目部署实例

本实例演示一个 `Laravel` 项目在虚拟机中从创建到运行的完整过程，项目名称暂定为 `project-1`：

1、在 Windows 中创建项目文件夹 `D:\test`

2、打开虚拟机设置，进入 `共享` 功能，添加 `D:\test\` 并命名为 `projects`。

*共享文件夹的作用是让 Windows 中的 `Ngxin设置` 和 `PHP代码` 在 CentOS 中运行。*

3、通过 `ssh` 命令行登录虚拟机。

4、共享文件夹在 CentOS 中的路径是 `/mnt/hgfs/projects`，我们要在这个文件夹中创建 `project-1` 项目：

```bash
$ cd /mnt/hgfs/projects
$ composer create-project --prefer-dist laravel/laravel project-1 "6.*"
```

创建完毕后，Windows 中的 `D:\test` 也会出现 `project-1` 文件夹。

5、在 `D:\test` 中创建 Nginx 配置文件 `vhosts\project-1.conf` 内容如下（支持Laravel）：

```nginx
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _;

    root         /mnt/hgfs/projects/project-1/public;

    include /etc/nginx/default.d/laravel.conf;
}
```

6、重启动虚拟机,在 Windows 的浏览器或 RESTFul 客户端输入 `虚拟机IP:80` 访问我们的 `project-1` 应用。

至此新项目部署完毕，我们还可以创建 `project-2`、`project-3` ... 等其它项目，要注意不同项目的 Nginx 端口号不能重复，否则 Nginx 无法启动。

## 外网访问虚拟机

虚拟机网络默认是NAT模式，可以通过端口转发实现外网访问虚拟机中的服务。

```bash
netsh interface portproxy add v4tov4 listenaddress=127.0.0.1 listenport=80 connectaddress=虚拟机IP connectport=80
```

可以参考这篇文章：https://zhuanlan.zhihu.com/p/266473873

## 命令行管理 CentOS

执行 `Laravel` 的 `artisan` 命令：

```bash
$ cd /mnt/hgfs/projects/project-1
$ php artisan XXX
```

Nginx 常用命令：

```bash
$ systemctl start nginx #启动
$ systemctl stop nginx #停止
$ systemctl restart nginx #重启
```

PHP-FPM 常用命令：

```bash
$ systemctl start php-fpm #启动
$ systemctl stop php-fpm #停止
$ systemctl restart php-fpm #重启
```

MySQL 常用命令：

```bash
$ systemctl start mysqld #启动
$ systemctl stop mysqld #停止
$ systemctl restart mysqld #停止
$ systemctl enable mysqld #启用MySQL服务
$ systemctl disable mysqld #禁用MySQL服务
```

系统命令：

```bash
$ ping 192.168.1.1 #PING，可以测试网络连接
$ curl https://www.google.com.hk #CURL，可以测试网络连接
$ htop #系统监视器
$ df -h #磁盘空间
$ ps -aux|grep nginx #当前启动的 Nginx 进程
$ ps -aux|grep php #当前启动的 PHP 或 PHP-FPM 进程
$ ps -aux|grep mysql #当前启动的 MySQL 进程
$ reboot #重启动
$ poweroff #关机
$ exit #退出 CentOS 终端
```
