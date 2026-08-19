# MoviePilot的参数说明
- 所有配置项均支持环境变量、env配置文件、WEB界面 三种配置方式，且优先级为：环境变量 > env配置文件 == WEB界面。在环境变量中配置了的项，env配置文件及WEB界面配置将不会生效，以环境变量为准（摘抄自[MP官方文档](https://wiki.movie-pilot.org/zh/configuration)）
- 由于MP本身就需要多个项目共同配合以完成一套完整的追片系统，单纯部署需要调整好参数，参数随时间流逝，会不断变化
- 为了精简本人compose模板中一些不必要的参数，尽可能在WebUI上进行操作，下面列出一些个人认为比较重要的参数，来源于[MP官方文档](https://wiki.movie-pilot.org/zh/configuration)
- 说实话参数越写越多，模板越写越长，注释也多，有时候又顾不上，其实最好还是多看看官方文档

## 01. PT站点认证配置（变量）

<details>
<summary>最后编辑时间：2025-11-14</summary>

```yaml
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
</details>


## 02. 电影电视剧的命名规则（变量）

<details>
<summary>最后编辑时间：2025-11-14</summary>

```yaml
      - MOVIE_RENAME_FORMAT={{title}}{% if year %} ({{year}}){% endif %}/{{title}}.{{original_name}}
      - TV_RENAME_FORMAT={{title}}{% if year %} ({{year}}){% endif %}/S0{{season}}/{{original_name}}
      # 电影和电视剧重命名格式，个人自用
```
</details>


## 03. QB和TR的种子存放目录（路径）

<details>
<summary>最后编辑时间：2025-11-14</summary>

```yaml

```
</details>


## 04. 使MP调用REDIS和PGSQL（变量 + 额外容器部署）

<details>
  路径以 群晖/绿联的存储池1 作为参考，请自行修改/volume1/docker/moviepilot这部分内容为自己实际内容
<summary>最后编辑时间：2025-11-14</summary>

```yaml
########################################
      ### 如果你有一个pgsql的容器，懂得使用方法，可以根据实际情况填写，不懂照抄即可 ###
      - DB_TYPE=postgresql
      # 数据库类型，用的pgsql，保持默认
      - DB_POSTGRESQL_HOST=localhost
      # pgsql数据库的IP地址，请根据实际情况填写
      - DB_POSTGRESQL_PORT=55053
      # pgsql数据库的访问端口，本模板pgsql的端口预设为55053
      - DB_POSTGRESQL_DATABASE=moviepilot
      - DB_POSTGRESQL_USERNAME=moviepilot
      - DB_POSTGRESQL_PASSWORD=moviepilot
      # pgsql数据库的子数据库的名字、账号、密码，统一为 moviepilot


      ### 如果你有一个redis的容器，懂得使用方法，可以根据实际情况填写，不懂照抄即可 ###
      - CACHE_BACKEND_TYPE=redis
      - CACHE_BACKEND_URL=redis://:moviepilot@localhost:55054
      # 连接redis，本模板redis的访问端口预设为55054
      # 书写格式为 redis://:【redis的密码】@【redis的IP地址】:【redis的访问端口】
    depends_on:
      mp-pgsql:
        condition: service_healthy
      mp-redis:
        condition: service_healthy
    # 关联pgsql和redis，照抄


  mp-redis:
    image: redis:latest
    # 镜像地址
    container_name: mp-redis
    # 容器名字
    hostname: mp-redis
    # 主机名
    command: redis-server --save 600 1 --requirepass moviepilot
    # 最后一串字符为redis的密码，预设为moviepilot
    volumes:
      - /volume1/docker/moviepilot/redis:/data
      # 数据目录
    network_mode: bridge
    # 模板预设使用bridge网络模式，如果懂得使用方法，可以根据实际情况来修改
    ports:
      - 55054:6379
      # 注意：并不存在WebUI，只需要通过IP:PORT调用即可，模板预设
    restart: unless-stopped
    # 重启策略，可根据实际情况而选择 no/always/unless-stopped/on-failure/on-failure:3
    healthcheck:
      test: ["CMD", "redis-cli", "--raw", "incr", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    # 照抄

  mp-pgsql:
    image: postgres:17
    # 镜像地址
    container_name: mp-pgsql
    # 容器名
    hostname: mp-pgsql
    # 主机名
    volumes:
      - /volume1/docker/moviepilot/pgsql:/var/lib/postgresql/data
      # 数据目录
    environment:    
      - POSTGRES_DB=moviepilot
      - POSTGRES_USER=moviepilot
      - POSTGRES_PASSWORD=moviepilot
      # 预设新建一个子数据库，子账号及其密码，统一为 moviepilot
    network_mode: bridge
    # 模板预设使用bridge网络模式，如果懂得使用方法，可以根据实际情况来修改
    ports:
      - 55053:5432
      # 注意：并不存在WebUI，只需要通过IP:PORT调用即可，模板预设
    restart: unless-stopped
    # 重启策略，可根据实际情况而选择 no/always/unless-stopped/on-failure/on-failure:3
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U moviepilot -d moviepilot"]
      interval: 10s
      timeout: 5s
      retries: 5
    # 照抄

########################################
```
</details>
