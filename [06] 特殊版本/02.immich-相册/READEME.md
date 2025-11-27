## 官方资料
1. [官网](https://immich.app)
2. [官方github项目](https://github.com/immich-app/immich)
3. [官方compose教程](https://immich.app/docs/install/docker-compose)
4. [官方compose升级教程](https://docs.immich.app/install/upgrading)
5. [官方文档](https://docs.immich.app/)
6. [官方硬件转码说明](https://immich.app/docs/features/hardware-transcoding)
7. [官方变量说明](https://immich.app/docs/install/environment-variables)
8. [官方clip说明](https://immich.app/docs/features/command-line-interface)

## 第三方教程

### immich相册 部署教程
[部署教程](https://wiki.slarker.me/unraid/deploy_immich.html)

### AI模型 替换教程
1. [AI模型替换教程-图文](https://wiki.slarker.me/unraid/immich_ai_model.html)
2. [AI模型替换教程-视频](https://www.bilibili.com/read/cv33865669)
>  AI模型列表，推荐 XLM-Roberta-Large-Vit-B-16Plus，人脸识别Facial Recognition用的模型是：buffalo_l，压缩包内还加入了antelopev2，据说效果更好

[AI模型下载](https://huggingface.co/immich-app) / [国内源分流地址1](https://www.123pan.com/s/WXqA-EGL6d.html) / [国内源分流地址2，提取码:fgee](https://www.123pan.com/s/YuAUVv-Qp1nA.html)

### immich相册 反向地理编码设置为中文
1. [immich相册反向地理编码设置为中文资源包+教程说明](https://www.123pan.com/s/mYvMTd-SJhY3)
2. [将immich相册反向地理编码设置为中文](https://post.smzdm.com/p/an9k0w57) (教程日期：2024-08-16)
3. [将immich相册地图模块设置为中文地图](https://post.smzdm.com/p/akl59689) (教程日期：2024-04-28)

### 关于变动的说明：
1. immich-server
```
    environment:
      - IMMICH_PORT=2283
      # 自定义容器端口
```
>   2024-06-11，v1.106.3版本开始，这里的变量由SERVER_PORT改为IMMICH_PORT
2. redis

3. pgsql
```
    image: tensorchord/pgvecto-rs:pg16-v0.3.0
    # 镜像地址
```
>  指定数据库，注意tag，截至2025-01-08前，官方支持 pg14/15/16，pg17名义上是兼容的
> 
>  为确保安装的pgvecto.rs版本兼容，tag范围是 >= 0.2.0, < 0.4.0，本人的模板使用的是 pg16-v0.3.0
> 
>  [tensorchord/pgvecto-rs](https://hub.docker.com/r/tensorchord/pgvecto-rs)

