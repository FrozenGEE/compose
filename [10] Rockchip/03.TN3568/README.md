# TN3568 armbian折腾笔记
```
硬件配置
✨ CPU：瑞芯微 rk3568 4核(Cortex-A55*4)
✨ RAM：4/8G
✨ ROM：eMMC(8G/16G/32G/64G), SATA *1
✨ 数据接口：USB2.0 *1
✨ 网络接口：1000G 网口 *1
```
[遇事不决问deepseek](https://chat.deepseek.com)

[armbian系统入门](https://github.com/FrozenGEE/compose/blob/main/%5B10%5D%20Rockchip/Armbian.md)

### ⭐SATA挂载到本地/mnt中
用于单盘NAS挂载存储介质到本地，docker容器的配置文件夹目录均存储在此处，统一路径，此处以使用 SSD 进行SATA挂载为例

1、识别设备
```
lsblk
fdisk -l
# 得知 SATA 为/dev/sda
```
2、确认分区情况
```
lsblk | grep sda
# 得知SATA分区情况
```
3、删除现有分区 (如果有的话)
```
fdisk /dev/sda
# 在fdisk命令行界面中，执行以下操作：
# 输入 p 查看现有分区
# 输入 d 删除分区，根据提示选择要删除的分区编号
# 重复删除所有分区
# 输入 w 保存更改并退出
```
4、新建分区
```
fdisk /dev/sda
# 在fdisk命令行界面中，执行以下操作：
# 输入 n 创建新分区
# 选择 1 创建主分区
# 按回车键，接受默认的起始扇区
# 按回车键，接受默认的结束扇区(使用全部存储空间)
# 输入 w 保存更改并退出
```
5、格式化分区
```
mkfs.ext4 /dev/sda1
# 以上为格式化分区为ext4

mkfs.btrfs /dev/sda1
# 以上为格式化分区为Btrfs
```
6、检查分区大小
```
parted /dev/sda print
```
7、创建挂载点
```
mkdir /mnt/ssd
# 将挂载到 /mnt/ssd 路径
```
8、临时挂载
```
mount /dev/sda1 /mnt/ssd
```
9、验证挂载状态
```
df -h | grep ssd
```
10、获取文件系统UUID
```
blkid /dev/sda1 | awk -F'UUID="' '{print $2}' | awk -F'"' '{print $1}'
# SSD 为 8842afa5-57db-4859-95a9-57ef49b3d059
```
11、修改 /etc/fstab 设置开机自动挂载
```
nano /etc/fstab
# 文本末端添加以下内容，然后推出并保存(按ctrl+X，Y，回车键)
UUID=8842afa5-57db-4859-95a9-57ef49b3d059 /mnt/ssd ext4 defaults 0 0
# 开机自动挂载SSD
```
11、挂载并验证
```
mount -a
# 测试自动挂载，若无报错即配置成功
df -h | grep ssd
# 验证挂载状态
```
12、如果遇到以下报错内容，则执行 systemctl daemon-reload 一次
```
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```
### ⭐开启WebDAV

### ⭐使用WebDAV挂载其他服务器
1、安装软件包
```
apt update
apt install davfs2 -y
```
2、创建挂载点目录，例如 /mnt/emmc/webdav
```
mkdir /mnt/emmc/webdav
```
3、配置 WebDAV 凭据 (可选)

编辑 /etc/davfs2/secrets，添加 WebDAV 服务器的用户名和密码
```
nano /etc/davfs2/secrets
```
添加如下内容 (替换 WEBDAV_URL、USERNAME、PASSWORD)，然后保存并退出
```
https://example.com/webdav  USERNAME  PASSWORD
```
4、挂载 WebDAV
```
mount -t davfs https://example.com/webdav /mnt/webdav
```
如果未在 secrets 文件配置密码，会提示输入用户名和密码

5、验证挂载
```
df -h | grep webdav
ls /mnt/emmc/webdav
```
6、开机自动挂载
```
nano /etc/fstab
```
7、添加以下内容
```
https://example.com/webdav  /mnt/emmc/webdav  davfs  user,rw,noauto  0  0
```
8、挂载
```
mount -a
```
