
# Ubuntu Python开发使用文档


http://demo.pythoner.com/itt2zh/index.html

## YKT环境使用文档

    sudo apt-get update 更新软件库

## Ubuntu 安装JDK参考文件
>https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get

第一步：更新软件库

    sudo apt-get update

第二步：选择安装Oracle JDK7 ，安装之前需要添加依赖

    一、添加使用的工具
         sudo apt-get install python-software-properties
    二、添加依赖
         sudo add-apt-repository ppa:webupd8team/java
    三、更新软件库
         sudo apt-get update
    四、安装JDK7
         sudo apt-get install oracle-java7-installer
安装完成后，执行java -version 查看安装信息

## Ubuntu 安装intellij Idea
参考文档
http://askubuntu.com/questions/272314/setup-and-install-intellij

第一步：下载Intellij IDEA 下载地址 www.jetbrains.com/idea/download/

第二步：解压缩： tar -zxvf ideaIC-XX.Y.Z.tar.gz

第三步：安装 sudo -i

第四步: 这一步是移动目录的 mv ideaIC-XX.Y.Z /opt/idea

第五步: 用gedit 些一个名称叫idea.sh文件  

    gedit idea.desktop
    
    文件内容：Copy进去就可以了
    [Desktop Entry]
    Name=IntelliJ IDEA
    Type=Application
    Exec=idea.sh
    Terminal=false
    Icon=idea
    Comment=Integrated Development Environment
    NoDisplay=false
    Categories=Development;IDE;
    Name[en]=IntelliJ IDEA
    然后执行下面的命令来自动安装
    desktop-file-install idea.desktop

第六步：创建一个应用链接

    cd /usr/local/bin
    ln -s /opt/idea/bin/idea.sh
第七步：为Idea创建图标

    cp /opt/idea/bin/idea.png /usr/share/poxmaps/idea.png
    

安装完成后，在终端执行 idea.sh，完成设置
在第一次打开的时候提示安装 Scala 、Live edit tool、IdeaVIM根据需求安装

## ubuntu Idea 使用python

参考文档http://little-bill.iteye.com/blog/1354518
打开Idea 进入File>Setting>plugins>browse repositorits 搜索python,下载安装python plugins 安装完成后重启Idea


## ubuntu 安装 PyCharm

参考文档

    http://www.cnblogs.com/zhcncn/p/4027025.html

一、下载PyCharm最新版本

二、进入安装包目录cd Download

三、解压缩文件 tar xfz pycharm-*.tar.gz

四、删除压缩包 rm pycharm-*.tar.gz

五、进pycharm-*/bin目录，执行pycharm.sh 进行安装

六、设置Ubuntu下Pycharm的快捷启动方式
    在Ubuntu下，每次都要找到 pycharm.sh所在的文件夹，执行./pycharm.sh
    sudo gedit /usr/share/applications/PyCharm.desktop
    然后输入以下内容，注意Exec和Icon需要找到正确的路径

    [Desktop Entry]
    Type=Application
    Name=Pycharm
    GenericName=Pycharm4
    Comment=Pycharm4:The Python IDE
    Exec="/opt/pycharm-4.5.1/bin/pycharm.sh" %f
    Icon=/opt/pycharm-4.5.1/bin/pycharm.png
    Terminal=pycharm
    Categories=Pycharm;

    然后双击打开，再锁定到启动器就好了.

## ubuntu 安装tornado

一、安装python一依赖，pip

    sudo apt-get install python-pip
二、安装tornado

    sudo pip install tornado

## ubuntu 安装git

    sudo apt-get install git

## ubuntu git key 生成

一、创建SSH Key

    $ ssh-keygen -t rsa -C "youremail@example.com"

 
二、将id_rsa.pub里面的公钥添加值gitgub上



安装完ubuntu系统后不能进入的解决办法
1、在启动菜单中选中*ubuntu 按e进入启动编辑模式，将编辑中的ro修改为rw按F10进入
2、进入系统后调出终端。输入sudo gedit /etc/grub.d/10_lupin
打开10_lupin搜索ro ${args} 将ro修改为rw保存并退出文本
3、在终端中输入 sudo update-grub 更新启动项配置


## 安装 PyMongo 用来支持 MongoDB

    sudo pip install pymongo
## 安装MongoDB

    http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

开始MongoDB

    sudo service mongod start

## 安装 python-dev

    sodo apt-get install python-dev

## 安装 motor

    sudo pip install motor

安装motor时也有可能出现error: command 'x86_64-linux-gnu-gcc' failed with exit status 1,这个是因为没有安装python-dev

安装motor时有可能出现ascii编码的问题，需要修改pip的__init__.py和basecommand.py文件 路径/usr/lib/python2.7/dist-packages/pip
安装motor时有可能出现ascii编码的问题，需要修改pip的__init__.py和basecommand.py文件 路径/usr/lib/python2.7/dist-packages/pip
将 import sys修改为

    import sys
    reload(sys)
    sys.setdefaultencoding('utf8')


##安装redis 驱动

    sudo pip install ujson redis

##安装redis 服务器

    sudo apt-get install redis-server

启动redis

     sudo redis-server
-------------------------------
以上是系统安装的问题

前台使用AdminLTE
    https://almsaeedstudio.com/preview

##安装 nginx

    sudo apt-get install nginx

修改配置

    安装版-本地

        sudo gedit /etc/nginx/sites-available/default

    编译版-服务器
        3.安装依赖
        sudo apt-get install libgd2-dev libpcre3 libpcre3-dev zlib1g-dev libpcap-dev libssl-dev libssl-dev openssl nginx-extras-dbg

        4.解压后放到nginx的modules下面,并进入nginx根目录配置
        ./configure --add-module=modules/nginx-upload-module-2.2 --add-module=modules/nginx-upload-progress-module --with-http_image_filter_module

        5.编译nginx
        make     
        make install
        
            sudo vim /usr/local/nginx/conf/nginx.conf

    server {
        client_max_body_size 100m;
        listen 80;
        server_name localhost;
        default_type text/html;
        
        location /uploads {
            proxy_pass http://192.168.7.84/uploads;
        }
        
        location / {
            proxy_pass http://127.0.0.1:8000;
        }
    }

重新启动nginx

    安装版-本地

        sudo /etc/init.d/nginx restart

    编译版-服务器

        sudo /usr/local/nginx/sbin/nginx -s stop && sudo /usr/local/nginx/sbin/nginx

        sudo /usr/local/nginx/sbin/nginx
        

nginx端口被占用时杀掉进程
    
    sudo fuser -k 80/tcp

        
创建上传文件

     sudo mkdir -p /data/yktong/uploads
     sudo ruby -e 'require "fileutils"; (0..9).each{|i| FileUtils.mkdir_p("/data/yktong/uploads/#{i}") rescue nil }'
     sudo chmod -R 777 *


sudo ruby -e 'require "fileutils"; (0..9).each{|i| FileUtils.mkdir_p("/data/yktong/uploads/images/user/#{i}") rescue nil }'
###解压缩文件乱码解决办法

1.安装（12.04及以上）：

    sudo apt-get install unar

使用：

    假设需要解压的ZIP包是foo.zip

lsar foo.zip #列出所有文件

     如果列出的文件名已经正确

unar foo.zip #解压所有文件

    如果列出的文件名还不正确

lsar -e GB18030 foo.zip #指定使用GB18030编码列出所有文件
unar -e GB18030 foo.zip #指定使用GB18030解压所有文件

    注：GB18030编码文件名的ZIP文件一般由简体中文版Windows产生，对于繁体中文版Windows产生的ZIP文件可以尝试BIG5-HKSCS编码，对其他语种的常见编码不再赘述。通用的原则是用lsar测试出正确的编码以后，用unar解压
    
    
    
    
gitHub 配置
    
    
    http://blog.csdn.net/lmmilove/article/details/40587393
    
    官方的
    https://about.gitlab.com/downloads/#ubuntu1404
    