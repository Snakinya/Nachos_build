# Nachos_build
# Docker安装教程
首先下载nachos_docker.tar.gz

链接：https://pan.baidu.com/s/1oOB7hHqkUkD2VBD1DPiINQ 

提取码：4952

下载后解压
```
tar -zxvf nachos_docker.tar.gz
```
载入镜像
```
sudo docker load -i nachos_docker.tar
```
载入后利用
```
sudo docker images
```
建立数据卷并查看
```
docker volume create volume_name
docker volume inspect volume_name
```
创建容器并完成nachos到数据卷的挂载
```
docker run -dit --name nachos-v4.1 -v volume_name:/home/nachos-v4.1 image_id
```
进入容器操作界面，宿主机可以通过mountpoint编辑文件
```
docker exec -it container_id /bin/bash
```
# Linux安装教程

本教程基于ubuntu 20.04系统安装

## 01 查看系统发行版本

```
sudo lsb_release -a
```


## 02 确定linux位数

Nachos依赖32位编译环境，如果是64位操作系统则需要安装32位编译环境。

```
dpkg --print-architecture 
```


## 03 Ubuntu 32位编译环境搭建

### 安装linux基本开放环境

```
sudo apt install build-essential
```


### 开启32位支持

```
sudo dpkg --add-architecture i386
```

### 安装C，C++多平台库

```
sudo apt install gcc-multilib g++-multilib
```


### 安装32位开发环境

#### 1 Lib32ncurses6（可能默认已安装）

```
sudo apt install lib32ncurses6
```

#### 2 Lib32z1

```
sudo apt install lib32z1 lib32z1-dev
```

#### 3 安装zlib1g,zlib1g-dev

```
sudo apt install zlib1g zlib1g-dev
```

#### 4 安装32位c和c++库：libstdc++6:i386 , libc6:i386,libgcc1:i386

```
sudo apt install libstdc++6:i386
sudo apt install libc6:i386
sudo apt install libgcc1:i386
```

#### 5 安装低版本gcc编译器（低于5.3）

查看gcc版本

```
gcc -v
```

修改源文件

```
sudo vim /etc/apt/sources.list
```

```
deb http://mirrors.aliyun.com/ubuntu/ xenial main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
```

添加源后更新gcc

```
sudo apt update
```

#### 6 安装低版本C和C++编译器

```
sudo apt-get install libgcc-4.8-dev gcc-4.8 gcc-4.8-multilib g++-4.8 g++-4.8-multilib
```

#### 7 改变符号链接gcc和g++指向的实际编译器

```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 40
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 40
```

### 编译Nachos源码

解压源码
这里需要解压到/usr/loacl目录下并重命名为nachos

```
tar xvzf nachos.tar.gz
```

进入目录观察结构

```
cd Nachos-4.1
tree
```


编译源码

[1] 进入NachOS下的code目录下的build.linux子目录

[2] 清除以前的编译结果并重新编译

```
make clean
rm -rf nachos
make depend
make
```

[3]测试

```
./nachos -u
```


### 安装交叉编译器

将交叉编译器移动到根目录并解压

```
mv tar xvzf mips-decstation.linux-xgcc.tgz /.
tar xvzf mips-decstation.linux-xgcc.tgz
```

####  编译mips平台可执行文件格式转换工具

切换到Nachos目录，进入到格式转换工具目录coff2noff

生成Makefile并编译

```
./configure
vim Makefile
make
```
![makefile](https://cosmoslin.oss-cn-chengdu.aliyuncs.com/img2/image-20220404224916016.png)

#### 修改mips用户空间makefile, 搭建用户态程序测试环境

进入到Nachos目录下code子目录下的test目录

检查Makefile.dep下的交叉编译器路径是否正确

编译test目录下的用户程序

测试用户态程序是否能在nachos 上运行

```
../build.linux/nachos -x add.noff
```


