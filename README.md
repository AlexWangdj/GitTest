#在线监测系统

## *Online Inspection System*

###运行环境

组件 | 版本
-------- | --------
`ubuntu` | `14.04`
`apache` | `2.4.7`
`php`    | `5.5.9`
`mysql`  | `5.5.50`

###配置说明

**基于 Ubuntu 14.04**

#### 第一部分
- 安装Ubuntu系统。
- 添加root用户 `sudo passwd`,并登录`su`。
- 配置ip ， 服务器ip网卡名字为 em1、em2、em3、em4，编辑配置文件：`sudo vi /etc/network/interfaces`，并用下面的行来替换有关em1的行：
    ``` 
    # The primary network interface
    auto em1
    iface em1 inet static
    address IP地址
    gateway 网关
    netmask 子网掩码
    ```
- 安装更新`apt-get update`。
- 安装ssh、vim、zip、git、lrzsz，`apt-get -y install ssh vim zip git lrzsz`。
- 安装mysql、apache，`apt-get -y install mysql-server mysql-client apache2`。
- 安装php，`apt-get -y install  php5 php5-mysql php5-cgi libapache2-mod-fcgid libfcgi-dev`。
- 进入目录，`cd /var/www/html`。
- 初始化git，`git init`。
- 添加源码地址，`git remote add origin https://github.com/irplayer/web.git`。
- 从github下载代码，`git pull origin master`。
- 创建软链接，`ln -s /var/www/html/site.conf /etc/apache2/sites-available/`。
- 禁用默认的主机配置，`a2dissite 000-default`。
- 然后启用新的主机配置，`a2ensite site.conf`。
- 执行加载Rewrite模块，`a2enmod rewrite`。
- 重启apache服务器，`service apache2 restart`。
- 将数据库文件导入到系统，可以利用Xshell的`rz`指令来实现。
- 进入mysql数据库，`mysql -uroot -p`。
- 创建名为db_gp的数据库并使用，`create database db_gp;`，`use db_gp;`。
- 导入sql文件，`source \var\www\html\db_gp.sql;`。
- 为数据库添加账户，并赋予全部权限，`grant all privileges on *.* to admin@'%' identified by 'admin';`。
- 修改mysql配置文件，`vim /etc/mysql/my.cnf`，将`bind-address =   127.0.0.1`删除或注释。
- 重启系统`reboot`。

#### 第二部分
- 安装make`apt-get install make`。
- `apt-get install libmysqlclient-dev`。
- 安装samba，`apt-get install samba`。
- 配置samba服务器
    ```
    [sambaserver]
    path = /home/samba
    read only = No
    # public = yes
    valid users = hs 
    #valid users = root
    public = no
    writable = yes
    printable = no
    create mask = 0644
    directory mask = 0755

    [www]
    path = /var/www
    read only = No
    # public = yes
    valid users = hs 
    #valid users = root
    public = no
    writable = yes
    printable = no
    create mask = 0644
    directory mask = 0755
    ```
- 安装c和c++编译器`apt-get install gcc`和`apt-get install g++`。
- `apt-get install pkg-config`。
- 安装ffmpeg和x264  
  1.编译yasm，进入yasm目录，依次输入以下指令：  
    ```
    ./configure --prefix=/usr/local/yasm
    make
    make install
    ```
  2.导入并解压x264，进入目录，输入以下指令：  
    ```
    ./configure --prefix=/usr/local/x264 --enable-shared --enable-static
    make
    make fprofiled
    make install
    ```
  3.导入并解压ffmpeg，进入目录，输入以下指令：  
    ```
    ./configure --prefix=/usr/local/ffmpeg --enable-shared --enable-yasm --enable-libx264 --enable-gpl --enable-pthreads --extra-cflags=-I/usr/local/x264/include --extra-ldflags=-L/usr/local/x264/lib  
    #!/bin/sh
    ./configure --prefix=/usr/local/ffmpeg \
                --enable-shared \
                --enable-yasm \
                --enable-libx264 \
                --enable-decoder=h264 \
                --enable-gpl \
                --enable-pthreads    \
                --extra-cflags=-I/usr/local/x264/include \
                --extra-ldflags=-L/usr/local/x264/lib \
                --enable-network \
                --enable-protocol=tcp \
                --enable-demuxer=rtsp
    make
    make install
    ```
  4.编译完成后，修改环境编译，在/etc/profile文件下增加如下字段：  
    ```
    FFMPEG=/usr/local/ffmpeg
    X264=/usr/local/x264
    YASM=/usr/local/yasm
    export FFMPEG X264 YASM
    export PATH=$PATH:$FFMPEG/bin:$X264/bin:$YASM/bin
    #export LD_LIBRARY_PATH=$YASM/lib:$X264/lib:$FFMPEG/lib:$LD_LIBRARY_PATH
    ```
- 用户组设置。


**基于 Windows 7 & Windows 10**

- 安装 `phpStudy` 到 `D:\php` 文件夹，复制 `cgi-bin`、`html`、`media` 文件夹到 `D:\var\www` 文件夹。
- 配置 `mysql` 用户权限。数据连接在 `D:\var\www\html\cgi\inc\common.php` 文件中配置。
- 复制 `D:\var\www\html\site.conf` 为 `D:\php\Apache\conf\vhosts.conf` 。
- 打开 `phpStudy` ，切换 `php版本` 至 `"apache + php5.5"`，切换 `运行模式` 至 `"系统服务"` ，重启 `phpStudy` 。

