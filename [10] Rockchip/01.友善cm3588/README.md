# 友善cm3588 OMV7 折腾笔记
[遇事不决问deepseek](https://chat.deepseek.com)

[友善cm3588官方资料](https://wiki.friendlyelec.com/wiki/index.php/CM3588/zh)

[友善cm3588官方固件下载地址](https://download.friendlyelec.com/CM3588)

友善cm3588默认自带```admin```和```pi```的账号，```admin```是用来WebUI登录的，记得第一时间修改，密码是```openmediavault```；而```pi```则是用于smb登录的，直接在WebUI上删除即可

### ⭐格式化清空硬盘
1. 打开OMV7的WebUI，登录账号，输入密码

2. 点击左侧的 存储器 → 磁盘

3. 选中设备，点击 擦除 → 勾选确认 → 是 → 快速，等待完成即可

### ⭐创建文件系统/阵列
1. 打开```OMV7的WebUI```，登录账号```admin```，输入密码```openmediavault```(根据实际情况填写，此乃初始默认密码，记得及时修改)
此处缺图
2. 点击左侧的```存储器```→```文件系统```，然后点击```挂载现有文件系统```或者```创建并挂载文件系统```
* 挂载现有文件系统：这是对现有已经存在存储分区所用的
* 创建并挂载文件系统：这是对于空盘所用的
* 演示使用挂载现有文件系统，请根据实际操作如果在役系统需要修改，最好把smb共享，webdav共享，docker等服务都停止
此处缺图
3. 选中```已有的文件系统```，点击```保存```，此时会出现```黄色横幅提示：“待应用的配置更改 您必须应用配置变更才会使他们生效。”```，点```√```确认保存
* 这是OMV的二次确认，保存后才会将修改的内容生效，后续不再重新赘述
此处缺图
### ⭐修改OMV阵列存储池挂载路径
1. 打开ssh，此处用xshell和xftp作为演示，添加ssh登录信息，账号密码均为```pi```(根据实际情况填写，此乃友善cm3588初始账号pi默认密码，如有其他账号则使用其他账号)
2. 先 ```sudo -i``` 提权，然后再次输入pi的密码```pi```
3. 输入```nano /etc/fstab```，将```/srv/dev-disk-by-uuid-2692a155-0242-4c73-b968-159e0656471c```修改为```/srv/sd-card```，内容自定义，接着退出保存 (按 Ctrl+X，回车键，Y)
4. 输入```nano /etc/openmediavault/config.xml```，找到下图内容，修改好，退出保存(按 Ctrl+X，回车键，Y)，
```
/srv/dev/disk-by-uuid/2692a155-0242-4c73-b968-159e0656471c
修改为
/dev/mmcblk0
# 此处可在 存储池 —— 文件系统 中的 设备 可以查看到

/srv/dev-disk-by-uuid-2692a155-0242-4c73-b968-159e0656471c
修改为
/srv/sdcard
```
5. 重启一次系统

### ⭐友善cm3588 docker部署推荐
| docker | 用途 | docker | 用途 | docker | 用途 |
| :---- | :---- | :---- | :---- | :---- | :---- |
| dpanel<br>portainer | docker管理器 | ddns-go | 域名解析 | lucky | 域名解析. 反代. SSL<br>webdav. ftp. 内置filebrower |
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
