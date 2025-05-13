尝试支持loongarch64


1.先去链接安装cpuinfo loongarch64的cpuinfo补丁地址： https://github.com/MQ-mengqing/cpuinfo/tree/loongarch_support

2.更新/CMakeModules/DuckStationUtils.cmake文件以支持loongarch64 代码已经改好了

3.加入sd3依赖 请手动安装 https://github.com/libsdl-org/SDL

4.加入plutosvg依赖 请手动安装 https://github.com/sammycage/plutosvg
