# v2RayA
[官方docker文档](https://v2raya.org/docs/prologue/installation/docker/)

[环境变量和命令行参数](https://v2raya.org/docs/manual/variable-argument)

[群晖实现透明代理](https://v2raya.org/docs/advanced-application/synology-transparent-proxy)

### v2RayA 内核情况说明
ssh 中执行```iptables --version```得出以下结果
- 数据更新日期：2025-05-12

| 系统 / 设备 | 系统版本 | Linux内核 |  输出结果 | legacy 后端 | nft 后端 | 
| :----: | :----: | :----: | :----: | :----: | :----: |
| unRAID | 7.1.2 | 6.12.24-Unraid | iptables v1.8.11 (legacy) | ✅ |  | 
| TrueNAS SCALE | 没装起来  | 不写  |   |   | 你猜 | 
| OMV7 | 没装起来 | 不写  |  |  | 猜的✅ | 
| 群晖ds918+ | 7.2.1-69057 Update7 | 4.4.302+ | iptables v1.8.3 (legacy) | ✅<br>特殊，见下 |  | 
| 威联通 | 刷黑群了 | 不写  |  |  | 猜的✅ | 
| 万由U-NAS | U-NAS 6.0  | 5.10.0-34-amd64 | iptables v1.8.7 (nf_tables) |  | 猜的✅ | 
| 铁威马TOS5 | 没设备 | 不写  |  |  | 猜的✅ | 
| 铁威马TOS6 | 没设备 | 不写  |  |  | 猜的✅ | 
| 绿联amd64 | UGOS Pro 1.4.0 | 6.1.27 | iptables v1.8.9 (nf_tables) |  | ✅ | 
| 绿联arm64 | UGOS Pro 1.4.2 | 6.1.84 | iptables v1.8.9 (nf_tables) |  | ✅ | 
| 飞牛OS | fnOS 0.9.2 | 6.12.24-trim  | iptables v1.8.9 (nf_tables) |  | ✅ | 
| 极空间T2 | zos 1.2.0 | 5.10.110 | iptables v1.8.7 (nf_tables) |  | ✅ | 
| 香橙派5plus | Ubuntu 22.04.5 LTS | 5.10.160 | iptables v1.8.7 (nf_tables) |  | ✅ | 
| 友善cm3588 | OMV 7.7.7-1 (Sandworm) | 6.1.99 | iptables v1.8.9 (nf_tables) |  | ✅ | 
| TN3568 | Armbian OS 25.05.0 bookworm | 6.1.134-ophub | iptables v1.8.9 (nf_tables) |  | ✅ | 

### 运行 v2rayA:
1. ```V2RAYA_V2RAY_BIN```的值应当是```/usr/local/bin/v2ray```或```/usr/local/bin/xray```，默认的核心是```xray```。
2. 如果你的宿主系统使用原生的```nftables```，那么就把```V2RAYA_NFTABLES_SUPPORT```设置为```on```。
3. 如果你的宿主系统使用```iptables```，那么可以通过```IPTABLES_MODE```变量来指定后端，将该变量的值设置为```nftables```将使用```nft```后端，设置为```legacy```将使用传统后端。
4. 以非 ROOT 权限运行 v2rayA 将无法使用部分功能，例如透明代理

以下是一个使用传统后端的示例:
```
docker run -d \
  --restart=always \
  --privileged \
  --network=host \
  --name v2raya \
  -e V2RAYA_LOG_FILE=/tmp/v2raya.log \
  -e V2RAYA_V2RAY_BIN=/usr/local/bin/v2ray \
  -e V2RAYA_NFTABLES_SUPPORT=off \
  -e IPTABLES_MODE=legacy \
  -v /lib/modules:/lib/modules:ro \
  -v /etc/resolv.conf:/etc/resolv.conf \
  -v /etc/v2raya:/etc/v2raya \
  mzz2017/v2raya
```
如果你使用 macOS 或其他不支持 host 模式的环境，在该情况下无法使用全局透明代理，或者你不希望使用全局透明代理，docker 命令会略有不同：
```
docker run -d \
  -p 2017:2017 \
  -p 20170-20172:20170-20172 \
  --restart=always \
  --name v2raya \
  -e V2RAYA_V2RAY_BIN=/usr/local/bin/v2ray \
  -e V2RAYA_LOG_FILE=/tmp/v2raya.log \
  -v /etc/v2raya:/etc/v2raya \
  mzz2017/v2raya
```

### 群晖v2rayA透明代理模式相关内容
[群晖系统缺失的一些iptables模块](https://github.com/sjtuross/syno-iptables)

- 来源：https://github.com/sjtuross/syno-iptables/wiki/v2rayA透明代理模式
  
#### 准备工作
1. 通过该页面[Synology Architectures](https://github.com/SynoCommunity/spksrc/wiki/Synology-and-SynoCommunity-Package-Architectures)查询架构，比如DS918+的架构为apollolake

2. 通过`uname -a`命令查询内核版本，比如DS918+ 7.0.1-42218系统内核为4.4.180+（结尾的加号代表自定义编译的4.X内核）

```text
Linux DSM7 4.4.180+ #42218 SMP Mon Oct 18 19:17:56 CST 2021 x86_64 GNU/Linux synology_apollolake_918+
```

3. 通过`iptables -V`命令查询iptables版本

```text
iptables v1.8.3 (legacy)
```

本仓库提供以下系统的预编译模块，经测试可以正常加载。
- 表格内容较旧，仅供参考

| arch       | kernel   | iptables version | system model | platform version |
| :--------- | :------- | :--------------- | :----------- | :--------------- |
| apollolake | 4.4.180+ | v1.8.3           | DS918+       | 7.0.1-42218      |
| apollolake | 4.4.59+  | v1.6.0           | DS918+       | 6.2.3-25426      |
| broadwell  | 3.10.105 | v1.6.0           | DS3617xs     | 6.2.3-25426      |
| bromolow   | 3.10.105 | v1.6.0           | DS3615xs     | 6.2.3-25426      |
| geminilake | 4.4.180+ | v1.8.3           | DS920+       | 7.1-42661        |
| geminilake | 4.4.302+ | v1.8.3           | DS220+       | 7.2-64570        |

#### 安装并尝试加载

上传相应的ko模块至`/lib/modules/`，上传相应的so模块至`/usr/lib/iptables/`，即可。

📝 文件名含ip6的用于原生支持Docker IPv6 NAT，其余的用于各种透明代理服务，可根据需要选择，全部安装也没事。

📝 ko内核模块和so用户模块一般需要同时安装。

⚠️ Windows和Mac用户注意，模块文件名是区分大小写的，大写的为标记模块，小写的为匹配模块，它们之间是相辅相成的，切勿彼此覆盖。

运行`sudo -i`之后再运行以下`insmod`命令尝试加载ko内核模块。由于模块互相有依赖性，需按一定顺序加载，有些是系统自带的模块。如果提示`File Exists`，说明已经加载，如果没有提示，说明加载成功。

<details>
<summary>4.X内核</summary>

```bash
insmod /lib/modules/nfnetlink.ko
insmod /lib/modules/ip_set.ko
insmod /lib/modules/ip_set_hash_ip.ko
insmod /lib/modules/xt_set.ko
insmod /lib/modules/ip_set_hash_net.ko
insmod /lib/modules/xt_mark.ko
insmod /lib/modules/xt_connmark.ko
insmod /lib/modules/xt_comment.ko
insmod /lib/modules/xt_TPROXY.ko
insmod /lib/modules/xt_socket.ko
insmod /lib/modules/iptable_mangle.ko
insmod /lib/modules/textsearch.ko
insmod /lib/modules/ts_bm.ko
insmod /lib/modules/xt_string.ko
```

```bash
insmod /lib/modules/nf_nat_ipv6.ko
insmod /lib/modules/nf_nat_masquerade_ipv6.ko
insmod /lib/modules/ip6t_MASQUERADE.ko
insmod /lib/modules/ip6table_nat.ko
insmod /lib/modules/ip6table_raw.ko
insmod /lib/modules/ip6table_mangle.ko
```

</details>
<details>
<summary>4.4.302 内核</summary>

```bash
insmod /lib/modules/nfnetlink.ko &> /dev/null
insmod /lib/modules/ip_set.ko &> /dev/null
insmod /lib/modules/ip_set_hash_ip.ko &> /dev/null
insmod /lib/modules/xt_set.ko &> /dev/null
insmod /lib/modules/ip_set_hash_net.ko &> /dev/null
insmod /lib/modules/xt_mark.ko &> /dev/null
insmod /lib/modules/xt_connmark.ko &> /dev/null
insmod /lib/modules/xt_comment.ko &> /dev/null

insmod /lib/modules/nf_conntrack_ipv6.ko &> /dev/null
insmod /lib/modules/nf_defrag_ipv6.ko &> /dev/null

insmod /lib/modules/xt_TPROXY.ko &> /dev/null
insmod /lib/modules/xt_socket.ko &> /dev/null
insmod /lib/modules/iptable_mangle.ko &> /dev/null
insmod /lib/modules/textsearch.ko &> /dev/null
insmod /lib/modules/ts_bm.ko &> /dev/null
insmod /lib/modules/xt_string.ko &> /dev/null

insmod /lib/modules/ip6_tables.ko &> /dev/null
insmod /lib/modules/nf_nat.ko &> /dev/null
insmod /lib/modules/nf_nat_ipv6.ko &> /dev/null
insmod /lib/modules/nf_nat_masquerade_ipv6.ko &> /dev/null
insmod /lib/modules/ip6t_MASQUERADE.ko &> /dev/null
insmod /lib/modules/ip6table_nat.ko &> /dev/null
insmod /lib/modules/ip6table_raw.ko &> /dev/null
insmod /lib/modules/ip6table_mangle.ko &> /dev/null
```
</details>

<details>
<summary>3.X内核</summary>

```bash
insmod /lib/modules/nfnetlink.ko
insmod /lib/modules/ip_set.ko
insmod /lib/modules/ip_set_hash_ip.ko
insmod /lib/modules/xt_set.ko
insmod /lib/modules/ip_set_hash_net.ko
insmod /lib/modules/xt_mark.ko
insmod /lib/modules/xt_connmark.ko
insmod /lib/modules/xt_comment.ko
insmod /lib/modules/nf_tproxy_core.ko
insmod /lib/modules/xt_TPROXY.ko
insmod /lib/modules/xt_socket.ko
insmod /lib/modules/iptable_mangle.ko
insmod /lib/modules/textsearch.ko
insmod /lib/modules/ts_bm.ko
insmod /lib/modules/xt_string.ko
```

```bash
insmod /lib/modules/nf_nat_ipv6.ko
insmod /lib/modules/ip6t_MASQUERADE.ko
insmod /lib/modules/ip6table_nat.ko
insmod /lib/modules/ip6table_raw.ko
insmod /lib/modules/ip6table_mangle.ko
```

</details>

📝 运行`lsmod`查看已加载的内核模块列表，或运行`dmesg | tail`查看加载失败的原因。

📝 不同内核版本netfilter编译生成的ko内核模块可能不完全一样。比如，`nf_tproxy_core.ko`模块只有3.X内核才会有，`nf_nat_masquerade_ipv6.ko`模块只有4.X内核才会有。

📝 为了群晖重启之后自动加载所需的内核模块，参考[通用模块加载的方法](https://github.com/sjtuross/syno-iptables/wiki/通用模块加载的方法)。

#### 加载缺失的模块并启动容器
在v2rayA启动时，为了确保所需的内核模块已经加载，可以覆盖默认的entrypoint为一个脚本，负责加载模块然后启动v2rayA，以下为docker run示例
```
docker run -d \
  --restart=always \
  --privileged \
  --network=host \
  --name v2raya \
  -e V2RAYA_ADDRESS=0.0.0.0:2017 \
  -e IPTABLES_MODE=legacy \
  -e V2RAYA_NFTABLES_SUPPORT=off \
  -v /lib/modules:/lib/modules \
  -v /etc/resolv.conf:/etc/resolv.conf \
  -v /volume1/docker/v2raya-config:/etc/v2raya \
  --entrypoint /etc/v2raya/bootstrap.sh \
  mzz2017/v2raya
```
⚠️ 请替换 /volume1/docker/v2raya-config 为你自己挂载的配置目录

以DS3617xs 6.2.3-25426为例，bootstrap.sh文件内容如下，同样存放于配置目录中
```
#!/bin/sh
insmod /lib/modules/nfnetlink.ko &> /dev/null
insmod /lib/modules/ip_set.ko &> /dev/null
insmod /lib/modules/ip_set_hash_ip.ko &> /dev/null
insmod /lib/modules/xt_set.ko &> /dev/null
insmod /lib/modules/ip_set_hash_net.ko &> /dev/null
insmod /lib/modules/xt_mark.ko &> /dev/null
insmod /lib/modules/xt_connmark.ko &> /dev/null
insmod /lib/modules/nf_tproxy_core.ko &> /dev/null
insmod /lib/modules/xt_TPROXY.ko &> /dev/null
insmod /lib/modules/iptable_mangle.ko &> /dev/null
insmod /lib/modules/textsearch.ko &> /dev/null
insmod /lib/modules/ts_bm.ko &> /dev/null
insmod /lib/modules/xt_string.ko &> /dev/null
v2raya
```
