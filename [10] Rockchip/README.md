# 香橙派5Plus/友善CM3588 折腾笔记
[遇事不决问deepseek](https://chat.deepseek.com)

[armbian系统入门](https://github.com/FrozenGEE/compose/blob/main/%5B10%5D%20Rockchip/Armbian.md)

[香橙派 5 Plus 折腾笔记 | 空桑](https://www.hqshi.cn/info/ops/orange-pi-5-plus)
## 一、常用命令
ubuntu/debian/armbian均通用
### ⭐确认身份
```
whoami
```
### ⭐切换root用户
```
sudo -i
# 后续命令默认root执行
```
### ⭐更新软件列表和软件源
```
apt update && apt upgrade -y
```
### ⭐安装常用软件(按需选择)
1、安装单个
```
apt install -y vim
# apt install -y 软件名
```
2、批量安装
```
apt install -y vim nano samba nfs-kernel-server rclone git pip clinfo
# apt install -y 软件名1 软件名2 软件名3 软件名4 软件名5
```
### ⭐查看命令
```
ls：列出当前目录下的文件和目录
ls -l：以长格式列出文件和目录，显示详细信息(权限、所有者、大小、修改时间等)
# ll 命令是 ls -l 的别名，如果 ll 不能使用，见下操作
ls -a：列出所有文件和目录，包括隐藏文件(以 . 开头的文件)
ls -la 或 ls -al：以长格式列出所有文件和目录，包括隐藏文件
ls -h：与 -l 一起使用，以人类可读的格式显示文件大小(如 KB、MB)
ls -R：递归列出子目录中的内容
```
### ⭐为 ll 设置别名
1. 编辑 Shell 配置文件

根据你使用的 Shell，编辑对应的配置文件
```
nano ~/.bashrc  # 如果使用 Bash
nano ~/.zshrc   # 如果使用 Zsh
```
2. 添加别名

在文件末尾添加以下内容
```
alias ll='ls -l'
```
3. 使配置生效

保存并退出编辑器(按ctrl+X，Y，回车键)，然后运行以下命令使配置生效
```
source ~/.bashrc  # 如果使用 Bash
source ~/.zshrc   # 如果使用 Zsh
```
### ⭐清屏
```
clear
```
### ⭐查看内核的命令
```
uname -srm
hostnamectl | grep -i kernel
cat /proc/version
# 以上三条均可
```
### ⭐用户管理
1、修改用户密码
```
passwd 用户名
```
2、查看用户权限
```
id 用户名
```
3、终止用户进程
```
pkill -KILL -u 用户名
```
4、 删除用户及关联文件，添加 -r 选项会同时删除主目录及相关文件(/home/用户名)
```
userdel 用户名
```
5、搜索属于该用户的文件，检查残留文件 (可选)
```
find / -user 用户名
```
6、创建用户并自动生成主目录
```
useradd -m 用户名
```
7、指定用户的登录Shell
```
useradd -m -s /bin/bash 用户名
```
8、自定义用户UID或家目录
```
useradd -m -u 1000 -d /home/用户名 用户名
```
9、将用户添加到附加组，如 sudo：usermod -aG sudo 用户名
```
usermod -aG 组名 用户名
```
10、重新指定用户的附加组列表 (需列出保留的所有组，移除目标组)
```
usermod -G "保留组列表" 用户名
```
### ⭐替换apt源为清华apt源
来源：https://www.cnblogs.com/lcxhk/p/14951334.html
```
echo "[Info] 正在备份默认apt源..."
cp /etc/apt/sources.list /etc/apt/sources.list.bak
echo "[Info] 正在替换apt源为清华apt源..."
echo deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse > /etc/apt/sources.list
echo deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse >> /etc/apt/sources.list
echo deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse >> /etc/apt/sources.list
echo deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse >> /etc/apt/sources.list
echo "[Info] 正在更新源..."
apt update
echo "[Info] 正在更新软件..."
apt upgrade -y
```
### ⭐docker 相关
1、docker 安装

Docker官方一键安装脚本，使用官方源安装(国内直接访问较慢)
```
curl -fsSL https://get.docker.com | bash
```
使用阿里源安装
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
使用中国区Azure源安装
```
curl -fsSL https://get.docker.com | bash -s docker --mirror AzureChinaCloud
```
一键安装最新版Docker Compose
```
COMPOSE_VERSION=`git ls-remote https://github.com/docker/compose | grep refs/tags | grep -oP "[0-9]+\.[0-9][0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1`
sh -c "curl -L https://github.com/docker/compose/releases/download/v${COMPOSE_VERSION}/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose"
chmod +x /usr/local/bin/docker-compose
```
2、设置自启动Docker
```
systemctl enable --now docker
```
3、配置国内镜像源

[国内 Docker 服务状态 & 镜像加速监控](http://status.kggzs.cn/status/docker)
```
#- 终端命令添加 Docker 镜像加速
#- 以部分镜像加速为例
#- 以下是一整条命令，一起复制到终端运行

# 清空 /etc/docker/daemon.json 文件，准备写入新内容
echo >/etc/docker/daemon.json

# 使用 cat 命令将以下内容写入 /etc/docker/daemon.json 文件
# 配置 Docker 镜像加速器，使用多个镜像源来提高镜像下载速度
cat >/etc/docker/daemon.json <<EOF
{
"registry-mirrors": [
"https://docker.1panel.live",
"https://k-docker.asia",
"https://docker.1ms.run"
]
}
EOF

# 重启 Docker 服务以使配置生效
systemctl restart docker
```
4、查看docker和compose版本
```
docker verion && docker compose version
```
5、docker 拉取镜像
格式：docker pull 镜像源/作者名/镜像名:标签
- lscr.io，docker.1ms.run 为镜像源，也可以叫镜像仓库，可以自建
- hslr/sun-panel 为作者名和镜像名，可以去[dockerhub](https://hub.docker.com/)上搜索，有的不在dockerhub仓库上，特别是ghcr.io开头的
- latest，beta 为镜像的标签，一般情况不写则会自动拉取latest，某些镜像的tag不存在latest，或者某些架构的tag不一样，具体去看镜像详细页面上的tag
```
docker pull lscr.io/linuxserver/qbittorrent:latest
docker pull docker.1ms.run/hslr/sun-panel:beta
```
6、docker 命令
| 命令 | 含义 |
| :---- | :---- |
| docker ps | 查看部署的docker容器 |
| docker start 容器ID或容器名 | 启动某个容器 |
| docker restart 容器ID或容器名 | 重启某个容器 |
| docker stop 容器ID或容器名 | 停止某个容器 |
| docker kill 容器ID或容器名 | 直接关闭容器 |
| docker rm 容器ID或容器名 | 删除某个容器 |
| docker tag 旧镜像名字 新镜像名 | 修改镜像名字<br>注意是完整的docker镜像名字 |
* stop和kill的主要区别：stop给与一定的关闭时间交由容器自己保存状态，kill直接关闭容器
### ⭐EMMC/TF/SATA/M2挂载到本地/mnt中
用于单盘NAS挂载存储介质到本地，docker容器的配置文件夹目录均存储在此处，统一路径，此处以香橙派5plus+官方debian系统+加装EMMC和M2 SSD为例

1、识别设备
```
lsblk
fdisk -l
# 得知 EMMC为/dev/mmcblk0，SSD为/dev/nvme0n1
```
2、确认分区情况
```
lsblk | grep mmcblk0
lsblk | grep nvme0n1
# 得知EMMC和SSD分区情况
```
3、删除现有分区 (如果有的话)
```
fdisk /dev/mmcblk0
fdisk /dev/nvme0n1
# 在fdisk命令行界面中，执行以下操作：
# 输入 p 查看现有分区
# 输入 d 删除分区，根据提示选择要删除的分区编号
# 重复删除所有分区
# 输入 w 保存更改并退出
```
4、新建分区
```
fdisk /dev/mmcblk0
fdisk /dev/nvme0n1
# 在fdisk命令行界面中，执行以下操作：
# 输入 n 创建新分区
# 选择 p 创建主分区
# 按回车键，接受默认的起始扇区
# 按回车键，接受默认的结束扇区(使用全部存储空间)
# 输入 w 保存更改并退出
```
5、格式化分区
```
mkfs.ext4 /dev/mmcblk0p1
mkfs.ext4 /dev/nvme0n1
# 以上为格式化分区为ext4

mkfs.btrfs /dev/mmcblk0p1
mkfs.btrfs /dev/nvme0n1
# 以上为格式化分区为Btrfs
```
6、检查分区大小
```
parted /dev/mmcblk0 print
parted /dev/nvme0n1 print
```
7、创建挂载点
```
mkdir /mnt/emmc
mkdir /mnt/ssd
# 将分别挂载到 /mnt 下的 /emmc 和 /ssd 路径
```
8、临时挂载
```
mount /dev/mmcblk0p1 /mnt/emmc
mount /dev/nvme0n1 /mnt/ssd
```
9、验证挂载状态
```
df -h | grep emmc
df -h | grep ssd
```
10、获取文件系统UUID
```
blkid /dev/mmcblk0p1 | awk -F'UUID="' '{print $2}' | awk -F'"' '{print $1}'
blkid /dev/nvme0n1 | awk -F'UUID="' '{print $2}' | awk -F'"' '{print $1}'
# EMMC 为 32e6dc02-4e27-4ad0-99c5-b4c0b97cf263
# SSD  为 e1e08765-8e5b-4a2d-957e-96a34855b674
```
11、修改 /etc/fstab 设置开机自动挂载
```
nano /etc/fstab
# 文本末端添加以下内容，然后推出并保存(按ctrl+X，Y，回车键)
UUID=32e6dc02-4e27-4ad0-99c5-b4c0b97cf263 /mnt/emmc ext4 defaults 0 0
# 开机自动挂载EMMC
UUID=e1e08765-8e5b-4a2d-957e-96a34855b674 /mnt/ssd ext4 defaults 0 0
# 开机自动挂载SSD
```
11、挂载并验证
```
mount -a
# 测试自动挂载，若无报错即配置成功
df -h | grep emmc
df -h | grep ssd
# 验证挂载状态
```
12、如果遇到以下报错内容，则执行 systemctl daemon-reload 一次
```
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```
### ⭐RAID阵列挂载到本地/mnt中
此处以友善CM3588安装OMV系统创建RAID阵列，将共享文件夹挂载到指定目录，因为OMV的共享文件夹路径是序列号，很长，使用docker compose不方便写绝对路径

### ⭐VPU
```
lsmod | grep -E "vpu|npu"
# 检查已加载的内核模块
dmesg | grep -iE "vpu|rkvdec|rkvenc"
# 通过内核日志查看与VPU相关的驱动加载信息，驱动名称如 rkvdec(解码)或 rkvenc(编码)会显示在日志中
cat /sys/class/vpu/vpu/version

apt update && sudo apt install -y clinfo && clinfo
# 用 clinfo 检查主机上的 OpenCL (GPU 固件)
docker exec -it jellyfin /usr/lib/jellyfin-ffmpeg/ffmpeg -v debug -init_hw_device rkmpp=rk -init_hw_device opencl=ocl@rk
# 要验证 OpenCL 运行时在 docker 容器内是否正常工作，您可以运行此命令，第一个 jellyfin 为容器名字
# watch -n 1 cat /sys/kernel/debug/rkrga/load
# 视频转码时，使用命令检查RGA引擎的占用情况
```
- 参考资料：[Rockchip VPU jellyfin硬件转码](https://jellyfin.org/docs/general/administration/hardware-acceleration/rockchip)
### ⭐NPU
查看NPU版本
```
cat /sys/kernel/debug/rknpu/version
```
加载 rknpu.ko 内核，仅适用于香橙派5Plus使用[kaylorchen 镜像文件](https://www.bilibili.com/video/BV1otFXeeEw8)，[内核源码](https://github.com/kaylorchen/linux-orangepi/tree/rknpu-0.9.8)，克隆的时候记得切分支
```
insmod /usr/lib/modules/5.10.160-rockchip-rk3588/kernel/drivers/rknpu/rknpu.ko
```
