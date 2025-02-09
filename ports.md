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
