# 【更新日志-2026-08-19】
- 清理了一下一些内容
- 某些经常翻新的项目，以及AI相关的项目，都放在了```[09] 单独合集```里面（=-=不过我本人不是经常折腾AI）
- 飞牛现再```unless-stopped```的问题也解决了，镜像也支持导出导入了，就差compose能一键更新，不停止情况下编辑内容，还有最重要的，compose文本编辑框无法全屏化问题解决了，就好用了
- dpanel的compose文本编辑框也可以最大化窗口了

[历史更新内容](https://github.com/FrozenGEE/compose/blob/main/WHAT'S_OLD.md)

# 【一堆肺话】
- 分享compose模板，方便新人，老手快速部署docker容器；因unraid模板而想写，但unraid模板只能用于unraid上，对于其他nas系统并不通用，而compose模板通用性很好
- 本人模板强烈建议使用dpanel或者portainer进行编写，其他工具自己执衫，其实大同小于的，不用担心太难
- 部分nas需要SSH才可以部署portainer，因为需要有权限访问docker的核心，但这一些nas的webui上无法选中路径，所以需要使用ssh命令进行部署，部署命令看文档后面
- 模板上很多变量可能很不简化（不小心故意的），还有大量注释，方便理解参数的用途，以及有用的信息，并且废话有点多.....
- 模板以host网络模式为主的，个人偏向于能自定义容器内部端口就自定义，然后host网络模式

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


[![Star History Chart](https://star-history.dera.page/svg?repos=FrozenGEE/compose&type=Date)](https://star-history.dera.page/#FrozenGEE/compose&Date)
