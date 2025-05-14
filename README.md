尝试支持loongarch64     尝试在aosc os上编译生成 https://aosc.io/


1.先去链接安装cpuinfo loongarch64的cpuinfo补丁地址： https://github.com/MQ-mengqing/cpuinfo/tree/loongarch_support 需要静态文件 如果构建系统清理静态依赖 需修改配置

2.更新/CMakeModules/DuckStationUtils.cmake文件以支持loongarch64 [代码已经改好了](https://github.com/ouni666/duckstation/blob/master/CMakeModules/DuckStationUtils.cmake)

3.加入sd3依赖 请手动安装 https://github.com/libsdl-org/SDL

4.加入plutosvg依赖 请手动安装 https://github.com/sammycage/plutosvg  需要静态文件 如果构建系统清理静态依赖 需修改配置 全平台通用库

5.加入discord-rpc依赖 请手动安装 https://github.com/stenzek/discord-rpc 需要静态文件 如果构建系统清理静态依赖 需修改配置 

6.加入soundtouch依赖 请手动安装 https://github.com/stenzek/soundtouch 需要静态文件 如果构建系统清理静态依赖 需修改配置 

7.libzip

8.extra-cmake-modules

9.qt-6

10.加入shaderc依赖   https://github.com/stenzek/shaderc

11.加入spirv_cross依赖   https://github.com/KhronosGroup/SPIRV-Cross
