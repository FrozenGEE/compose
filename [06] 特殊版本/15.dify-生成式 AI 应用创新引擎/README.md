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
- 需要安装git，没有安装或者不会用的，可以向B站某位同学学习，直接一个 DownloadZIP
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
curl -L "https://mirrors.tuna.tsinghua.edu.cn/docker-compose/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose
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
进入 Dify 源代码的 Docker 目录
```
cd docker/dify
```
复制环境配置文件
```
cp .env.example .env
```
修改 docker-compose.yaml 文件内容
```
nano docker-compose.yaml
```
按 PgDn键 或者 方向键↓ 找到第590行，如下内容 (请根据实际情况查找，官方可能有个更新，202-05-27时，此时是774行)
```
    ports:
      - '${EXPOSE_NGINX_PORT:-80}:${NGINX_PORT:-80}'
      - '${EXPOSE_NGINX_SSL_PORT:-443}:${NGINX_SSL_PORT:-443}'
```
修改为如下内容，然后保存(按 Ctrl+X，Y，回车键)，这是直接不用官方预设的参数，改为自己端口映射
```
    ports:
      # - '${EXPOSE_NGINX_PORT:-80}:${NGINX_PORT:-80}'
      # - '${EXPOSE_NGINX_SSL_PORT:-443}:${NGINX_SSL_PORT:-443}'
      - 15580:80
```
启动 Docker 容器部署
```
docker-compose up -d
```
运行命令后，你应该会看到类似以下的输出，显示所有容器的状态和端口映射
```
[+] Running 11/11
 ✔ Network docker_ssrf_proxy_network  Created                                                                 0.1s 
 ✔ Network docker_default             Created                                                                 0.0s 
 ✔ Container docker-redis-1           Started                                                                 2.4s 
 ✔ Container docker-ssrf_proxy-1      Started                                                                 2.8s 
 ✔ Container docker-sandbox-1         Started                                                                 2.7s 
 ✔ Container docker-web-1             Started                                                                 2.7s 
 ✔ Container docker-weaviate-1        Started                                                                 2.4s 
 ✔ Container docker-db-1              Started                                                                 2.7s 
 ✔ Container docker-api-1             Started                                                                 6.5s 
 ✔ Container docker-worker-1          Started                                                                 6.4s 
 ✔ Container docker-nginx-1           Started    
 ✔ Container dify-nginx-1           Started    
```
最后检查是否所有容器都正常运行，在这个输出中，你应该可以看到包括 3 个业务服务 api / worker / web，以及 6 个基础组件 weaviate / db / redis / nginx / ssrf_proxy / sandbox
```
docker-compose ps
```
## 更新 Dify
进入 dify 源代码的 docker 目录，按顺序执行以下命令：
```
cd dify/dify
docker-compose down
git pull origin main
docker-compose pull
docker-compose up -d
```
## 同步环境变量配置 (重要！)
- 如果 .env.example 文件有更新，请务必同步修改你本地的 .env 文件
- 检查 .env 文件中的所有配置项，确保它们与你的实际运行环境相匹配。你可能需要将 .env.example 中的新变量添加到 .env 文件中，并更新已更改的任何值
## 访问 Dify
浏览器打开：```http://你的服务器的IP:15580```，然后等待转圈圈，接着会提示你创建管理员账号，邮箱随便写，要记住，然后登录账号密码即可
