- 以下内容从模板中剔除了
````
      ######################################
      # - REDIS_HOST=127.0.0.1
      # 指定要连接的Redis的地址，默认为127.0.0.1
      # - REDIS_PORT=6379
      # 指定要连接的Redis的端口号，默认为6379
      # - REDIS_PASSWORD=Redis的密码
      # 指定要连接的Redis的密码，具体看自己的配置单
      ######################################
      # https://mtmt.tech/docs/advanced/env
      # 以下内容来源于官方文档，均为默认，可以不写
      # - RAW_SUPPORT=open
      # RAW格式支持，默认开启
      # - SCAN_INTERVAL=15
      # 指定自动扫描图库的间隔时间，单位为分钟，默认为15，最大支持9999
      # - EXIF_OVERWRITE_TYPE=overwrite_original_in_place
      # 指定exiftool写入模式，默认为overwrite_original
      # - DAY_MAX_FILE_NUM=298
      # 时间线模式中单天显示的照片数量上限，默认为298
      # - STREAM_LINK_TTL=30
      # 分享的串流地址有效时间，单位为分钟，默认为30
      # CACHE_DIR_PATH=/config/cache
      # 自定义保存缩略图的位置，默认为/config/cache
      ######################################
      # - PROXY_HOST_AMAP=http://xxx.com/
      # 指定代理地址-高德api，默认为空
      # - PROXY_HOST_MAPBOX=http://xxx.com/
      # 指定代理地址-mapbox api，默认为空
      ######################################
      # - PROXY_HOST_AUTH=http://xxx.com/
      # 指定代理地址-授权服务器，默认为空，仅作预留，不写
      ######################################
````
