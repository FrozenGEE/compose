# 【各nas portainer 部署】
- 汉化版镜像：6053537/portainer-ce
- 官方镜像：portainer/portainer-ce

> 请见上说明根据自己实际情况修改，以下以中文版为例，如果9000端口冲突，就把冒号前面的9000修改掉

> 使用教程👇
- https://article.juejin.cn/post/7127050260710424589
- https://juejin.cn/post/7345379917821411347
- https://zhuanlan.zhihu.com/p/617947859

- 以下命令，请按照实际修改，均为以第一个存储池/volume1的共享文件夹/docker，作为docker的数据存放路径

- 注意：该中文版镜像已经停止更新维护，对于docker版本较高的，会不支持镜像的一些管理操作，这就推荐官方版本了，用习惯了英文真不难

| NAS | portainer-zh部署命令 |
| :----: | :---- | 
| Debian/Ubuntu | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/docker/portainer-zh:/data 6053537/portainer-ce:latest<br><br>路径仅供参考，按自己实际操作 |
| unRAID | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/user/appdata/portainer-zh:/data 6053537/portainer-ce:latest |
| TrueNAS | 待补充 |
| CASAOS/ZimaOS | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /DATA/AppData/portainer-zh:/data 6053537/portainer-ce:latest |
| 群晖 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce:latest |
| 威联通 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /share/container-station-data/portainer-zh:/data 6053537/portainer-ce:latest |
| 铁威马 | TOS5<br>docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /Volume1/User/docker/portainer-zh:/data 6053537/portainer-ce:latest<br><br>TOS6<br>docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /Volume1/docker/portainer-zh:/data 6053537/portainer-ce:latest |
| 万由 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/unas/data/docker/portainer-zh:/data 6053537/portainer-ce:latest |
| 华硕 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce:latest |
| 飞牛OS | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /vol1/1000/docker/portainer-zh:/data 6053537/portainer-ce:latest |
| 新绿联 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce:latest |
| 旧绿联 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/dm-0/.ugreen_nas/用户ID/docker/portainer-zh:/data 6053537/portainer-ce:latest<br><br>记得把"用户ID"替换掉 |
| 极空间 | docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /tmp/zfsv3/nvme$/手机号码+字母/data/docker/portainer-zh:/data 6053537/portainer-ce:latest<br><br>[2024-09-28更新] 现在arm也不会冲突重启了，命令行修改为自启动，记得把"/nvme$/手机号码+字母"替换掉 |

unRAID有插件支持但是交互体验个人觉得不好用，商店就有portainer现成模板，推荐直接安装部署即可，要用中文版把存储库名字替换一下就可以用汉化版即可

群晖，绿联，华硕，铁威马，飞牛OS，TrueNAS，OMV 官方的套件/应用/内置功能都支持compose编写，portainer可以不按照，直接用自带的

CasaOS同理，自带导入compose后转换为自己的模板，也可以导出

# 【portainer 食用方法】
:white_check_mark: 视频教程：
https://www.bilibili.com/video/BV1TCgNeLEtu

:white_check_mark: 图文教程：

- 1、使用文本编辑器先把自己的docker文件路径，存储池路径记录下来（见上说明）

- 2、下载模板，按照模板内的注释修改内容，请务必确认好配置单无误

- 3、这里拿汉化版作为演示便于理解，官方版一样的位置的，部署好portainer（见上）并打开，点击docker，左侧的堆栈（stack），右侧的新增（Add）

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-01-03.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-04.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-05.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-06.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-07.png)

- 4、命名stack的名字，把预先写好的compose内容复制到文本框中，点击部署，等待完成，如果有报错，看右上交报文提示，寻找网友帮助

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-08-10.png)

- 5、后续修改更新 & 更新镜像

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-11.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-12.png)

- 8、未映射卷 & 残留卷

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-13.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-14.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-15.png)

- 9、镜像导出 & 镜像导入 & 镜像删除

![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-16.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-17.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/00.Portainer/Portainer-18.png)
