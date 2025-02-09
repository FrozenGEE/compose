# 【更新日志-2025-02-10】
- 整理一下README内容，将大部分内容收纳到[NAS模板文档须知](https://github.com/FrozenGEE/compose?tab=readme-ov-file#nas%E6%A8%A1%E6%9D%BF%E6%96%87%E6%A1%A3%E9%A1%BB%E7%9F%A5)
- 新增[rockchip 折腾日记](https://github.com/FrozenGEE/compose/tree/main/%5B10%5D%20Rockchip)
- [电视盒子(armbain)](https://github.com/ophub/amlogic-s9xxx-armbian)可以参考[黑豹PatherX2(RK3566)](https://github.com/FrozenGEE/compose/tree/main/%5B10%5D%20Rockchip/03.%E9%BB%91%E8%B1%B9PatherX2)的模板，建议
- OMV或多盘位ARM-NAS可以参考[友善CM3588(RK3588)](https://github.com/FrozenGEE/compose/tree/main/%5B10%5D%20Rockchip/01.%E5%8F%8B%E5%96%84cm3588)的模板
- 直接armbian手搓NAS？很好，[香橙派5Plus(RK3588)](https://github.com/FrozenGEE/compose/tree/main/%5B10%5D%20Rockchip/01.%E5%8F%8B%E5%96%84cm3588)拿去参考
- 继续挖坑不填的dpanel

[历史更新内容](https://github.com/FrozenGEE/compose/blob/main/WHAT'S_OLD.md)

# 【一堆肺话】
- 分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好
- 本人模板强烈建议使用portainer进行编写，其他工具自己执衫，其实大同小于的，不用担心太难
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

7、[各nas dpanel 部署教程及食用方法](https://github.com/FrozenGEE/compose/blob/main/dpanel.md)(还没写...挖坑)
