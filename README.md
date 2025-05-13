尝试支持loongarch64


1.先去链接安装cpuinfo loongarch64的cpuinfo补丁地址： https://github.com/MQ-mengqing/cpuinfo/tree/loongarch_support 需要静态文件 如果构建系统清理静态依赖 需修改配置

2.更新/CMakeModules/DuckStationUtils.cmake文件以支持loongarch64 代码已经改好了

3.加入sd3依赖 请手动安装 https://github.com/libsdl-org/SDL

4.加入plutosvg依赖 请手动安装 https://github.com/sammycage/plutosvg  需要静态文件 如果构建系统清理静态依赖 需修改配置

5.加入discord-rpc依赖 请手动安装 https://github.com/azahar-emu/discord-rpc
