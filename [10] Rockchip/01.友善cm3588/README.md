# 友善cm3588 OMV7 折腾笔记
[遇事不决问deepseek](https://chat.deepseek.com)

[友善cm3588官方资料](https://wiki.friendlyelec.com/wiki/index.php/CM3588/zh)

[友善cm3588官方固件下载地址](https://download.friendlyelec.com/CM3588)

友善cm3588默认自带```admin```和```pi```的账号，```admin```是用来WebUI登录的，记得第一时间修改，密码是```openmediavault```；而```pi```则是用于smb登录的，直接在WebUI上删除即可

### ⭐切换root用户 (提权)
```
sudo -i
# 再次输入账号密码
# 后续命令默认root执行
```
### ⭐修改密码
```
passwd root
# 连续输入两次新密码
password admin
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
apt install -y vim nano samba nfs-kernel-server rclone git pip clinfo neofetch btop ncdu
# apt install -y 软件名1 软件名2 软件名3 软件名4 软件名5
```
3、安装deb软件包，使用 apt 会自动处理依赖问题，无需额外操作
```
apt install ./包名.deb
```
4、检查是否安装成功
```
5、dpkg -l | grep 包名.deb
```
6、卸载 .deb 包
```
apt remove 包名.deb
```
7、完全移除软件包及其配置文件
```
apt purge 包名.deb
```
### ⭐安装 x-cmd (个人自用)
```
eval "$(wget -O- https://get.x-cmd.com)"
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
### ⭐格式化清空硬盘
1、打开OMV7的WebUI，登录账号，输入密码

2、点击左侧的 存储器 → 磁盘

3、选中设备，点击 擦除 → 勾选确认 → 是 → 快速，等待完成即可

### ⭐创建文件系统/阵列
1、打开```OMV7的WebUI```，登录账号```admin```，输入密码```openmediavault```(根据实际情况填写，此乃初始默认密码，记得及时修改)

此处缺图

2、点击左侧的```存储器```→```文件系统```，然后点击```挂载现有文件系统```或者```创建并挂载文件系统```
* 挂载现有文件系统：这是对现有已经存在存储分区所用的
* 创建并挂载文件系统：这是对于空盘所用的
* 演示使用挂载现有文件系统，请根据实际操作如果在役系统需要修改，最好把smb共享，webdav共享，docker等服务都停止

此处缺图

3、选中```已有的文件系统```，点击```保存```，此时会出现```黄色横幅提示：“待应用的配置更改 您必须应用配置变更才会使他们生效。”```，点```√```确认保存
* 这是OMV的二次确认，保存后才会将修改的内容生效，后续不再重新赘述

此处缺图

### ⭐修改OMV阵列存储池挂载路径
1、打开ssh，此处用xshell和xftp作为演示，添加ssh登录信息，账号密码均为```pi```(根据实际情况填写，此乃友善cm3588初始账号pi默认密码，如有其他账号则使用其他账号)

2、先 ```sudo -i``` 提权，然后再次输入pi的密码```pi```

3、输入```nano /etc/fstab```，将```/srv/dev-disk-by-uuid-2692a155-0242-4c73-b968-159e0656471c```修改为```/srv/sd-card```，内容自定义，接着退出保存 (按 Ctrl+X，回车键，Y)

此处缺图

4、输入```nano /etc/openmedaivault/config.yaml```，找到下图内容，修改好，退出保存(按 Ctrl+X，回车键，Y)，
```
/srv/dev/disk-by-uuid/2692a155-0242-4c73-b968-159e0656471c
修改为
/dev/mmcblk0
# 此处可在 存储池 —— 文件系统 中的 设备 可以查看到

/srv/dev-disk-by-uuid-2692a155-0242-4c73-b968-159e0656471c
修改为
/srv/sdcard
```
此处缺图

5、WebUI刷新即可
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

OMV在WebUI上安装compose插件，插件安装后配置一下即可安装成功

此处缺图

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
| docker rm 容器ID或容器名 | 删除某个容器 | docker tag 旧镜像名 新镜像名 | 修改镜像名字<br>注意是完整的docker镜像名字 |
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
| dpanel<br>portainer | docker管理器 | ddns-go | 域名解析 | lucky | 域名解析、反代、SSL<br>webdav、ftp、内置filebrower |
| alist| 网盘挂载<br>webdav | sun-panel | 导航页 | mt-photos<br>immich | 相册 |
| ttyd-bridge | 网页端ssh | tailscale<br>zerotier<br>netbird | 异地组网 | wg-easy<br>wireguard<br>openvpn | 自建异地组网 |
| syncthing | 多端同步 | v2raya | 魔法 | frps / frpc | 内网穿透服务 |
| filebrower | 文件管理器 | home-assistant | 家庭自动化平台 | rustdesk | 远程桌面服务 |
| watchtower | 自动更新容器 | dockercopilot | 一键更新容器 | jellyfin | 媒体库 |
| ani-rss | 追番神器 | radarr<br>sonarr<br>jackett<br>prowlarr | 媒体库自动化工具 | cookiecloud | cookie云备份 |
| qbittorrent<br>transmission | 种子下载器 | aria2 | 通用下载器 | clouddrive2 | 挂载网盘到本地 |
| lsky | 图床 | lobe-chat<br>chatgpt-next | ai助手 | ollama | 本地运行大型语言模型框架 |
| bili-sync-rs | B站收藏夹下载 | siyuan-note | 思源笔记 | vaultwarden | 密码库 |
| vocechat | 聊天室 | synctv | 和朋友一起看视频 | smokeping | 网络性能监控工具 |
| admin<br>mariadb<br>postgresql<br>redis| 数据库套装 | minio | 对象存储 | kms-server | 微软激活器 |


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
