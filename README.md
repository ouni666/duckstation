尝试支持loongarch64


1.先去链接安装cpuinfo loongarch64的cpuinfo补丁地址： https://github.com/MQ-mengqing/cpuinfo/tree/loongarch_support 需要静态文件 如果构建系统清理静态依赖 需修改配置

2.更新/CMakeModules/DuckStationUtils.cmake文件以支持loongarch64 代码已经改好了

3.加入sd3依赖 请手动安装 https://github.com/libsdl-org/SDL

4.加入plutosvg依赖 请手动安装 https://github.com/sammycage/plutosvg  需要静态文件 如果构建系统清理静态依赖 需修改配置 全平台通用库

5.加入discord-rpc依赖 请手动安装 https://github.com/azahar-emu/discord-rpc 全平台通用库
暂时遇到报错discord-rpc没有配置文件 先记录 后续再改
CMake Error at CMakeModules/DuckStationDependencies.cmake:21 (find_package):
  By not providing "FindDiscordRPC.cmake" in CMAKE_MODULE_PATH this project
  has asked CMake to find a package configuration file provided by
  "DiscordRPC", but CMake did not find one.

  Could not find a package configuration file provided by "DiscordRPC"
  (requested version 3.4.0) with any of the following names:

    DiscordRPCConfig.cmake
    discordrpc-config.cmake

  Add the installation prefix of "DiscordRPC" to CMAKE_PREFIX_PATH or set
  "DiscordRPC_DIR" to a directory containing one of the above files.  If
  "DiscordRPC" provides a separate development package or SDK, be sure it has
  been installed.
Call Stack (most recent call first):
  CMakeLists.txt:32 (include)
