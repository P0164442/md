# **1、下载php7.3**
    https://windows.php.net/download#php-7.3-ts-VC15-x64
   
1. PHP根目录下的php.ini-development复制一份并改名为php.ini
2. 找到extension_dir，修改为，我的路径
   
        extension_dir = "E:\php\php7.3/ext"
3. 环境变量path新增
   
        E:\php\php7.3
4. 备注，php.ini下面的几条常用配置，去掉前面的分号，保存.(没测试过)
   
        ;extension=curl
        ;extension=gd2
        ;extension=mbstring
        ;extension=mysqli
        ;extension=openssl
        ;extension=pdo_mysql
        ;extension=pdo_oci
        ;extension=pdo_odbc
        ;extension=pdo_pgsql
        ;extension=pdo_sqlite
        ;extension=pgsql

# **2、下载对应的vc**
    PHP 7.0 和 7.1 需要 VC CRT 14（Visual Studio 2015）。 PHP 7.2, 7.3 and 7.4 需要 VC CRT 15 (Visual Studio 2017)。 微软的开发环境 Visual Studio 2019 包含了编译 PHP 的全部环境要求，请参阅 » https://visualstudio.microsoft.com/downloads/。

# **3、下载Apache Lounge（apache不提供现成的编译包，下载第三方的）**
    https://www.apachelounge.com/download/VC15/

1. 放到C盘下面
    C:\Apache24
2. 打开文件

        C:\Apache24\conf\httpd.conf
3. 修改如下相关（另外如果默认的80端口被其他程序用了也需要修改）
   
         LoadModule php7_module "E:\php\php7.3\php7apache2_4.dll" 
        <IfModule php7_module> 
            PHPIniDir "E:\php\php7.3"
            AddType application/x-httpd-php .php
            AddType application/x-httpd-php-source .phps
        </IfModule> 

        <IfModule dir_module>
            DirectoryIndex index.html index.php
        </IfModule>
4. 安装Apache服务
 
   4.1 管理员权限打开CMD，进入Apache的bin目录

   4.2 输入以下命令，进行Apache服务的安装，出现The 'Apache2.4' service is successfully installed.

        httpd.exe -k install

    4.3 C:\Apache24\htdocs下面，新增index,php文件，内容是

        <?php 
        echo phpinfo();
5. 备注
   
    5.1 如果更改php文件目录，找到 `DocumentRoot "${SRVROOT}/htdocs"`，将"${SRVROOT}/htdocs"改成你的web目录，即你想存放web工程的地方。 

     5.2 如果Apache2.4不放在C盘下面，修改Apache目录找到`Define SRVROOT "/Apache24"`，将"/Apache24"改成Apache所在的目录

    5.3 有一个说法,把Require all denied改成Require all granted,未验证是做什么用的

        <Directory />
            AllowOverride none
            Require all denied
        </Directory>



# **4、测试**
1. 管理员权限打开CMD，进入Apache的bin目录
2. 输入以下命令，启动Apache服务（好像是自动的，以后开机不需要再手动开启）

        net start Apache2.4
3. 测试

        http://localhost/index.php



