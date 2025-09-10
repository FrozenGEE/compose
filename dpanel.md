# 【各nas dpanel 部署】
- 官方网站：https://dpanel.cc
- 官方文档：https://dpanel.cc/#/zh-cn/install/docker
- 官方镜像：dpanel/dpanel:latest
- 官方镜像精简版：dpanel/dpanel:lite
> 在 lite 版中，不包含域名转发功能(80 443端口)。即容器内不会安装 nginx 及 acme.sh 等相关组件需要域名转发请借助外部工具
- 因为国内docker墙了的原因，对拉取造成影响，所以需要使用官方阿里云加速镜像地址进行拉取，如下
```
阿里云加速镜像地址
registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:latest
registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite
```

## 部署教程
- 以下ssh一键部署命令，请按照实际修改，均为以第一个存储池/volume1的共享文件夹/docker，作为docker的数据存放路径

- 根据自己的喜好选择，个人仅以docker管理器为主，偏向于使用lite版，因此使用精简版lite

| NAS | dpanel/dpanel:lte 部署命令 |
| :----: | :---- | 
| Debian/Ubuntu/OMV | docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite<br><br>路径仅供参考，按自己实际操作 |
| unRAID |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/user/appdata/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite |
| TrueNAS | 待补充 |
| CASAOS/ZimaOS |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /DATA/AppData/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite |
| 群晖&新绿联&华硕 |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite |
| 威联通 | docker run -d --name dpanel -p 8807:8080 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /share/Container/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite |
| 铁威马 | TOS5<br>docker run -d --name dpanel --network=host --name=dpanel --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /Volume1/User/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite<br><br>TOS6<br>docker run -d --name dpanel --network=host --name=dpanel --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /Volume1//User/docker/dpanel:/dpanel lite |
| 万由 |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/unas/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite |
| 飞牛OS |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /vol1/1000/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite |
| 旧绿联 |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/dm-0/.ugreen_nas/用户ID/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite<br><br>记得把"用户ID"替换掉 |
| 极空间 |docker run -d --name dpanel --network=host --restart=always -e APP_NAME=dpanel -e APP_SERVER_PORT=8807 -v /var/run/docker.sock:/var/run/docker.sock -v /tmp/zfsv3/硬盘接口+数字/手机号码+字母/data/docker/dpanel:/dpanel registry.cn-hangzhou.aliyuncs.com/dpanel/dpanel:lite<br><br>记得把"/硬盘接口+数字/手机号码+字母"替换成实际的 |

> unRAID有插件支持但是交互体验个人觉得不好用，建议unRAID直接自建模板进行部署dpanel，剩余的其他容器则使用dpanel进行部署
> 
> 群晖，新绿联，华硕，铁威马，飞牛OS，TrueNAS，OMV 官方的套件/应用/内置功能都支持compose编写，dpanel可以不安装，直接用自带的
> 
> CasaOS自带导入compose后转换为自己的模板，也可以导出，甚至dpanel中就可以导入CasaOS和1Panel商店中的模板，成为dpanel的现成应用商店
>
> 至于极空间，[狗屎一般的compose](https://www.bilibili.com/video/BV1V95EzFEfe)
