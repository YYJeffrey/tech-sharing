## 系统环境

CentOS7（4H16G） http://isoredirect.centos.org/centos/7/isos/x86_64/
PackStack https://www.rdoproject.org/install/packstack/

## 安装方式

packstack all-in-one

## 安装之前

```shell
# 开启SSH服务
vim /etc/ssh/sshd_config
Port 22
systemctl restart sshd

# 配置固定IP
vim /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE="eth0"
BOOTPROTO="static"
ONBOOT="yes"
IPADDR=192.168.123.15
NETMASK=255.255.255.0
GATEWAY=192.168.123.1
DNS1=192.168.123.1
DNS2=114.114.114.114
DNS3=223.6.6.6
systemctl restart network

# 关闭SELinux
vim /etc/sysconfig/selinux
SELINUX=disabled
reboot
```

## 安装步骤

```shell
# 先决条件
su
yum update -y
systemctl disable firewalld
systemctl stop firewalld
systemctl disable NetworkManager
systemctl stop NetworkManager
systemctl restart network

# 软件安装
yum install -y centos-release-openstack-train
yum update -y
yum install -y openstack-packstack

# 软件启动
packstack --allinone

# 登录平台 http://192.168.123.15/dashboard
cat /root/keystonerc_admin

# 对象存储Dashboard实验
# 1.平台创建私有容器
# 2.平台创建共有容器
# 3.平台创建目录
# 4.平台目录上传文件
# 5.平台下载文件
# 6.平台点击链接下载文件
# 7.平台删除容器

# 对象存储Cli实验
# 加载环境变量
source ./keystonerc_admin
# 创建容器
openstack container create Swift
# 创建文件
echo hello > test.txt
# 上传文件到容器
openstack object create Swift test.txt
# 查看对象列表
openstack object list Swift
# 下载文件
openstack object save --file get.txt Swift test.txt
```

## OpenStack 其他问题

### leatherman 版本问题

```shell
yum downgrade leatherman
```
