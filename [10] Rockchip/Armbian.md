# Armbian
## 系统版本对比

| 版本名称         | 内核版本               | 支持周期               | 软件包版本               | 适用场景                                 |
|------------------|------------------------|------------------------|--------------------------|------------------------------------------|
| **Jammy**        | Linux 6.1 LTS或更新  | 5年 (LTS)  | Ubuntu 22.04 LTS       | 长期稳定场景<br>(服务器、家庭网关)            |
| **Noble**        | Linux 6.1 LTS或更新  | 5年 (LTS) | Ubuntu 22.04 LTS        |  与 Jammy 类似，为不同分支       |
| **Bullseye**     | Linux 5.10+ | 长期支持<br>(Debian社区)  | Debian 稳定版(较旧)    | 高稳定性需求<br>(工业控制、嵌入式设备)     |
| **Bookworm**     | Linux 6.1 LTS或更新   | 长期支持<br>(Debian社区)  | Debian 新稳定版(最新)  | 开发者测试、新硬件支持                   |
| **HassIoSupervisor** | 基于bookworm，依赖定制版本       | 社区维护<br>(周期不定)   | 高度定制<br>(Home Assistant 适配版) | 智能家居专用<br>(Home Assistant 适配版)    |

## 软件包差异
   - **Ubuntu LTS(Jammy/Noble)**：软件包较新且稳定
   - **Debian 稳定版(Bullseye)**：软件包旧但兼容性极佳
   - **Debian 测试版(Bookworm)**：软件包最新，适合开发者
> 详细支持列表参考：  
> - [Armbian 官方仓库](https://github.com/armbian)  
> - [Ophub 的 Amlogic S9XXX 项目](https://github.com/ophub/amlogic-s9xxx-armbian)
