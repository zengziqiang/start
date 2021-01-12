### Linux安装配置redis5
## 1、下载上传redis5安装包

```
mkdir soft
cd soft
## 上传或下载
wget http://download.redis.io/releases/redis-5.0.8.tar.gz
redis-5.0.8.tar.gz
```
## 2、解压安装包

```
tar -zxvf redis-5.0.8.tar.gz
cd redis-5.0.8
```
## 3、安装依赖环境
> yum install gcc
## 4、编译(时间可能稍稍长一点)

```
# 在redis-5.0.8目录下执行
make
# 如果make出错,解决错误后需要执行
make distclean
```
## 5、创建安装目录并安装

```
mkdir -p /usr/local/redis5
make PREFIX=/usr/local/redis5 install
```
## 6、配置环境变量
```
vim /etc/profile
# 在文件尾添加以下配置
export REDIS_HOME=/usr/local/redis5
export PATH=$PATH:$REDIS_HOME/bin
# 保存后
source /etc/profile
```
## 7、启动安装实例

```
# 进入安装工具文件夹
cd utils/
# 执行
./install_server.sh 
#一步步回车就行了
```
## 8、查看启动情况
`ps -ef | grep redis`
## 7、修改配置文件
> vim /etc/redis/6379.conf

1. 将daemonize no 修改为daemonize yes（意思是，启动方式为后台启动）
2. 将bind 127.0.0.1 修改为bind 0.0.0.0（意思是，准许远程连接）
3. 修改登录密码# requirepass foobared将前面的#去掉 将foobared 修改为自己的密码（默认为空）
## 9、重启验证

```
/etc/init.d/redis_6379 restart
```
