# Linux安装配置nginx1.18.0服务器
## 一、安装相关依赖
1. 安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境安装
> yum  -y install  gcc gcc-c++ popt-devel openssl-devel
2. PCRE pcre-devel 安装 （nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库）、
> yum install -y pcre pcre-devel
3. zlib 安装（ nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库）
> yum install -y zlib zlib-devel
4. OpenSSL 安装 （nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http））
> yum install -y openssl*
5. ncurses-devel安装
> yum -y install ncurses-devel
## 二、安装包下载或上传
1.方式一、下载
> wget https://nginx.org/download/nginx-1.18.0.tar.gz

2.方式二、上传
```
cd soft
rz
nginx-1.18.0.tar.gz
```
## 三、nginx的编译安装
1. 创建文件夹
```
mkdir -p /usr/local/nginx
```
2. 解压指定安装目录

```
tar -zxvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
./configure --prefix=/usr/local/nginx
```
3. 编译安装

```
make && make install
```
## 四、启动脚本的配置
1.上传安装
```
cd /etc/init.d
rz #上传文件
# 给文件赋权限
chmod +715 nginx
# 添加nginx开启启动
chkconfig --list
chkconfig --add nginx
chkconfig nginx on
```
2.结合nginx.service操作nginx命令

```
 systemctl status nginx
 systemctl stop nginx
 systemctl start nginx
 systemctl restart nginx
 # 验证配置文件是否正确
  /etc/init.d/nginx configtest
 # 不停机重新启动
 /usr/nginx/sbin/nginx -s reload
```
