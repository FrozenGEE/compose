# 香橙派5Plus 折腾笔记
[遇事不决问deepseek](https://chat.deepseek.com)

[armbian系统入门](https://github.com/FrozenGEE/compose/blob/main/%5B10%5D%20Rockchip/Armbian.md)

[香橙派 5 Plus 折腾笔记 | 空桑](https://www.hqshi.cn/info/ops/orange-pi-5-plus)
### ⭐修改root密码
```
passwd root
```
### ⭐删除自带用户 orangerpi 同时删除主目录及相关文件(/home/orangerpi)
```
pkill -KILL -u orangerpi && userdel -r orangerpi
```
### ⭐创建用户UID和家目录
```
useradd -m -u 1000 -d /home/rk3588 rk3588
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
### ⭐更新软件列表和软件源
```
apt update && apt upgrade -y
```
### ⭐EMMC/TF/M2挂载到本地/mnt中
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
### ⭐docker 相关
1、docker 安装

通过apt安装
```
apt install -y docker.io
```
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
docker version && docker compose version
```
5、docker 拉取镜像
格式：docker pull 镜像源/作者名/镜像名:标签
- lscr.io，docker.1ms.run，k-docker.asia 为镜像源，也可以叫镜像仓库，可以自建
- hslr/sun-panel 为作者名和镜像名，可以去[dockerhub](https://hub.docker.com/)上搜索，有的不在dockerhub仓库上，特别是ghcr.io开头的
- latest，beta 为镜像的标签，一般情况不写则会自动拉取latest，某些镜像的tag不存在latest，或者某些架构的tag不一样，具体去看镜像详细页面上的tag
```
docker pull lscr.io/linuxserver/qbittorrent:latest
docker pull k-docker.asia/hslr/sun-panel:beta
docker pull docker.1ms.run/dpanel/dpanel:lite
```
6、docker 命令
| 命令 | 含义 | 命令 | 含义 |
| :---- | :---- | :---- | :---- |
| docker ps | 查看部署的docker容器 | docker images ps | 查看本地镜像 |
| docker start 容器ID或容器名 | 启动某个容器 | docker stop 容器ID或容器名 | 停止某个容器 |
| docker restart 容器ID或容器名 | 重启某个容器 | docker kill 容器ID或容器名 | 直接关闭容器 |
| docker rm 容器ID或容器名 | 删除某个容器 | docker tag 旧镜像名字 新镜像名 | 修改镜像名字<br>注意是完整的docker镜像名字 |
* stop和kill的主要区别：stop给与一定的关闭时间交由容器自己保存状态，kill直接关闭容器
### ⭐部署 dpanel —— docker管理器
```
sudo docker run -it -d --name dpanel --restart=always -p 8807:8080 -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/emmc/docker/dpanel:/dpanel dpanel/dpanel:lite
```
### ⭐dpanel 替换加速源
```
https://k-docker.asia
https://io.k-docker.asia
https://docker.1panel.top
https://docker.1ms.run
https://docker.ketches.cn
https://docker.amingg.com
https://spp-docker.asia
https://lq-docker.asia
https://hj-docker.asia
https://docker.1panel.live
https://dockerproxy.1panel.live
https://proxy.1panel.live
```
### ⭐香橙派5Plus docker部署推荐
| docker | 用途 | docker | 用途 | docker | 用途 |
| :---- | :---- | :---- | :---- | :---- | :---- |
| dpanel<br>portainer | docker管理器 | ddns-go | 域名解析 | lucky | 域名解析、反代、SSL |
| alist| 网盘挂载 | sun-panel | 导航页 | mt-photos<br>immich | 相册 |
| ttyd-bridge | 网页端ssh | tailscale<br>zerotier<br>netbird | 异地组网 | wg-easy<br>wireguard<br>openvpn | 自建异地组网 |
| syncthing | 多端同步 | v2raya | 魔法 | frps / frpc | 内网穿透服务 |
| filebrower | 文件管理器 | home-assistant | 家庭自动化平台 | rustdesk | 远程桌面服务 |
| watchtower | 自动更新容器 | dockercopilot | 一键更新容器 | jellyfin | 媒体库 |
| ani-rss | 追番神器 | moviepilot | 媒体库自动化工具 | cookiecloud | cookie云备份 |
| qbittorrent<br>transmission | 种子下载器 | aria2 | 通用下载器 | clouddrive2 | 挂载网盘到本地 |
| lsky | 图床 | lobe-chat<br>chatgpt-next | ai助手 | ollama | 本地运行大型语言模型框架 |
| bili-sync-rs | B站收藏夹下载 | siyuan-note | 思源笔记 | vaultwarden | 密码库 |
| vocechat | 聊天室 | synctv | 和朋友一起看视频 | smokeping | 网络性能监控工具 |
| admin<br>mariadb<br>postgresql<br>redis| 数据库套装 | minio | 对象存储 |   |   |


### ⭐香橙派5Plus jellyfin硬件转码
- 参考资料：[Rockchip VPU jellyfin硬件转码](https://jellyfin.org/docs/general/administration/hardware-acceleration/rockchip)

1、确保设备中存在 mpp、rga、dri、dma_heap，否则请将 BSP 内核升级到 5.10 LTS 及更高版本，运行此命令
```
$ ls -l /dev | grep -E "mpp|rga|dri|dma_heap"

drwxr-xr-x  2 root       root          80 Jan  1  1970 dma_heap
drwxr-xr-x  3 root       root         140 Jan 18 18:50 dri
crw-rw----  1 root       video   241,   0 Jan 18 18:50 mpp_service
crw-rw----  1 root       video    10, 122 Jan 18 18:50 rga
```
2、在主机上安装 ARM Mali OpenCL 

对于Ubuntu-Rockchip和Armbian上的6.1 LTS内核以及旧版5.10 LTS内核，请安装v1.9-1-2d267b0

https://github.com/tsukumijima/libmali-rockchip/releases/download/v1.9-1-2d267b0/libmali-valhall-g610-g13p0-gbm_1.9-1_arm64.deb

对于其他 SBC 供应商制作的发行版上的 6.1 LTS 内核，请安装 v1.9-1-55611b0

https://github.com/tsukumijima/libmali-rockchip/releases/download/v1.9-1-55611b0/libmali-valhall-g610-g13p0-gbm_1.9-1_arm64.deb

以下为安装命令，请根据实际情况修改
```
mkdir -p ~/tmp/libmali && cd ~/tmp/libmali
wget 'https://github.com/tsukumijima/libmali-rockchip/releases/download/v1.9-1-2d267b0/libmali-valhall-g610-g13p0-gbm_1.9-1_arm64.deb'
sudo dpkg -i ./libmali-valhall-g610-g13p0-gbm_1.9-1_arm64.deb
```
3、用 clinfo 检查主机上的 OpenCL (GPU 固件)
```
apt update && sudo apt install -y clinfo && clinfo
```
4、部署 jellyfin
详情见 [compose模板](https://github.com/FrozenGEE/compose/blob/main/%5B10%5D%20Rockchip/02.%E9%A6%99%E6%A9%99%E6%B4%BE5plus/jellyfin-%E7%A7%81%E4%BA%BA%E5%AA%92%E4%BD%93%E5%BA%93.yml)

5、要验证 OpenCL 运行时在 docker 容器内是否正常工作，您可以运行此命令，第一个 jellyfin 为容器名字
```
docker exec -it jellyfin /usr/lib/jellyfin-ffmpeg/ffmpeg -v debug -init_hw_device rkmpp=rk -init_hw_device opencl=ocl@rk
```
视频转码时，使用命令检查RGA引擎的占用情况
```
watch -n 1 cat /sys/kernel/debug/rkrga/load
```
检查 OpenCL 运行时状态
```
sudo /usr/lib/jellyfin-ffmpeg/ffmpeg -v debug -init_hw_device rkmpp=rk -init_hw_device opencl=ocl@rk

arm_release_ver: g13p0-01eac0, rk_so_ver: 10
[AVHWDeviceContext @ 0xaaaae8321360] 1 OpenCL platforms found.
[AVHWDeviceContext @ 0xaaaae8321360] 1 OpenCL devices found on platform "ARM Platform".
[AVHWDeviceContext @ 0xaaaae8321360] 0.0: ARM Platform / Mali-G610 r0p0
[AVHWDeviceContext @ 0xaaaae8321360] cl_arm_import_memory found as platform extension.
[AVHWDeviceContext @ 0xaaaae8321360] cl_khr_image2d_from_buffer found as platform extension.
[AVHWDeviceContext @ 0xaaaae8321360] DRM to OpenCL mapping on ARM function found (clImportMemoryARM).
...
```
通过内核日志查看与VPU相关的驱动加载信息，驱动名称如 rkvdec(解码)或 rkvenc(编码)会显示在日志中
```
cat /sys/class/vpu/vpu/version
```
也可以通过查看cpu占用情况确认是否硬件转码，如果cpu100%占用，则是cpu软件转码
