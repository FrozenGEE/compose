## 关于 开源/闭源 && 免费/付费
- 开源≠免费
- 闭源≠不安全

## 关于 付费项目
- 硬件转码是付费项目，刮削不是付费项目
- ```PLEX```和```EMBY```官方正版均有付费内购，```JELLYFIN```开源免费
- 但是不是强制付费，付费也要看是什么付费，别瞎几把说付费连什么付费都不知道
- 只是把NAS服务端当作海报墙，不需要服务端硬件转码，让客户端进行硬件解码播放的，付费不付费有不一样的吗？

## 关于 刮削
- 准备好魔法，对刮削有帮助
- 默认的刮削的元数据源头都是来自TMDB的，无论是MOVIEPILOT、NASTOOL、PLEX、EMBY、JELLYFIN，乃至VIDHUB、INFUSE用的trakt都是
- 所以别扯什么XXX软件刮削数据更好，你只要是同一个数据源，TMD都一样，有区别的就只有展现的元数据内容有那些，怎展现
- 比如绿联、EMBY、JELLYFIN支持使用logo图片作为名字展现、支持每一集的小标题(如果有数据)，但是PLEX和极空间不支持

## 关于 EMBY
- ```EMBY```模板上使用的是```linuxserver```的镜像，均支持```amd64```和```arm64```
- 如果使用开心版，将镜像改为```amilys/embyserver```，```arm64v8```设备使用```amilys/embyserver_arm64v8```

## 关于 JELLYFIN
- ```JELLYFIN```模板上使用的是```nyanmisaka```的镜像，默认tag为```latest```，仅支持```amd64```，如果是```arm64```则tag改为```latest-rockchip```
- ```arm64```用户如果你不需要硬件转码，推荐直接使用```EMBY```，只充当一个海报墙来使用，用第三方客户端硬件解码，NAS服务端则只是负责数据传输，不做硬件转码
- 要是你非要使用```arm64```设备，并且还要硬件转码，并且你的视频规格特别高，建议去买[绿联DH4300Plus](https://www.ugnas.com/products-detail/id-43.html)
- ```arm64```设备硬件转码高规格视频保底至少是```RK3588```，在[JELLYFIN的官方文档](https://jellyfin.org/docs/general/post-install/transcoding/hardware-acceleration/rockchip)中就有写原因

```
解码和编码 https://github.com/rockchip-linux/mpp

AVC / H.264 8-bit由于其出色的兼容性而仍然被广泛使用。所有支持 RKMPP 的 Rockchip SoC 都可以对其进行解码和编码
解码和编码 H.264 8-bit - 任何支持RKMPP的Rockchip SoC
解码 H.264 10-bit - 几乎所有 RK33xx 及更高版本的 Rockchip SoC

Rockchip 上的 HEVC 支持很复杂
解码 HEVC 8-bit - 几乎所有 RK33xx 及更高版本的 Rockchip SoC
编码 HEVC 8-bit - 几乎所有 RK35xx 及以上的 Rockchip SoC
解码 HEVC 10-bit - 几乎所有 RK33xx 及以上的 Rockchip SoC

Rockchip 在其最新的 SoC 中增加了对 AV1 加速的支持
解码 AV1 8/10-bit - 瑞芯微 RK3588/3588S SoC
编码 AV1 8/10-bit - 截至 RK3588 系列，没有支持 AV1 编码器的 Rockchip SoC

RK3588/3588S 最高支持 1080p@480fps 或 4k@120fps 转码
RK356x 具有编码器的分辨率限制，即 1080p@100fps。它无法满足实时 4k 编码的需求
不推荐使用 RK33xx 及更早版本，它们的编码器只有 H.264 1080p@30fps
由于缺乏测试设备，无法比较不同 Rockchip SoC 代之间的质量差异
一般来说，SoC越新，编码质量越好。从第一印象来看，RK3588 系列上的VPU可以很好地满足实时流媒体的质量要求

jellyfin官方docker镜像附带所有必要的用户模式 Rockchip MPP & RGA & OpenCL 驱动程序
您需要做的是将 VPU 的设备文件从主机传递给 Docker，并启用特权模式
没有可靠的方法可以读取 Rockchip SoC 上 VPU 的占用情况，但仍然可以通过读取其他引擎来验证这一点，例如 RGA (2D hwaccel blitter)
1、在 Jellyfin Web 客户端播放视频，通过设置较低的分辨率或比特率触发视频转码
2、使用命令检查RGA引擎的占用情况(需要root权限)：sudo watch -n 1 cat /sys/kernel/debug/rkrga/load

RK3588/3588S是目前（2024年）最推荐的Soc，较旧的芯片可能受支持
例如RK356×和RK33xx，它们对编码分辨率的支持相当有限，并且缺乏硬件HDR色调映射支持
```

## 关于 QB/TR
- 注意某些私人BT站点中的规矩，造成损失，概不负责


## MP 变量说明
- 个人是很喜欢直接在compose中就写好MP的认证站点信息，但是太长了，因此从中分离出来
1、 认证站点
```
##############################################
#### 认证站点 ####
      - AUTH_SITE=iyuu,agsvpt,audiences,discfan,freefarm,haidan,hddolby,hdfans,hhclub,icc2022,ptba,ptvicomo,wintersakura,xingtan,zmpt,sunny,ptcafe,ptzone,kufei,yemapt,hspt,xingyunge,cspt,tmpt,raingfh,gtkpw,ptlgs,hdbao,sewerpt,ptskit
      ## 只有通过后才能使用站点相关功能，支持配置多个认证站点，使用英文逗号，进行分隔，会依次执行认证操作，直到有一个站点认证成功
      ## UID为网站分配给你的数字ID，请在个人信息内查看，切勿泄露
      ## 密钥在控制面板中查看，切勿泄露
      ## 以下认证写好后记得把#注释去掉，这部分可以选择不写，MP已经支持在WebUI上进行认证
##############################################
      # - IYUU_SIGN=
      # iyuu的认证，输入IYUU的登录令牌即可，IYUU本身也是需要认证的
##############################################
      # - HHCLUB_USERNAME=
      # - HHCLUB_PASSKEY=
      # hhclub的用户名和密钥，注意这里是用户名
##############################################
      # - AUDIENCES_UID=
      # - AUDIENCES_PASSKEY=
      # audiences的用户UID和密钥
##############################################
      # - HDDOLBY_ID=
      # - HDDOLBY_PASSKEY=
      # hddolby的用户ID和密钥，注意这里是用户ID
##############################################
      # - ZMPT_UID=
      # - ZMPT_PASSKEY=
      # zmpt的用户UID和密钥
##############################################
      # - FREEFARM_UID=
      # - FREEFARM_PASSKEY=
      # freefarm的用户UID和密钥
##############################################
      # - HDFANS_UID=
      # - HDFANS_PASSKEY=
      # hdfans的用户UID和密钥
##############################################
      # - WINTERSAKURA_UID=
      # - WINTERSAKURA_PASSKEY=
      # wintersakura的用户UID和密钥
##############################################
      # - LEAVES_UID=
      # - LEAVES_PASSKEY=
      # leaves的用户UID和密钥
##############################################
      # - PTBA_UID=
      # - PTBA_PASSKEY=
      # ptba的用户UID和密钥
##############################################
      # - ICC2022_UID=
      # - ICC2022_PASSKEY=
      # icc2022的用户UID和密钥
##############################################
      # - XINGTAN_UID=
      # - XINGTAN_PASSKEY=
      # xingtan的用户UID和密钥
##############################################
      # - PTVICOMO_UID=
      # - PTVICOMO_PASSKEY=
      # ptvicomo的用户UID和密钥
##############################################
      # - AGSVPT_UID=
      # - AGSVPT_PASSKEY
      # agsvpt用户UID和密钥
##############################################
      # - HDKYL_UID=
      # - HDKYL_PASSKEY=
      # hdkyl的用户UID和密钥
##############################################
      # - QINGWA_UID=
      # - QINGWA_PASSKEY=
      # qingwa的用户UID和密钥
##############################################
      # - DISCFAN_UID=
      # - DISCFAN_PASSKEY=
      # discfan的用户UID和密钥
##############################################
      # - HAIDAN_ID=
      # - HAIDAN_PASSKEY=
      # haidan的用户ID和密钥，注意这里是用户ID
##############################################
      # - ROUSI_UID=
      # - ROUSI_PASSKEY=
      # rousi的用户UID和密钥
##############################################
      # - SUNNY_UID=
      # - SUNNY_PASSKEY=
      # sunny的用户UID和密钥
##############################################
      # - PTCAFE_UID=
      # - PTCAFE_PASSKEY=
      # ptcafe的用户UID和密钥
##############################################
      # - PTZONE_UID=
      # - PTZONE_PASSKEY=
      # ptzone的用户UID和密钥
##############################################
      # - KUFEI_UID=
      # - KUFEI_PASSKEY=
      # kufei的用户UID和密钥
##############################################
      # - YEMAPT_UID=
      # - YEMAPT_AUTH=
      # yemapt的用户UID和密钥
##############################################
      # - HSPT_UID=
      # - HSPT_AUTH=
      # hspt的用户UID和密钥
##############################################
      # - XINGYUNGE_UID=
      # - XINGYUNGE_PASSKEY=
      # xingyunge的用户UID和密钥
##############################################
      # - CSPT_UID=
      # - CSPT_PASSKEY=
      # cspt的用户UID和密钥
##############################################
      # - TMPT_UID=
      # - TMPT_PASSKEY=
      # tmpt的用户UID和密钥
##############################################
      # - RAINGFH_UID=
      # - RAINGFH_PASSKEY=
      # raingfh的用户UID和密钥
##############################################
      # - GTKPW_UID=
      # - GTKPW_PASSKEY=
      # gtkpw的用户UID和密钥
##############################################
      # - PTLGS_UID=
      # - PTLGS_PASSKEY=
      # ptlgs的用户UID和密钥
##############################################
      # - HDBAO_UID=
      # - HDBAO_PASSKEY=
      # hdbao的用户UID和密钥
##############################################
      # - SEWERPT_UID=
      # - SEWERPT_PASSKEY=
      # sewerpt的用户UID和密钥
##############################################
      # - PTSKIT_UID=
      # - PTSKIT_PASSKEY=
      # ptskit的用户UID和密钥
```
