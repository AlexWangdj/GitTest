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

- 安装Ubuntu系统。
- 添加root用户 `sudo passwd`,并登录`su`。
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
