## 构建属于Ansible的ContOS

### 初始配置
```shell
docker run -d -it -p 6000:8080 --name my-go1.17 --privileged=true huangbosbos/ansible-centos:v0.2 /usr/sbin/init

docker exec -it my-go1.17 /bin/bash
```

### 打包
```shell
docker commit \
    --author "Lucky <huangbosbos@gmail.com>" \
    --message "ansible-centos new" \
    ansible-centos \
    ansible-centos:v0.3

docker tag ansible-centos:v0.3 huangbosbos/ansible-centos:v0.3

docker push huangbosbos/ansible-centos:v0.3
```

### 内置设置
```shell
mkdir -p /home/golang   
cd /home/golang   
wget https://golang.google.cn/dl/go1.17.linux-amd64.tar.gz  
tar -zxf go1.17.linux-amd64.tar.gz -C /usr/local  
```

vim /etc/profile  

```
#golang env config
export GO111MODULE=on
export GOROOT=/usr/local/go 
export GOPATH=/home/gopath
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

```shell
source /etc/profile  
go env -w GOPROXY=https://goproxy.cn,direct  
source /etc/profile  
go env -w "GOFLAGS"="-mod=mod"   
cd /data/gopath    
mkdir -p /data/gopath  
cd /data/gopath  
```

vim hello.go  

```golang
package main  
import "fmt"  
func main() {  
    fmt.Printf("Hello, world!\n")  
}  
```
go rum hello.go 
