## 构建属于Ansible的ContOS
这里根据Dockerfile方式构建一个CentOS的可远程SSH的镜像。

### Dockerfile文件
vim Dockerfile
```shell script
# Ansible CentOS Dockerfie
# 生成的新镜像以centos镜像为基础
FROM centos:7
# 指定作者信息
MAINTAINER lucky

#CentOS Linux release 7.9.2009 (Core)
RUN cat /etc/redhat-release

RUN mkdir -p /vdb/tower
WORKDIR /vdb/tower

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN yum -y install epel-release --nogpgcheck
RUN yum -y install ansible vim curl deltarpm wget sudo
RUN yum -y install yum-utils
RUN yum -y install python-devel
RUN yum -y install postgresql
RUN yum -y install httpd
RUN yum -y install net-tools
RUN yum -y install initscripts
RUN yum -y install telnet
RUN yum -y install gcc gcc-c++ libstdc++-devel

# 安装openssh-server
RUN yum -y install openssh-server

RUN mkdir /var/run/sshd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key

# 指定root密码
RUN /bin/echo 'root:123456'|chpasswd
RUN /bin/sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd
RUN /bin/echo -e "LANG=\"en_US.UTF-8\"" > /etc/default/local
EXPOSE 22
CMD /usr/sbin/sshd -D
```

### build镜像
在Dockerfile当前目录执行下面语句，开始构建镜像。
> 注意：最后面的点不要忘了，表明是读取当前目录的Dockerfile文件。
```shell script
docker build -t ansible-centos:v1.0 .

docker build --no-cache --squash -t huangbosbos/ansible-centos:v1.0 .
```

### 查看镜像
```shell script
docker images
```

### 启动容器
```shell script
docker run -itd -p 10022:22 --name centos_1 huangbosbos/ansible-centos:v1.0
```

打包成功的话会出现下面的提示，可能时间会有点长，耐心等待。
### 引用
```shell script
FROM ansible-centos:v1.0
```

## 参考
[Docker实现CentOS容器SSH远程登录](https://blog.csdn.net/A632189007/article/details/78625378)
