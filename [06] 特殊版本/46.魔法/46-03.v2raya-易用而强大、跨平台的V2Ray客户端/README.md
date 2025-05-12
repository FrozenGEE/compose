# v2RayA
[官方docker文档](https://v2raya.org/docs/prologue/installation/docker/)

[环境变量和命令行参数](https://v2raya.org/docs/manual/variable-argument)

[群晖实现透明代理](https://v2raya.org/docs/advanced-application/synology-transparent-proxy)

[群晖系统缺失的一些iptables模块](https://github.com/sjtuross/syno-iptables)

### v2RayA 内核情况说明
ssh 中执行```iptables --version```得出以下结果
- 数据更新日期：2025-05-12

| 系统 / 设备 | 系统版本 | Linux内核 |  输出结果 | legacy 后端 | nft 后端 | 
| :----: | :----: | :----: | :----: | :----: | :----: |
| unRAID | 7.1.2 | 6.12.24-Unraid | iptables v1.8.11 (legacy) | ✅ |  | 
| 群晖ds918+ | 7.2.1-69057 Update7 | 4.4.302+ | iptables v1.8.3 (legacy) | ✅ |  | 
| 绿联dx4600 | UGOS Pro 1.4.0 | 6.1.27 | iptables v1.8.9 (nf_tables) |  | ✅ | 
| 绿联dxp2800 | UGOS Pro 1.4.0 | 6.1.27 | iptables v1.8.9 (nf_tables) |  | ✅ | 
| 极空间T2 | zos | 5.10.110 | iptables v1.8.7 (nf_tables) |  | ✅ | 
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
