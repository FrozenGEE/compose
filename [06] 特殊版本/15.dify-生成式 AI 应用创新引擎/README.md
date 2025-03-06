# dify-生成式 AI 应用创新引擎 Docker Compose 部署教程
[官网](https://dify.ai/zh)

[官方文档](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/docker-compose)

## 前提条件
安装 Dify 之前, 请确保你的机器已满足最低安装要求：
- 支持 amd64 / arm64 设备
- CPU >= 2 Core
- RAM >= 4 GiB
- Docker 19.03 or later
- Docker Compose 1.28 or later
* 如果没有安装，按照以下步骤按照

## 安装必要软件
替换apt源为清华apt源
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
更新apt软件源，安装git
```
apt update && apt upgrade && apt install -y git vim nano
```
一键安装docker及docker-compose
```
COMPOSE_VERSION=`git ls-remote https://github.com/docker/compose | grep refs/tags | grep -oP "[0-9]+\.[0-9][0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1`
sh -c "curl -L https://github.com/docker/compose/releases/download/v${COMPOSE_VERSION}/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose"
chmod +x /usr/local/bin/docker-compose
```
设置自启动Docker
```
systemctl enable --now docker
```
配置国内镜像源
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
## 克隆 Dify 代码仓库
进入你docker容器路径映射存放目录，具体可见该[文档](https://github.com/FrozenGEE/compose/blob/main/volumes.md)说明，这里以```/mnt/docker```为例子
```
cd /mnt/docker
```
新建一个专门存放dify的目录
```
mkdir dify
cd dify
```
克隆 Dify 源代码至本地环境。
```
# 假设当前最新版本为 0.15.3
git clone https://github.com/langgenius/dify.git --branch 0.15.3
```
进入 Dify 源代码目录
```
cd dify
```
修改 Dify 源代码下的 docker 目录，因为启动 compose 时，会更新目录名称生成容器的名字
```
mv docker/ dify/
```
进入 Dify 源代码的 Docker 目录
```
cd dify
```
复制环境配置文件
```
cp .env.example .env
```
修改 docker-compose.yaml 文件内容
```
nano docker-compose.yaml
```
按 PgDn键 或者 方向键↓ 找到第590行，在最前方空白处输入 # 进行注释
```

```
按 回车键、方向键↑、输入以下内容
```
- 15580:80
```

启动 Docker 容器
```
```

```

```

```
