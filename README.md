# 【更新日志-2025-05-28】
- 修正更新```v2rayA```的模板，统一都放到了[[06]特殊版本/46.魔法](https://github.com/FrozenGEE/compose/tree/main/%5B06%5D%20%E7%89%B9%E6%AE%8A%E7%89%88%E6%9C%AC/46.%E9%AD%94%E6%B3%95/46-03.v2raya-%E6%98%93%E7%94%A8%E8%80%8C%E5%BC%BA%E5%A4%A7%E3%80%81%E8%B7%A8%E5%B9%B3%E5%8F%B0%E7%9A%84V2Ray%E5%AE%A2%E6%88%B7%E7%AB%AF)里面
- ```media-aio```模板中```v2rayA```的内容，均统一使用不支持全局代理的模板，如果有需要使用全局代理的，自行修改
- ```飞牛OS 0.9.2```最新版本实测docker compose使用```unless-stopped```已经可以重启docker后容器自启动，后续的模板将统一使用
- 新增一份基于nastool的自动化媒体库一条路模板，暂时用着还可以，以BT为主
- 将绿联从群晖模板中分离，compose模板以使用相对路径为主，默认都支持X86，ARM是否支持可以看注释，如果是针对ARM的镜像会特别标注，特指MT相册和Jellyfin媒体库
- DH4300Plus (RK3588C ARM64v8) 大部分日常使用的docker镜像都支持，会特别标记 (还没写进去)
- 后续不再带华硕NAS，铁威马TOS5玩了，删了，减少工作量
- 修正部分compose错误的内容

[历史更新内容](https://github.com/FrozenGEE/compose/blob/main/WHAT'S_OLD.md)

# 【一堆肺话】
- 分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好
- 本人模板强烈建议使用dpanel或者portainer进行编写，其他工具自己执衫，其实大同小于的，不用担心太难
- 部分nas需要SSH才可以部署portainer，因为需要有权限访问docker的核心，但这一些nas的webui上无法选中路径，所以需要使用ssh命令进行部署，部署命令看文档后面
- 模板上很多变量可能很不简化（不小心故意的），还有大量注释，方便理解参数的用途，以及有用的信息，并且废话有点多.....
- 模板很多都是以外链接，host网络模式为主的，个人偏向于能自定义容器内部端口就自定义，然后host网络模式
- 而外链接是指针对于数据库的调用，我看了官方教程compose之类的教程后而个人修改的，因为个人不喜欢创建太多的docker容器，数据库要是为每一个docker容器单独创建，就会产生大量的数据库容器
- 大量的容器部署了，对于除了unraid和casaos以外的不带webui管理面板的nas系统，倒是无妨，毕竟看不见为净，但是我尽量能外链接就外链接，所以有些带有数据库管理的docker容器需要自己先提前在数据库容器中创建好对应的子数据库和子账号密码，再去compose中设置好（注意看注释说明）
- 后续也会搞一份专门的内置关联，啊.....开局就挖坑了，GBF了，还有好多没写π_π(直到2025年还没填坑......)
- 并且暂时分享通用模板，后续分享个人认为合适的现成compose参数配置模板，直接搞上去即可使用，做法类似casaos商店那样一键部署（但还是要自己稍微检查修改）

# 【docker的一些小知识】
- docker的更新其实就是把当前部署好的docker容器先卸载了，然后使用相同的配置单，再去部署一个新的容器，如果你仔细观察可以发现，docker容器的ID每一次更新都会变化

- 所以利用这个特性，只要你做好路径映射，把必要的东西映射到nas的实际路径上，如果你的nas要转移，docker部署的容器，你只要把映射出来的东西都打包到新的nas上，按照原来的配置单，稍微修改对应实际情况，就可以完美恢复（portainer可以在左侧“volume”/“卷”中看到哪些没有映射，并且可以删除残留卷）
- 这也是我个人用群晖，比起套件，更加优先用docker的原因，而且我也很推荐这么做

# 【NAS模板文档须知】
1、[NAS的路径说明](https://github.com/FrozenGEE/compose/blob/main/volumes.md)

2、[NAS默认端口说明](https://github.com/FrozenGEE/compose/blob/main/ports.md)

3、[NAS第一个用户的权限说明](https://github.com/FrozenGEE/compose/blob/main/uid_gid.md)

<del>4、[各nas portainer 部署教程及食用方法](https://github.com/FrozenGEE/compose/blob/main/portainer.md)</del>

5、[logo图标的食用方法](https://github.com/FrozenGEE/compose/blob/main/logo.md)

6、[compose模板一些其他配置参数](https://github.com/FrozenGEE/compose/blob/main/compose.md)

7、[各nas dpanel 部署教程及食用方法](https://github.com/FrozenGEE/compose/blob/main/dpanel.md)


[![Star History Chart](https://api.star-history.com/svg?repos=FrozenGEE/compose&type=Date)](https://star-history.com/#FrozenGEE/compose&Date)
