# 【更新日志-2025-01-04】
- 待办：找个时间，把portainer换成dpanel，dpanel上手很不错
- 飞牛OS docker容器使用unless-stopped无法在docker启动时自启动，修改为always即可

#### 限制容器的cpu和物理内存大小，均可添加以下代码内容进行限制
```shell
    deploy:
      resources:
        limits:
          memory: 2046M
          cpus: 2 
          # 限制容器最多可以使用主机上2个CPU，物理内存最高占用为2g
```
- 书写限制的方式有很多，此处仅列举其一，更多的详细内容可见以下参考资料
- [Docker资源（CPU/内存/磁盘IO/GPU）限制与分配指南](https://developer.aliyun.com/article/1064221)
- [如何在 Docker 中限制CPU和内存的使用 ？](https://segmentfault.com/a/1190000045656750)
- [Docker容器内存限制详解：如何设置最大内存大小以优化Python应用性能](https://www.oryoy.com/news/docker-rong-qi-nei-cun-xian-zhi-xiang-jie-ru-he-she-zhi-zui-da-nei-cun-da-xiao-yi-you-hua-python-yin.html)

#### Docker macvlan设置，使容器拥有独立ip(以局域网为192.168.1.*，父路由器为192.168.1.1为例子)
- 如果需要让docker使用独立IP进行访问，需要先执行以下命令创建一个基于macvlan的docker网段，或者手动创建一个
- 适用于让 QB/TR/其他docker容器 具有一个独立IP地址，添加其IP至魔法网络的黑名单中，不让其走魔法网络，从而避免标记为盒子
```shell
docker network create -d macvlan --gateway 192.168.1.1 --subnet 192.168.1.0/24 --ipv6 -o parent=eth0 br0
```
| 命令 | 含义 |
| :---- | :---- |
| -d macvlan | 指定网络驱动为macvlan |
| --gateway 192.168.1.1 | 指定设备网关，即你的路由器管理页面的地址<br>建议是不走科学环境的网关，如果你的网络比较复杂，请根据实际情况多调试修改 |
| --subnet 192.168.1.0/24 | 指定子网范围，/24代表掩码255.255.255.0，可自定义的ip地址在192.168.1.*内 |
| --ipv6 | 照抄即可，用于自动创建ipv6网络，会由父路由器分配下发ipv6地址 |
| -o parent=eth0 | 指定物理网卡，使用ifconfig查看，一般为eth0，eth1，eth2等 |
| br0 | 网络名称，可以自定义，创建容器时需要使用 |

##### 以lucky为例子，以下内容省略路径映射，仅作测试
- docker cli命令创建使用br0模板
```shell
docker run -d --name lucky-test --net=br0 --ip=192.168.1.233 docker.1panel.live/gdy666/lucky:latest
```
- docker compose创建使用br0示例，选用lucky来测试获取ipv4和ipv6地址是否为公网ip
```shell
services:
  lucky-test:
    image: docker.1panel.live/gdy666/lucky:latest
    container_name: lucky-test
    hostname: lucky-test
### 重点内容，删除原本的网络设置，替换为以下为网络设置内容
##############################
# 下方的 br0 为自定义网络的名字，请根据实际情况修改
# 注意：将会直接使用容器的端口进行访问，比如原本的80，443，3306，5244，8063，16601，某些容器可能需要在变量中自定义容器端口
    networks:
      br0:
        ipv4_address: 192.168.1.233
      # 设置一个没有占用的局域网ip地址

# 以下照抄，如果一张compose有多个容器部署，则需要放到compose内容最后面，并且写上其他容器的网络信息
networks:
    br0:
      external: true
      name: br0
# 仅供参考，请按实际情况添加
#    host:
#      external: true
#      name: br0
#  bridge:
#      external: true
#      name: br0
##############################
```
lucky正确获取到ipv4和ipv6地址
![image](https://github.com/FrozenGEE/compose/blob/main/.images/06.MAVLAN/macvlan-lucky.png)
使用[ipw.cn](https://ipw.cn) ping ipv6地址测试
![image](https://github.com/FrozenGEE/compose/blob/main/.images/06.MAVLAN/macvlan-ipw.png)


参考资料：
- ﻿[docker的macvlan网络不会搞？它其实很简单](https://post.smzdm.com/p/agq9mw53)
- [如何将容器运行到Docker Macvlan网络上](https://blog.laoyutang.cn/linux/docker-macvlan.html)
- [docker使用macvlan配置网络，使容器与宿主机在同一局域网，广播域内](https://zhuanlan.zhihu.com/p/669471518)
- [深入了解Macvlan技术：实现容器网络隔离与通信](https://fish-pro.github.io/2024/01/23/深入了解Macvlan技术-实现容器网络隔离与通信)

#### compose模板中自定义 DNS 服务器，可以是单个值或列表的多个值，添加以下代码其一内容即可
```shell
  dns: 8.8.8.8
```
```shell
  dns:
    - 8.8.8.8
    - 223.6.6.6
```

#### compose模板中使用 depends_on 设置依赖关系
- docker-compose up ：以依赖性顺序启动服务。在以下示例中，先启动 db 和 redis ，才会启动 web
- docker-compose up SERVICE ：自动包含 SERVICE 的依赖项。在以下示例中，docker-compose up web 还将创建并启动 db 和 redis
- docker-compose stop ：按依赖关系顺序停止服务。在以下示例中，web 在 db 和 redis 之前停止
```shell
version: "3.7"
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
```
- 来自于[docker compose 菜鸟教程](https://www.runoob.com/docker/docker-compose.html)

[历史更新内容](https://github.com/FrozenGEE/compose/blob/main/WHAT'S_OLD.md)

# 【一堆肺话】
- 分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好
- 本人模板强烈建议使用portainer进行编写，其他工具自己执衫，其实大同小于的，不用担心太难
- 部分nas需要SSH才可以部署portainer，因为需要有权限访问docker的核心，但这一些nas的webui上无法选中路径，所以需要使用ssh命令进行部署，部署命令看文档后面
- 模板上很多变量可能很不简化（不小心故意的），还有大量注释，方便理解参数的用途，以及有用的信息，并且废话有点多.....
- 模板很多都是以外链接，host网络模式为主的，个人偏向于能自定义容器内部端口就自定义，然后host网络模式
- 而外链接是指针对于数据库的调用，我看了官方教程compose之类的教程后而个人修改的，因为个人不喜欢创建太多的docker容器，数据库要是为每一个docker容器单独创建，就会产生大量的数据库容器
- 大量的容器部署了，对于除了unraid和casaos以外的不带webui管理面板的nas系统，倒是无妨，毕竟看不见为净，但是我尽量能外链接就外链接，所以有些带有数据库管理的docker容器需要自己先提前在数据库容器中创建好对应的子数据库和子账号密码，再去compose中设置好（注意看注释说明）
- 后续也会搞一份专门的内置关联，啊.....开局就挖坑了，GBF了，还有好多没写π_π
- 并且暂时分享通用模板，后续分享个人认为合适的现成compose参数配置模板，直接搞上去即可使用，做法类似casaos商店那样一键部署（但还是要自己稍微检查修改）

# 【docker的一些小知识】
- docker的更新其实就是把当前部署好的docker容器先卸载了，然后使用相同的配置单，再去部署一个新的容器，如果你仔细观察可以发现，docker容器的ID每一次更新都会变化

- 所以利用这个特性，只要你做好路径映射，把必要的东西映射到nas的实际路径上，如果你的nas要转移，docker部署的容器，你只要把映射出来的东西都打包到新的nas上，按照原来的配置单，稍微修改对应实际情况，就可以完美恢复（portainer可以在左侧“volume”/“卷”中看到哪些没有映射，并且可以删除残留卷）
- 这也是我个人用群晖，比起套件，更加优先用docker的原因，而且我也很推荐这么做

# 【NAS的路径说明】
- 模板上会有很多【这里替换为你的docker数据目录】这些文字，一般每一个docker镜像都会有自己配置数据文件夹存在，用于存放容器自己的配置文件，所以很建议集中存放，见上说明，以下内容仅供参考，你只要理解了，其实都能知道这是什么意思，部分路径待后续补充，本人遗忘了。
> docker容器配置文件目录是专门用来集中存放各个容器的配置文件等数据<br>例如emby的"/config"映射到/xxx/emby，ddnsgo的"/root"映射到/xxx/ddns-go，alist的"/opt/data/alist"映射到/xxx/alist等等<br>以下使用emby作为例子，以便于理解

> 共享文件夹需要在文件管理器或者控制面板等地方进行新建，是用于数据存放的目录，例如媒体库，相册，数据下载目录等<br>包括docker容器配置文件目录也是属于共享文件夹(以我个人模板而言)

>  *代表第几个存储池，根据实际情况来写，后续更新针对nas专用的适配模板将会默认都用/volume1，请根据实际情况修改

```shell
💡unRAID的docker容器配置文件目录存放路径
/mnt/user/appdata/emby

💡unRAID的数据目录存放路径
/mnt/user/共享文件夹
💡第二种写法属于是自哪一块硬盘上存储
/mnt/disk*/共享文件夹
💡第三种写法属于是在缓存盘上存储
/mnt/cache/共享文件夹

第二种和第三种写法都适合于docker配置文件存放路径，不展开细说，模板均使用第一种写法
```

```shell
💡群晖的docker容器配置文件目录存放路径
/volume*/docker/emby

💡群晖的数据目录存放路径开头
/volume*/共享文件夹
```

```shell
💡威联通的docker容器配置文件目录存放路径
/share/Container/emby

💡威联通的数据目录存放路径
/share/共享文件夹
💡另一种写法
/share/CACHEDEV*_DATA/共享文件夹

对于这两种写法，模板以第一种写法为主，因为创建好第一个存储池，系统会预设创建好部分文件夹

💡威联通初始化后会自动创建一些文件夹
/share/Multimeida # 多媒体
/share/Pulic      # 公共文件夹
```

```shell
💡TrueNAS的docker容器配置文件目录存放路径
/mnt/共享文件夹/docker/emby

💡TrueNAS的数据目录存放路径
/mnt/共享文件夹

TrueNAS以SCALE为准，基于debian的
```

```shell
💡铁威马TOS5的docker容器配置文件目录存放路径
/Volume*/User/docker/emby

💡铁威马TOS5的数据目录存放路径
/Volume*/User

💡铁威马TOS6的docker容器配置文件目录存放路径
/Volume*/docker/emby

💡铁威马TOS6的数据目录存放路径
/Volume*/共享文件夹
```

```shell
💡华硕(华芸)的docker容器配置文件存放路径
/volume*/docker/emby

💡华硕(华芸)的数据目录存放路径
/volume*/共享文件夹
```

```shell
💡CasaOS/ZimaOS的docker容器配置文件目录存放路径
/DATA/AppData/emby

💡CasaOS/ZimaOS的数据目录存放路径
/DATA/共享文件夹
```

```shell
💡fnOS的docker容器配置文件存放路径
/vol*/用户id/docker/emby

💡fnOS的数据存放路径
/vol*/用户id

用户id，详见下表，第一个用户为1000，创建第二个用户则是1001，第三个是1002，如此类推
第一个用户的第一个存储池的共享文件夹路径是/vol1/1000
第一个用户的第二个存储池的共享文件夹路径是/vol2/1000
第三个用户的第二个存储池的共享文件夹路径是/vol2/1002
类推......
```

```shell
💡万由的docker容器配置文件目录存放路径
/mnt/unas/data/docker/emby

💡万由的数据目录存放路径
/mnt/unas/data/共享文件夹
```

```shell
💡绿联新系统(基于debian)的docker容器配置文件目录存放路径
/volume*/docker/emby
💡绿联新系统(基于debian)的数据目录存放路径开头
/volume*/共享文件夹

💡绿联新系统(基于debian)的个人文件夹存放路径(系统默认)
/volume*/@home/xxx/yyy
另一个写法：/home/xxx/yyy
xxx为用户名，包括管理员和普通用户，yyy为在此之下所创建的文件夹，其中包括了每一个用户的相册文件夹“Photos”
```

```shell
💡绿联旧系统(基于op)的docker容器配置文件目录存放路径
/mnt/media_rw/存储池序列号/.ugreen_nas/用户ID/docker/aaa
💡另一种写法：/mnt/dm-*/.ugreen_nas/用户ID/docker/aaa

💡绿联旧系统(基于op)的数据目录存放路径
/mnt/media_rw/存储池序列号/.ugreen_nas/用户ID/文件夹名字/bbb
💡另一种写法：/mnt/dm-*/.ugreen_nas/用户ID/data/bbb

第一种写法是根据webui上创建docker容器后使用portainer查看所得知的，推荐这种
另一种写法中dm-* 中的*代表第几个存储池，从0开始算起，但实际你自己是怎么设置的请视情况修改

由于每个人的存储池序列号和用户ID不一样，因为无法写出现成模板，[全通用]的模板可以使用
没什么值得保留的就趁早上新系统吧
```

```shell
💡极空间系统的docker容器配置文件目录存放路径
/tmp/zfsv3/sata$/手机号码/data/docker/aaa
/tmp/zfsv3/nvme$/手机号码/data/docker/aaa
/tmp/zfsv3/sata$/手机号码+字母/data/docker/aaa
/tmp/zfsv3/nvme$/手机号码+字母/data/docker/aaa

💡极空间系统的数据目录存放路径开头
/tmp/zfsv3/sata$/手机号码/data/bbb
/tmp/zfsv3/nvme$/手机号码/data/bbb
/tmp/zfsv3/sata$/手机号码+字母/data/bbb
/tmp/zfsv3/nvme$/手机号码+字母/data/bbb

"nvme$"和"sata$" 根据自己实际情况修改，$为数字；"手机号码"为个人手机号码，到处都是手机号，真他妈恶心
如果你有第二台极空间，并且用同一个手机号绑定注册(强制联网激活机器)，则为需要在手机号码后添加上a-z的字母
例如：1688888888，1688888888a，1688888888b，1688888888c，这样类推，所以同一个手机号极限是多少？27个？

由于每个人的硬盘类别不一样，手机号为个人隐私，因为无法写出现成模板，[全通用]的模板可以使用
```

# 【NAS默认端口说明】
只列举WebUI，WebDAV，SSH，因为FTP，SFTP，rsync，SMB，TELNET这些都是一样，除非官方魔改，欢迎更多不同品牌的NAS来补充

💡Debian/Ubuntu/unRAID/CasaOS/ZimaOS/OMV/TrueNAS等众多linux系统的WebUI http和https端口为80和443，ssh端口为22

| NAS<br>tcp端口(http/https) | WebUI| WebDAV | SSH | 其他/备注 |
| :----: | :----: | :----: | :---- | :---- |
| unRAID | 80/443 | 无 | 22 |
| TrueNAS-SCALE | 80/443 | 砍了 | 22 |
| OMV | 80/443 | 无 | 22 |
| CasaOS/ZimaOS | 80/443 | 无 | 22 |
| 群晖 | 5000/5001 | 5005/5006 | 22 | [DSM 服务使用哪些网络端口？ - Synology 知识中心](https://kb.synology.cn/zh-cn/DSM/tutorial/What_network_ports_are_used_by_Synology_services) |
| 威联通 | 8080/443 | 5005/5006 | 22 | [QTS 5.0.x 服务端口](https://docs.qnap.com/operating-system/qts/5.0.x/zh-cn/qnap-服务端口-C25795F.html)、[QuTS-hero 服务端口](https://docs.qnap.com/operating-system/quts-hero/4.5.x/zh-cn/GUID-DC25795F-A720-40C2-9159-66514178E6F6.html) |
| 铁威马 | 8181 | 5005/5006 | 9222 |
| 万由 | 80/443 | 192.168.1.10/webdav | 22 | webdav的端口和webui一样，万由是通过 nas的ip地址/webdav 路径的形式就可以访问 |
| 华硕 | 8000/8001 | 9800/9802 | 22 | [ASUSTOR 的 NAS 的应用程序或服务使用了那些网络端口?](https://www.asustor.com/zh-cn/knowledge/detail/?id=6&group_id=601) |
| 飞牛OS | 5666/5667 | 5005/5006 | 22 | fnOS 从 V0.8.22 版本之后，默认端口修改为 HTTP 5666 和 HTTPS 5667 端口<br>在公测阶段飞牛仍继续占用 8000 和 8001 端口<br>允许用户通过 8000 和 8001 端口访问到飞牛系统<br>在正式版本之后将不再使用 8000 和 8001 端口<br>[如何修改飞牛系统的端口？](https://help.fnnas.com/articles/fnosV1/settings/port-customization.md) |
| 新绿联 | 9999 | 5005/5006 | 22 |
| 旧绿联 | 9999 | 5081 | 922 | ssh密码需要手机号验证码获取并且仅开启3天 |
| 极空间 | 5055/5056 | 5005/5006 | 可自定义<br>最低10000 | 客户端访问必须正代 5055和8050<br>文档同步 22000；挂载为磁盘 9001<br>自带的下载器 51413 (tcp及udp)|

# 【NAS第一个用户的权限说明】
通常来说，这个账号可以smb访问，但有的NAS安装过程或者初始化中可以不预设，例如OMV

或者unRAID，TruNAS的WebUI登录的账号是root账号，需要手动创建才有

很多厂商NAS初始化过程中都会要求设置的，这里指的是这个账号，一般都不允许也不建议使用 admin 这个账号

在ssh中可以通过命令 “id 用户名” 进行查询，以下表格以 cheems 作为示例

一般第二用户的 UID 是第一个用户的 UID+1，但也有特殊的，具体情况请实际查询，但其实只需要第一个用户即可（即管理员账号）

而 root 则是最高管理员账号，拥有最高权限，可以读写nas中一切东西

`uid=0(root) gid=0(root) groups=0(root)`

还有 nobody，是一个特殊的用户账户，它通常用于运行那些不需要特权的服务进程

`uid=99(nobody) gid=100(users) groups=100(users),98(nobody)`

欢迎更多不同品牌的NAS来补充

| NAS | ssh中执行 id cheems 获取到的内容 | 其他/备注 |
| :----: | :---- | :---- |
| Debian | uid=1000(cheems) gid=1000(cheems) groups=1000(cheems),24(cdrom),25(floppy),<br>27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev) |
| Ubuntu | 
| unRAID | uid=1000(cheems) gid=100(users) groups=100(users) |
| TrueNAS-SCALE | uid=950(truenas_admin) gid=950(truenas_admin) groups=950(truenas_admin),544(builtin_administrators<br><br>uid=3000(cheems) gid=3000(cheems) groups=950(cheems),545(builtin_users) | truenas_admin 为系统自动创建<br>cheems 为自建的第一个账号 |
| OMV | uid=998(admin) gid=100(users) groups=100(users),997(openmediavault-admin)<br><br>uid=1000(cheems) gid=1000(cheems) groups=1000(cheems),24(cdrom),<br>25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),109(netdev)| admin 为系统自动创建<br>cheems 为自建的第一个账号，可在WebUI上修改 |
| CASAOS/ZimaOS |
| 群晖 | uid=1026(cheems) gid=100(users) groups=100(users),101(administrators) |
| 威联通 | uid=1000(cheems) gid=100(everyone) groups=100(everyone),0(administrators) |
| 铁威马 | uid=0(cheems) gid=0(everyone) groups=0(everyone),3(admin),4(allusers) |
| 万由 | uid=1001(admin) gid=100(Administrators) groups=1001(Administrators),1002(everyone) | 初始化设置时，账号默认设置为admin，不可自定义 |
| 华硕 | uid=1000(cheems) gid=100(users) groups=100(users),999(administrators) |
| 飞牛OS | uid=1000(cheems) gid=1001(Users) groups=1001(Users),1000(Administrators) |
| 新绿联 | uid=1000(cheems) gid=10(admin) groups=10(admin),100(users),1000(user),133(ughomeusers) |
| 旧绿联 | 激活机器的账号仅仅只是一个拥有较高权限的普通账号<br><br>通过官方途径得到的ssh使用的账号是root账号 |
| 极空间 | uid=1004(手机号) gid=1005(手机号) groups=1005(手机号),27(sudo) | 真他妈恶心 |

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

# 【logo 食用方法】
- unRAID：先创建一个名为"LOGO"的共享文件夹，然后把图标的压缩包解压到目录内，路径为 /mnt/user/LOGO/LOGO.png 这样的格式即可，提供的compose均预设好，也可以选择使用网址访问
 
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-02.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-03.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/01.unRAID/unRAID-04.png)

- CasaOS：使用图床，然后将图片上传后，在logo的地址栏上输入logo的访问地址即可
  
![image](https://github.com/FrozenGEE/compose/blob/main/.images/02.CasaOS/CasaOS-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/02.CasaOS/CasaOS-02.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/02.CasaOS/CasaOS-03.png)

- Sun-Panel：先在nas中找个地方存放好logo图标，将"/app/conf/uploads"该路径映射，映射到logo文件夹上，具体请看compose的注释，然后在图标编辑的时候，输入"/uploads/LOGO.PNG"即可
  
![image](https://github.com/FrozenGEE/compose/blob/main/.images/04.Sun-Panel/Sun-Panel-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/.images/04.Sun-Panel/Sun-Panel-02.png)
