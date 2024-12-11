#### 【更新日志-2024-11-27】
- 加料更新MT相册的compose，注释量巨大，强烈建议去官方文档，写的极度详细，一步到胃
- 特殊版本中新增jellyfin的rockchip版compose，简单测评了一下
- RK3588/3568播放 “UHD.BluRay.2160p.x265.10bit.DV.HDR.TrueHD7.1”，RK3588色彩映射正常，RK3568打开色彩映射会无法播放，不开则可以，但色彩不正常
![image](https://github.com/FrozenGEE/compose/blob/main/%5B06%5D%20%E7%89%B9%E6%AE%8A%E7%89%88%E6%9C%AC/31.jellyfin-%E7%A7%81%E4%BA%BA%E5%AA%92%E4%BD%93%E5%BA%93_RK%E7%89%B9%E4%BE%9B%E7%89%88/JF-RK3588-01.png)
![image](https://github.com/FrozenGEE/compose/blob/main/%5B06%5D%20%E7%89%B9%E6%AE%8A%E7%89%88%E6%9C%AC/31.jellyfin-%E7%A7%81%E4%BA%BA%E5%AA%92%E4%BD%93%E5%BA%93_RK%E7%89%B9%E4%BE%9B%E7%89%88/JF-RK3568-01.png)
- RK3568播放 “BluRay.1080p.x265.10bit.3Audio” 这不带DV HDR规格的视频则正常
![image](https://github.com/FrozenGEE/compose/blob/main/%5B06%5D%20%E7%89%B9%E6%AE%8A%E7%89%88%E6%9C%AC/31.jellyfin-%E7%A7%81%E4%BA%BA%E5%AA%92%E4%BD%93%E5%BA%93_RK%E7%89%B9%E4%BE%9B%E7%89%88/JF-RK3568-03.png)
- 依此类推，如果需要arm硬件转码且视频规格高的，选用RK3588，规格不高可以选择RK3588之下的其他RK CPU

#### 【更新日志-2024-11-14】
- 铁威马TOS5和TOS6系统的路径有所改变，修正这部分内容，新增对应的路径说明
- 新增 万由UNAS6.0 的路径，端口，第一用户权限等说明，端口注意如果模板有冲突，自行修改
- 上传万由UNAS的compose模板，味精测试，应该没问题，系统永久版单卖499rmb
- 修正MP-v2使用6666端口无法访问WebUI的问题，这个6666端口居然是特殊端口，涨知识了
- MP-v1的3333端口改为33033，v2的6666端口为55055

#### 【更新日志-2024-11-10】
- 重新整理 群晖&新绿联&华硕 的compose，他们三的路径都是一样的，完全通用，好事
- 因为本人没有华硕NAS的设备，compose 味精测试，但应该冇问题，一些细节后续再去慢慢修
- 华硕的第一个用户的权限：uid=1000(用户名) gid=100(users) groups=100(users),999(administrators)
- 新增华硕NAS的路径和端口说明，端口注意如果模板有冲突，自行修改
  
#### 【更新日志-2024-11-07】
- 移动了某些模板以便于查找（如frps和frpc，mviepolit）
- moviepilot旧的模板删了，换新的，v1和v2两个版本，均可共存，模板增减了部分内容，注意检查，一切以官方为准
- MP的模板存放在特殊版本里的20；01那份模板中的MP还没有修改，还是v1的旧版模板，但是其实没什么区别，还是能正常部署的，找天空闲再去修改
- 下次找个时间把github好好整理，本人并不太会写md文档，这份github内容越写越多了，需要精简，优化排版了
- 还有一件事：fork来干嘛，你直接zip打包下载得了，我又不跑路，我修修改改频繁的要命的，甚至还有一些错漏的地方（指注释内容），特意不修正的

#### 【更新日志-2024-10-22】
- 上传适用于铁威马的compose模板，味精测试，但应该冇问题
- 铁威马的第一个用户的权限：uid=0(用户的名字) gid=0(everyone) groups=0(everyone),3(admin),4(allusers)
- 新增部分WebUI端口说明，见下方内容，模板写的时候不一定都有考虑到NAS本身服务所用到的端口，所以有的模板可能没有写好，导致冲突，这部分自行修改了

#### 【更新日志-2024-10-09】
- QB和TR如果下载出错，可以尝试给root权限，即PUID和PGID为0（已写入注释中）
- MP模板推荐去看 [06] 里面的 aio模板 和 附带的图片 作为部署，下载路径，媒体库路径的设置参考

#### 【更新日志-2024-10-03】
- 开坑特殊版本，主要是放一些附带专属数据库的docker容器，或者一些组合搭配的
- 例如：媒体库自动化一条路部署 PEJ+mp+qb+tr+iyuu+cookiecloud(附带一个v2raya)，compose旁边有三张图可供参考，记得看
- 注意：仅供参考学习交流，有一定概率挖坑不填，比较要反复测试，懒癌......
- y1s1，自动化其实不一定好使，或者符合个人习惯/需求，比如我，顶多算个好找资源，方便整理，降低工作量的手段

#### 【更新日志-2024-09-24】
#### 💡【紧急】plex/emby/jf模板删除使用内存作为缓冲目录的参数💡
#### 💡【紧急】plex/emby/jf模板删除使用内存作为缓冲目录的参数💡
#### 💡【紧急】plex/emby/jf模板删除使用内存作为缓冲目录的参数💡
- 【紧急】新绿联用了会导致设备变回初始化，并且无法初始化成功
- 【紧急】但是可以重启恢复，稳妥起见还是从模板中删除了

#### 【更新日志-2024-09-24】
- 绿联新系统大换血，用就得抛弃旧系统的思维
- 【重要】更新绿联新系统的个人文件夹的绝对路径说明，看下面，官方相册文件夹无法自定义存放在哪个存储池，只能把整个个人文件夹设置到其他存储池才可以修改
- 绿联新系统的compose和群晖一样，都需要提前建立好对应的共享文件夹，文件夹才可以执行成功，如果用portainer等第三方的部署，就只需要创建好共享文件夹即可
- 绿联新系统compose不像群晖那样子选择保存路径后，路径之下现有的compose文件不会提示调用，体验略差

#### 【更新日志-2024-08-27】
- 速更新增fnOS的支持，其实就是把模板稍微改改替换一下就完事了，新NAS系统简单体验了一下，还挺不错的
- [飞牛OS](https://www.fnnas.com)的第一个用户的权限（也是管理员账号）：uid=1000(用户的名字) gid=1001(Users) groups=1001(Users),1000(Administrators)

#### 【更新日志-2024-07-15】
- 上传适用于威联通的compose模板，味精测试，但应该冇问题，仅支持QuTScloud，QTS5.0版本及其以上，Container Station(docker)的路径可能和以前的qnap不太一样，冇求证过
- 威联通的第一个用户的权限：uid=1000(用户的名字) gid=100(everyone) groups=100(everyone),0(administrators)

#### 【更新日志-2024-07-10】
- 提供logo图，可用于unRAID，CasaOS的图标显示，也可以用于Sun-Panel当中使用，其余同理
- logo源于 [xushier/HD-Icons](https://github.com/xushier/HD-Icons)、[walkxcode/dashboard-icons](https://github.com/walkxcode/dashboard-icons)、casaos和unraid商店、有些是从网上找的
- [整合包-2024-07-10](https://www.123pan.com/s/YuAUVv-eW1nA.html)

##### 【更新日志-2024-07-05】
- 来自casaos的项目，YAML Generator 生成器：https://play.cuse.eu.org/generate
- 通过“在线docker面板”生成对应的compose模板，666666，流下没有技术的泪水
- 项目来源：[CasaOS-AppStore-Play](https://github.com/Cp0204/CasaOS-AppStore-Play)
- 另外推举一个cli转compose的网站：https://www.composerize.com
- 123盘的docker离线镜像好久没更新了，这里附上docker加速源项目：[cmliu/CF-Workers-docker.io](https://github.com/cmliu/CF-Workers-docker.io) ，推荐自建，只需要域名+cloudflare即可

##### 【更新日志-2024-06-25】
- 上传一些针对于群晖和新绿联的预设写好的模板，默认存储池均为/volume1，部分内容还需要自己稍微修改，或者增添，注意看注释
- 群晖用户请预先创建好对应的路径的文件夹，绿联用户请预先创建好共享文件夹
- unRAID用户不需要预先创建文件夹，对于unRAID的模板，linux原生系统都可以直接用

##### 【更新日志-2024-06-17】
- 上传一些模板，初步摸索github怎么用T_T
- docker国内墙了，对于一些小白无法下载镜像，这里提供个人拉取导出的镜像文件，拉取时间是6月8日，保证是当天最新，但不保证一定有你想要的镜像，部分镜像在后续有所更新，偶尔更新的频率
- https://www.123pan.com/s/YuAUVv-Qp1nA.html 提取码:fgee
