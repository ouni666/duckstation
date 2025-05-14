尝试支持loongarch64     尝试在aosc os上编译生成 https://aosc.io/


1.先去链接安装cpuinfo loongarch64的cpuinfo补丁地址：   https://github.com/MQ-mengqing/cpuinfo/tree/loongarch_support 需要静态文件 如果构建系统清理静态依赖 需修改配置

2.更新/CMakeModules/DuckStationUtils.cmake文件以支持loongarch64 [代码已经改好了](https://github.com/ouni666/duckstation/blob/master/CMakeModules/DuckStationUtils.cmake)

3.加入sd3依赖 请手动安装                               https://github.com/libsdl-org/SDL

4.加入plutosvg依赖 请手动安装                          https://github.com/sammycage/plutosvg  需要静态文件 如果构建系统清理静态依赖 需修改配置 全平台通用库

5.加入discord-rpc依赖 请手动安装                       https://github.com/stenzek/discord-rpc 需要静态文件 如果构建系统清理静态依赖 需修改配置 

6.加入soundtouch依赖 请手动安装                        https://github.com/stenzek/soundtouch 需要静态文件 如果构建系统清理静态依赖 需修改配置 

7.libzip              安装命令 oma install libzip

8.extra-cmake-modules 安装命令 oma install extra-cmake-modules 

9.qt-6                安装命令 oma install qt-6

10.加入shaderc依赖                                    https://github.com/stenzek/shaderc

11.加入spirv-cross依赖        注意aosc系统构建要求（_下划线改-）                        https://github.com/KhronosGroup/SPIRV-Cross

CMake Error at CMakeModules/DuckStationDependencies.cmake:47 (find_package):
  By not providing "Findspirv_cross_c_shared.cmake" in CMAKE_MODULE_PATH this
  project has asked CMake to find a package configuration file provided by
  "spirv_cross_c_shared", but CMake did not find one.

  Could not find a package configuration file provided by
  "spirv_cross_c_shared" with any of the following names:

    spirv_cross_c_sharedConfig.cmake
    spirv_cross_c_shared-config.cmake

  Add the installation prefix of "spirv_cross_c_shared" to CMAKE_PREFIX_PATH
  or set "spirv_cross_c_shared_DIR" to a directory containing one of the
  above files.  If "spirv_cross_c_shared" provides a separate development
  package or SDK, be sure it has been installed.
Call Stack (most recent call first):
  CMakeLists.txt:32 (include)

