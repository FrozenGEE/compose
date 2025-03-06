#### 限制容器的cpu和物理内存大小，均可添加以下代码内容进行限制
```shell
    deploy:
      resources:
        limits:
          memory: 2046M
          cpus: '4'
          # 限制容器最多可以使用主机上2个CPU，物理内存最高占用为2g
```
- 书写限制的方式有很多，此处仅列举其一，更多的详细内容可见以下参考资料
- [Docker资源（CPU/内存/磁盘IO/GPU）限制与分配指南](https://developer.aliyun.com/article/1064221)
- [如何在 Docker 中限制CPU和内存的使用 ？](https://segmentfault.com/a/1190000045656750)
- [Docker容器内存限制详解：如何设置最大内存大小以优化Python应用性能](https://www.oryoy.com/news/docker-rong-qi-nei-cun-xian-zhi-xiang-jie-ru-he-she-zhi-zui-da-nei-cun-da-xiao-yi-you-hua-python-yin.html)

#### Docker macvlan设置，使容器拥有独立ip(以局域网为192.168.1.*，父路由器为192.168.1.1为例子)
- 如果需要让docker使用独立IP进行访问，需要先执行以下命令创建一个基于macvlan的docker网段，或者手动创建一个
- 适用于让 QB/TR/其他docker容器 具有一个独立IP地址，添加其IP至魔法网络的黑名单中，不让其走魔法网络，从而避免标记为盒子
- 暂未学会 macvlan与其他网络模式的容器通信
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
#   host:
#     external: true
#     name: br0
#   bridge:
#     external: true
#     name: br0
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
- [Docker 网络模型之 macvlan 详解，图解，实验完整](https://cloud.tencent.com/developer/article/1432601)

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
