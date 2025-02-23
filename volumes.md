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
/share/Multimeida        # 多媒体
/share/Pulic             # 公共文件夹
/share/Photo             # 相册
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

💡万由的主目录存放路径
/mnt/unas/data/Homes

💡万由的相册存放路径
/mnt/unas/data/Homes/用户名/uphoto
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

另一种书写格式，其中开头的data_n001为第一个存储池，如此类推
/data_n001/data/udata/real/手机号码/docker/aaa
/data_n001/data/udata/real/手机号码+字母/docker/aaa

💡极空间系统的数据目录存放路径开头
/tmp/zfsv3/sata$/手机号码/data/bbb
/tmp/zfsv3/nvme$/手机号码/data/bbb
/tmp/zfsv3/sata$/手机号码+字母/data/bbb
/tmp/zfsv3/nvme$/手机号码+字母/data/bbb

另一种书写格式，其中开头的data_n001为第一个存储池，如此类推
/data_n001/data/udata/real/手机号码/bbb
/data_n001/data/udata/real/手机号码+字母/bbb

"nvme$"和"sata$" 根据自己实际情况修改，$为数字；"手机号码"为个人手机号码，也可以选择使用另一种书写格式，但绕不开路径用手机号，真他妈恶心
如果你有第二台极空间，并且用同一个手机号绑定注册(强制联网激活机器)，则为需要在手机号码后添加上a-z的字母
例如：1688888888，1688888888a，1688888888b，1688888888c，这样类推，所以同一个手机号极限是多少？27个？

由于每个人的硬盘类别不一样，手机号为个人隐私，因为无法写出现成模板，[全通用]的模板可以使用
```
