


- mtphotos/mt-photos-deepface作为备选，已从compose中移除

```
  mt-photos-deepface:
    image: mtphotos/mt-photos-deepface:latest
    # 镜像地址，不支持arm架构
    # noavx-latest: 如果改容器无法启动，尝试使用这支持cpu无avx指令的tag
    # cuda-latest: CUDA版本镜像，N卡用户可以选择这个tag
    container_name: mt-photos-deepface
    # 容器名字
    hostname: mt-photos-deepface
    # 主机名
    environment:
      - API_AUTH_KEY=1234567890
      # 人脸识别API，可以随便写，在mt-photos内设置要对应该项
      ######################################
    # volumes:
      # - 【这里替换为你的docker数据存放目录】/mt-photos/deepface/models:/models
      ## 镜像已内置 retinaface.h5 和 facenet512_weights.h5，如果不修改识别模型，不需要添加目录映射
      ## 当指定了别的模型，容器内下载模型很慢，可以增加 /models 目录映射
      ## 预训练模型下载地址：https://github.com/serengil/deepface_models/releases/tag/v1.0
      ## 如果访问下载github速度慢，请打开下面这个github代理加速下载网站进行下载
      ## https://mirror.ghproxy.com/
      ## 将对应的预训练模型放到容器内的 /models/.deepface/weights/ 下，请对应放到实际路径上
      ######################################
    network_mode: bridge
    # network_mode: host
    # host模式需要容器内的端口不被占用，不需要端口映射，后续端口映射全都开头加#注释掉，否则注释掉这条
    ports:
      - 8065:8066/tcp
      # API 端口
      # 注意：并不存在WebUI，只需要通过IP:PORT调用即可
    restart: always
    # 重启策略，可根据实际情况而选择 no/always/unless-stopped/on-failure/on-failure:3
    labels:
      net.unraid.docker.managed: dockerman
      net.unraid.docker.webui: https://mtmt.tech/docs/advanced/facial_api
      # 因不存在WebUI，所以此处为官方说明文档
      net.unraid.docker.icon: /mnt/user/LOGO/mt-photos-deepface.png
      # 适用于unraid的图标，可以写unRAID的路径地址，也可以写一个图标地址(局域网或广域网均可)
      # 注意：通过compose创建的docker容器，无法在unRAID上进行编辑
```
