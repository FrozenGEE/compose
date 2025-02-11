# RK3588 折腾笔记
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


