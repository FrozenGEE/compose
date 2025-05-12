# v2Raya 内核情况说明
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
| 友善cm3588 | OMV7.7.7-1 (Sandworm) | 6.1.99 | iptables v1.8.9 (nf_tables) |  | ✅ | 
| TN3568 | Armbian OS 25.05.0 bookworm | 6.1.134-ophub | iptables v1.8.9 (nf_tables) |  | ✅ | 
