[Debian Docker 安装 | 菜鸟教程](https://www.runoob.com/docker/debian-docker-install.html)
# Debian Docker 安装
Docker 支持以下的 64 位 Debian 版本：
* Debian Bookworm 12
* Debian Bullseye 11

支持的架构包括 x86_64（amd64）、armhf、arm64 和 ppc64le

### ⭐卸载旧版本
如果你之前安装过 Docker Engine 之前，你需要卸载旧版本，避免冲突：
```
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
### ⭐自动安装
1. 使用官方安装脚本自动安装
```
 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh
```
2. 使用阿里源自动安装
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
3. 使用中国区Azure源自动安装
```
curl -fsSL https://get.docker.com | bash -s docker --mirror AzureChinaCloud
```
### ⭐手动安装
1. 更新软件包
```
sudo apt update && sudo apt upgrade -y
```
2. 安装依赖包
安装一些需要的依赖包，这些包允许 apt 使用 HTTPS 协议来访问 Docker 仓库
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
3. 添加 Docker 官方 GPG 密钥
使用下面的命令来添加 Docker 官方的 GPG 密钥
```
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
4. 添加 Docker 仓库
添加 Docker 官方的 APT 软件源
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 更新
sudo apt-get update
```
5. 更新 APT 软件包缓存
添加仓库后，更新 APT 包索引
```
sudo apt update
```
确保你现在从 Docker 官方仓库安装 Docker 而不是 Debian 默认仓库
```
apt-cache policy docker-ce
```
你应该看到它指向```https://download.docker.com/```，确保这就是官方的 Docker 仓库

6. 安装 Docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
7. 启动并验证 Docker
启动 Docker 并设置为开机自启
```
sudo systemctl start docker
sudo systemctl enable docker
```
验证 Docker 是否安装成功
```
docker version
```
运行以下测试命令确保 Docker 正常工作
```
sudo docker run hello-world
```
### ⭐卸载 docker
1. 删除安装包
```
sudo apt-get purge docker-ce
```
2. 删除镜像、容器、配置文件等内容
```
sudo rm -rf /var/lib/docker
```
