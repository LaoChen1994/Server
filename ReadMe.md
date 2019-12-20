# 服务器使用方法

### 1. 用户申请

+ 向老师进行申请
+ 由管理员进行统一的添加用户和权限

### 2. 服务器使用方法

+ 必须使用校园网络，目前通过网线进行链接没有问题

+ Linux

  + 连接: 使用ssh 进行连接到远程服务器: ssh chenyx@remote_ip  (remote_ip目前为172.17.161.137)

  + 文件传输: 

    ~~~bash
    # 文件上传
    scp file_source file_target
    
    #　例子：复制单一文件
    scp /home/space/test.txt chenyx@172.17.161.137:/home/chenyx/other/music
    
    # 例子：上传文件夹
    scp -r /home/space/music chenyx@172.17.161.137:/home/chenyx/other/music
    
    
    # 文件下载
    scp chenyx@172.17.161.137:/home/chenyx/other/music /home/space/test.txt
    ~~~

+ Windows

  + 连接: 在Windows系统搜索远程桌面连接，然后输入remote_ip  (remote_ip目前为172.17.161.137) 点击连接，进入后输入用户名，密码即可。

  + 文件传输: 
    ~~~bash
    # 1. pscp下载
    # 从http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html 下载pscp，将其放入windows的system32文件夹下
    
    # 2. 上传文件
    # 1.开始→运行→cmd进入到dos模式 输入以下命令:
    pscp D:\java\apache-tomcat-5.5.27\webapps\szfdc.rar（即文件地址） chenyx@172.17.161.137:/home/chenyx/test
    # 2.输入密码 ok 文件已经上传到服务器的/home/chenyx/test目录下了
    
    # 3. 下载文件
    # 1.开始→运行→cmd进入到dos模式 输入以下命令:
    pscp dev@172.17.161.137:/home/dev/gren.sql d:\gren.sql
    
    # 其中：dev为linux的用户名，192.168.68.248为远程Linux主机ip地址，/home/dev/gren.sql为linux下的文件，d:\gren.sql为保存在本地的文件。
    
    #例子：文件下载
    pscp chenyx@172.17.161.137:/home/chenyx/gren.sql d:\gren.sql
    ~~~

 ### 3. linux的常用命令

​    常用的linux命令可以参考博客[linux常用的27个命令](https://www.jianshu.com/p/0056d671ea6d)

### 4. 常见问题

**共用环境的目录：**　'/opt/'

+ **Q1**: 如何使用共用的conda环境

+ A1: 

  + 备份原来bashrc文件
  + 将共有的conda环境设置为用户的环境变量
  + 在conda环境下挂载自己的虚拟环境(必须这么弄, 不然会污染全局,大家必须遵守)
  + 进入虚拟环境,进行自己环境的配置

  ~~~bash
  # 文件备份
  cp ~/.bashrc ~/.bashrc.bak
  
  # 添加环境变量
  echo 'export PATH=$PATH:/opt/anaconda3/bin/' >> ~/.bashrc
  
  # 查看环境变量是否成功, 如果成功上述命令应该存在于bashrc文件的最后一行
  cat ~/.bashrc
  
  # 如果上述成功
  source ~/.bashrc
  
  # 查看conda 使用是否正常,能看到版本号即为正常
  conda -V
  
  # 创建conda虚拟环境
  
  # 1. 查看已有环境名称
  conda info -e
  
  # 2. 创建和原来环境名不同的环境
  conda create -n [自己的环境名] python=[x.x(你想要的python版本号)]
  
  # 3. 进入虚拟环境
  source activate [自己的环境名]
  ~~~

+ **Q2**: 图形界面死机怎么办？

+ A2: 重启图形界面后登录

  + Ctrl + Alt + F1推出图形界面，进入命令行模式
  
+ 关闭现有图形界面　
  
    ~~~bash
    # 关闭Xorg
    sudo pkill Xorg
    ~~~
  
  + 重新链接
  
+ **Q3**: 使用pycharm配置独立的虚拟环境时，需要添加conda环境，但是在自身目录环境下的.conda/envs/username/bin下没有找到conda?

+ 解决方法

    + 方法1: 直接添加公共目录下的conda添加环境为/opt/anaconda3/bin/conda

    + 方法2: 拉一个软链接过来

        ~~~bash
        # 拉个软链接到自己的conda目录下　以我的用户名cyx为例
        ln -s /opt/anaconda3/bin/conda ~/.conda/envs/cyxTest/bin/conda
        ~~~

        之后添加conda目录为~/.conda/envs/cyxTest/bin/conda即可

#### 5. 关于使用者值班表进行值班的事宜

​	为了能够更方便的协调显卡的使用，这里采取值班制度，显卡的使用时间和使用显卡的id号需要记录在案，其具体的申请规则和记录方法如下：

​	申请规则：

	1. 最多一次可占用两张显卡
 	2. 占用显卡必须在/opt/dutyList.md中进行记录(显卡id+使用时间+使用者名称)
 	3. 占用显卡应为没有被使用的空闲显卡

记录方法:

~~~bash
# 1. 查询空闲显卡id
nvidia-smi

# 2. 进入opt目录编辑文档
sudo vi /opt/dutyList.md

# 3. 按照要求填写即可
~~~