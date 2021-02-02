    备注：虚拟机以root为操作账号。
    mysql 退出一般用quit，exit。
    cd ~  可以回到主目录
---
#下面是mysql安装
---
###1.安装虚拟机

###2.执行yum更新
```bash
    yum -y update
```

###3.下载wget
```bash
    yum install wget -y
```

###4.下载mysql源安装包
```bash
    wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

###5.安装mysql
```bash
    yum localinstall mysql57-community-release-el7-8.noarch.rpm
```

###5.验证安装
```bash
    yum repolist enabled | grep "mysql.*-community.*"
```
结果如图：
![mysql验证](/images/redmine/1.png)

###6.执行安装
注意：如果不执行yum module disable mysql，则安装的时候会报错
*<font color=red>Error: Unable to find a match: mysql-community-server</font>*
```bash
    yum module disable mysql
    yum install mysql-community-server
```

###7.启动MySQL
```bash
    systemctl start mysqld
```

###8.查看启动状态
```bash
    systemctl status mysqld
```

结果如图：
![mysql验证](/images/redmine/2.png)

###9.设置开机自启动
备注：daemon-reload: 重新加载某个服务的配置文件，如果新安装了一个服务，归属于 systemctl 管理，要是新服务的服务程序配置文件生效，需重新加载。
```bash
    systemctl enable mysqld
    systemctl daemon-reload
```
###10.安装vim
```bash
    yum install -y vim
```
备注：vim常用命令，更详细的请查看 [vim 常用命令大全（转）](https://www.cnblogs.com/chen-nn/p/11531932.html)

    i:进入编辑状态
    :w 保存文件但不退出vi
    :w file 将修改另外保存到file中，不退出vi
    :w! 强制保存，不推出vi
    :wq 保存文件并退出vi
    :wq! 强制保存文件，并退出vi
    q: 不保存文件，退出vi
    :q! 不保存文件，强制退出vi
    :e! 放弃所有修改，从上次保存文件开始再编辑

###11.查看mysql临时密码
mysql安装完成之后，在/var/log/mysqld.log文件中有一个默认临时密码，用户名是root。查看密码：
```bash
    grep 'temporary password' /var/log/mysqld.log
```
备注：也可以通过vim命令，直接去log文件查看临时密码。

###12.输入账号和临时密码登录

```bash
    mysql -uroot -p
```
结果如图：
![mysql验证](/images/redmine/3.png)

###13.修改MySQL密码 
*<font color=red>注意：mysql的语法，后面要加“;”</font>*
```bash
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'Win2003@';
```

###14.设置root账号可远程访问，远程访问MySQL
*<font color=red>默认root的账号只能localhost本地访问的，如需要远程访问，还需要如下设置</font>*
####14.1.进入mysql数据库
如果执行了13步，默认应该是在mysql下，如果之前退出了，则执行下面命令重新进入，密码是13步修改的新密码。
```bash
    mysql -uroot -p
```
####14.2.添加远程访问密码
```bash
    GRANT ALL ON *.* TO root@'%' IDENTIFIED BY 'Win2003@' WITH GRANT OPTION;
```
####14.3.刷新修改
```bash
    flush privileges;
```
####14.4.退出mysql
```bash
    exit;
```
###15.开启防火墙（root下）
```bash
    firewall-cmd --permanent --zone=public --add-port=3306/tcp
    firewall-cmd --reload
```
###16.使用工具链接
结果如图：
![mysql验证](/images/redmine/4.png)

---
#下面是redmine安装
---
###1.redmine安装