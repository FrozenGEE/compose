# 【前言】

分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好

本人模板强烈建议使用portainer进行编写，其他工具自己执衫（翻译：自己看着办）


# 部分nas需要SSH才可以部署portainer，因为需要有权限访问docker的核心，命令看文档最后


模板上很多变量可能很不简化（不小心故意的），还有大量注释，方便理解参数的用途，以及有用的信息，并且废话有点多......

模板很多都是以外链接，host网络模式为主的，个人偏向于能自定义容器内部端口就自定义，然后host；而外链接是指针对于数据库的调用，我看了官方教程compose之类的教程后而个人修改的，因为个人不喜欢创建太多的docker容器，数据库要是为每一个docker容器单独创建，就会产生大量的数据库容器，如果没有webui面板（比如unraid和casaos）倒是无妨，毕竟看不见为净，但是我尽量能外链接就外链接，所以有些带有数据库管理的docker容器需要自己先提前创建好，再去compose中设置好（注意看注释说明），后续也会搞一份专门的内置关联，啊.....开局就挖坑了，GBF了，还有好多没写π_π

并且暂时分享通用模板，后续分享个人认为合适的现成compose参数配置模板，直接搞上去即可使用，做法类似casaos商店那样一键部署（但还是要自己稍微检查修改）

#【docker的一些小知识】

docker的更新其实就是把当前部署好的docker容器先卸载了，然后使用相同的配置单，再去部署一个新的容器，如果你仔细观察可以发现，docker容器的ID每一次更新都会变化，所以利用这个特性，只要你做好路径映射，把必要的东西映射到nas的实际路径上，如果你的nas要转移，docker部署的容器，你只要把映射出来的东西都打包到新的nas上，按照原来的配置单，稍微修改对应实际情况，就可以完美恢复，这也是我个人用群晖，比起套件，更加优先用docker的原因，而且我也很推荐这么做

#【NAS的路径说明】

模板上会有很多【这里替换为你的docker数据目录】这些文字，一般每一个docker镜像都会有自己配置数据文件夹存在，用于存放容器自己的配置文件，所以很建议集中存放，见上说明，仅供参考，你只要理解了，其实都能知道这是什么意思，部分路径待后续补充，本人遗忘了。
aaa代表docker容器的配置文件目录；bbb代表某个共享文件夹
*代表第几个存储池，根据实际情况来写，适配模板将会默认都用/volume1，请根据实际情况修改

unraid的docker配置文件存放路径：/mnt/user/appdata/aaa
unraid的数据存放路径开头：/mnt/user/bbb
熟悉unraid的玩家会知道另一个绝对路径写法，这里不展开说明

群晖的docker配置文件存放路径：/volume*/docker/aaa
群晖的数据存放路径开头：/volume*/bbb

威联通的docker配置文件存放路径：
威联通的数据存放路径开头：

omv的docker配置文件存放路径：
omv的数据存放路径开头：

casaos的docker配置文件存放路径：/DATA/AppData/aaa
casaos的数据存放路径开头：/DATA/bbb

TrueNAS的docker配置文件存放路径：
TrueNAS的数据存放路径开头：

绿联旧系统(基于op)的docker配置文件存放路径：/mnt/dm-*/.ugreen_nas/用户ID/docker/aaa

绿联旧系统(基于op)的数据存放路径开头：/mnt/dm-*/.ugreen_nas/用户ID/data/

dm-* 中的*代表第几个存储池，从0开始算起，但实际你自己是怎么设置的请视情况修改

绿联新系统(基于debian)的docker配置文件存放路径：/volume*/docker/aaa
绿联旧系统(基于debian)的数据存放路径开头：/volume*/bbb

极空间系统的docker配置文件存放路径：/volume*/docker/aaa
极空间系统的数据存放路径开头：/volume*/bbb


# 【各nas portainer 部署】
汉化版镜像：6053537/portainer-ce
官方镜像：portainer/portainer-ce
请见上说明根据自己实际情况修改，以下以中文版为例，如果9000端口冲突，就把冒号前面的9000修改掉

使用教程：
https://article.juejin.cn/post/7127050260710424589
https://juejin.cn/post/7345379917821411347
https://zhuanlan.zhihu.com/p/617947859

unraid商店就有模板，推荐直接安装部署即可，要用中文版把存储库名字替换一下就可以用汉化版即可

群晖7.2开始自带compose编写，不用装，但你也可以安装套件或者用群晖compose/cli来安装，但请预先在/volume1/docker上创建好一个portainer-zh的文件夹，群晖不会自动创建不存在的文件夹
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce

威联通(待补充)


casaos，商店中就可以一键安装，但非汉化版
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /DATA/AppData/portainer-zh:/data 6053537/portainer-ce

debian/ubuntu/armbian(其实可以随便自定义路径都可以，这里写和casaos一样)
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /DATA/AppData/portainer-zh:/data 6053537/portainer-ce

绿联旧系统
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/dm-0/.ugreen_nas/用户ID/docker/portainer-zh:/data 6053537/portainer-ce

绿联新系统(和群晖一样，但不需要预先创建对应的文件夹)
docker run -d -p 9000:9000 --name=portainer-zh --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer-zh:/data 6053537/portainer-ce

极空间docker用的是魔改的portainer，官方没有开放ssh，但可以通过一些途径获取到，然而本人的Z2pro（rk3568）使用portainer会和极空间的魔改portainer冲突，x86网友表示不会，所以这里修改portainer的启动参数为不自启动
(待补充)
