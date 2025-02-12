# RK3588 折腾笔记
[armbian系统入门](https://github.com/FrozenGEE/compose/blob/main/%5B10%5D%20Rockchip/Armbian.md)
## 一、常用命令
ubuntu/debian/armbian均通用
### ⭐切换root用户
```
sudo -i
```
### ⭐查看用户权限
```
id 用户名
```
### ⭐armbian 配置
```
armbian-config
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
### ⭐安装常用软件(按需选择)
```
# 安装单个
apt install samba -y
# 批量安装
apt install -y vim nano samba nfs-kernel-server rclone git pip
```
### ⭐查看命令
```
ls：列出当前目录下的文件和目录
ls -l：以长格式列出文件和目录，显示详细信息（权限、所有者、大小、修改时间等）
# ll 命令是 ls -l 的别名，如果 ll 不能使用，见下操作
ls -a：列出所有文件和目录，包括隐藏文件（以 . 开头的文件）
ls -la 或 ls -al：以长格式列出所有文件和目录，包括隐藏文件
ls -h：与 -l 一起使用，以人类可读的格式显示文件大小（如 KB、MB）
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

保存并退出编辑器，然后运行以下命令使配置生效
```
source ~/.bashrc  # 如果使用 Bash
source ~/.zshrc   # 如果使用 Zsh
```


### ⭐docker 相关
1、docker 安装

Docker官方一键安装脚本，使用官方源安装（国内直接访问较慢）
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
