仅支持RK3588系列,能调用NPU，处理效果很不错，例如绿联DH4300plus，香橙派5系列，友善cm3588，cyber-aio-3588等开发板均可

但是，cm3588刷上飞牛OS官方系统，两个AI镜像却无法启动，不断重启，原因不明，大佬也不想折腾，不过飞牛OS的直接用飞牛相册也足够了

命令查看npu调度情况 watch -n 1 cat /sys/kernel/debug/rknpu/load
