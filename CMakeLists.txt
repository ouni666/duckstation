cmake_minimum_required(VERSION 3.19) 
project(duckstation C CXX) 
 
# Policy settings 
cmake_policy(SET CMP0069 NEW) 
set(CMAKE_POLICY_DEFAULT_CMP0069 NEW) 
 
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR) 
    message(FATAL_ERROR "DuckStation does not support in-tree builds. Please create a separate build directory.") 
endif() 
 
if(NOT CMAKE_BUILD_TYPE MATCHES "Debug|Devel|MinSizeRel|RelWithDebInfo|Release") 
    message(STATUS "CMAKE_BUILD_TYPE not set, defaulting to Release.") 
    set(CMAKE_BUILD_TYPE "Release") 
endif() 
 
# 针对Clang的编译器优化处理 
if(CMAKE_C_COMPILER_ID MATCHES "Clang" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang") 
    # 移除不兼容的GCC优化选项 
    foreach(var CMAKE_C_FLAGS CMAKE_CXX_FLAGS) 
        string(REPLACE "-fira-loop-pressure" "" ${var} "${${var}}") 
        string(REPLACE "-fira-hoist-pressure" "" ${var} "${${var}}") 
        set(${var} "${${var}}" PARENT_SCOPE) 
    endforeach() 
endif() 
 
# 原始配置保留部分 
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/CMakeModules/") 
include(DuckStationUtils) 
 
detect_operating_system() 
detect_compiler() 
detect_architecture() 
detect_page_size() 
detect_cache_line_size() 
 
include(DuckStationBuildOptions) 
include(DuckStationDependencies) 
include(DuckStationCompilerRequirement) 
 
if(LINUX OR BSD) 
    include(CheckPIESupported) 
    check_pie_supported() 
    set(CMAKE_POSITION_INDEPENDENT_CODE TRUE) 
endif() 
 
if(MSVC) 
    add_definitions("/D_CRT_SECURE_NO_WARNINGS") 
    foreach(config CMAKE_C_FLAGS CMAKE_CXX_FLAGS) 
        string(REPLACE "/W3" "/W4" ${config} "${${config}}") 
        set(${config} "${${config}} /Oi /Gm- /utf-8 /Zc:__cplusplus") 
    endforeach() 
 
    string(REPLACE "/Ob1" "/Ob2" CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELWITHDEBINFO}") 
    string(REPLACE "/Ob1" "/Ob2" CMAKE_C_FLAGS_DEVEL "${CMAKE_C_FLAGS_DEVEL}") 
    string(REPLACE "/Ob1" "/Ob2" CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO}") 
    string(REPLACE "/Ob1" "/Ob2" CMAKE_CXX_FLAGS_DEVEL "${CMAKE_CXX_FLAGS_DEVEL}") 
 
    string(REPLACE "/INCREMENTAL" "/INCREMENTAL:NO" CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO "${CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO}") 
    string(REPLACE "/INCREMENTAL" "/INCREMENTAL:NO" CMAKE_EXE_LINKER_FLAGS_DEVEL "${CMAKE_EXE_LINKER_FLAGS_DEVEL}") 
 
    set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} /OPT:REF /OPT:ICF") 
    set(CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO "${CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO} /OPT:REF /OPT:ICF") 
    set(CMAKE_EXE_LINKER_FLAGS_DEVEL "${CMAKE_EXE_LINKER_FLAGS_DEVEL} /OPT:REF /OPT:ICF") 
endif() 
 
if(CMAKE_C_COMPILER_ID MATCHES "Clang" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR CMAKE_C_COMPILER_ID MATCHES "GCC" OR CMAKE_CXX_COMPILER_ID MATCHES "GCC") 
    include(CheckCXXFlag) 
    check_cxx_flag(-Wall COMPILER_SUPPORTS_WALL) 
    check_cxx_flag(-Wno-class-memaccess COMPILER_SUPPORTS_MEMACCESS) 
    check_cxx_flag(-Wno-invalid-offsetof COMPILER_SUPPORTS_OFFSETOF) 
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-switch") 
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-switch") 
endif() 
 
if(MSVC) 
    string(REPLACE "/EHsc" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}") 
    string(REPLACE "/GR" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}") 
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /D_HAS_EXCEPTIONS=0 /permissive-") 
    if(CMAKE_CXX_COMPILER_ID MATCHES "Clang") 
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /clang:-fno-rtti") 
    endif() 
else() 
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -fno-rtti") 
endif() 
 
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin") 
 
if(ALLOW_INSTALL) 
    if(INSTALL_SELF_CONTAINED) 
        set(CMAKE_INSTALL_BINDIR "${CMAKE_INSTALL_PREFIX}") 
        set(CMAKE_INSTALL_LIBDIR "${CMAKE_INSTALL_PREFIX}") 
    else() 
        include(GNUInstallDirs) 
    endif() 
endif() 
 
if(CMAKE_SIZEOF_VOID_P EQUAL 4) 
    add_definitions("-D_FILE_OFFSET_BITS=64") 
endif() 
 
if(BUILD_TESTS) 
    enable_testing() 
endif() 
 
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DFMT_EXCEPTIONS=0") 
 
add_subdirectory(dep) 
add_subdirectory(src) 
 
include(DuckStationBuildSummary) 
