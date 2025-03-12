# 【NAS第一个用户的权限说明】
unRAID，TruNAS的 WebUI 登录的账号是 root 账号

OMV的 WebUI 登录的账号是 admin 账号，默认密码是 openmediavault，登录后记得第一时间修改

很多厂商NAS初始化过程中都会要求设置一个管理员账号，一般都不允许也不建议使用 admin 这个账号

在ssh中可以通过命令 “id 用户名” 进行查询，以下表格以 link 作为示例

一般第二用户的 UID 是第一个用户的 UID+1，但也有特殊的，具体情况请实际查询，但其实只需要第一个用户即可（即管理员账号）

而 root 则是最高管理员账号，拥有最高权限，可以读写nas中一切东西

`uid=0(root) gid=0(root) groups=0(root)`

还有 nobody，是一个特殊的用户账户，它通常用于运行那些不需要特权的服务进程

`uid=99(nobody) gid=100(users) groups=100(users),98(nobody)`

欢迎更多不同品牌的NAS来补充

| NAS | ssh中执行 id link 获取到的内容 | 其他/备注 |
| :----: | :---- | :---- |
| Debian | uid=1000(link) gid=1000(link) groups=1000(link),24(cdrom),25(floppy),<br>27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev) |
| Ubuntu | uid=1000(link) gid=1000(link) groups=1000(link) |
| unRAID | uid=1000(link) gid=100(users) groups=100(users) |
| TrueNAS-SCALE | uid=950(truenas_admin) gid=950(truenas_admin) groups=950(truenas_admin),544(builtin_administrators<br><br>uid=3000(link) gid=3000(link) groups=950(link),545(builtin_users) | truenas_admin 为系统自动创建<br>link 为自建的第一个账号 |
| OMV | uid=998(admin) gid=100(users) groups=100(users),997(openmediavault-admin)<br><br>uid=1000(link) gid=1000(link) groups=1000(link),24(cdrom),<br>25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),109(netdev)| admin 为系统自动创建<br>link 为自建的第一个账号，可在WebUI上修改 |
| CASAOS/ZimaOS |
| 群晖 | uid=1026(link) gid=100(users) groups=100(users),101(administrators) |
| 威联通 | uid=1000(link) gid=100(everyone) groups=100(everyone),0(administrators) |
| 铁威马 | uid=0(link) gid=0(everyone) groups=0(everyone),3(admin),4(allusers) |
| 万由 | uid=1001(admin) gid=100(Administrators) groups=1001(Administrators),1002(everyone) | 初始化设置时，账号默认设置为admin，不可自定义 |
| 华硕 | uid=1000(link) gid=100(users) groups=100(users),999(administrators) |
| 飞牛OS | uid=1000(link) gid=1001(Users) groups=1001(Users),1000(Administrators) |
| 新绿联 | uid=1000(link) gid=10(admin) groups=10(admin),100(users),1000(user),133(ughomeusers) |
| 旧绿联 | 激活机器的账号仅仅只是一个拥有较高权限的普通账号<br><br>通过官方途径得到的ssh使用的账号是root账号 |
| 极空间 | uid=1004(手机号) gid=1005(手机号) groups=1005(手机号),27(sudo) | 真他妈恶心 |
