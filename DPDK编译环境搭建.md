# DPDK编译环境搭建

系统我个人用的debian 10.2, 硬件用的阿里云ECS.S6, 规格1核处理器, 2G内存, 2张网卡, 1M带宽.

## 安装需要的程序和包

```bash
apt install pkgconf gcc-multilib numactl libdpdk-dev  && pip3 install meson ninja pyelftools
```

## 下载源码并解压

```bash
curl -O http://fast.dpdk.org/rel/dpdk-21.11.1.tar.xz && xz -d dpdk-21.11.1.tar.xz && tar -xvf dpdk-21.11.1.tar && mv dpdk-stable-21.11.1 dpdk && rm dpdk-21.11.1.tar
```
## 编译

```bash
cd dpdk && meson build && cd build && ninja && ninja install && ldconfig
```

## 加载驱动内核

```bash
modprobe uio_pci_generic
```

## 配置大页

配置了128个2MB大页内存.

```bash
echo 128 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages && mkdir /mnt/huge && mount -t hugetlbfs pagesize=1GB /mnt/huge
```

检查配置是否成功.

```bash
grep -i HugePages_Total /proc/meminfo # 查看大页是否配置成功
mount | grep hugetlbfs # 查看是否挂载成功
cat /proc/meminfo | grep Huge # 查看详细的大页信息
```

## 绑定网卡

```bash
./usertools/dpdk-devbind.py --bind=uio_pci_generic <device name or <bus>:<device>.<function>
```

查看网卡具体信息.

```bash
./usertools/dpdk-devbind.py --status # 查看网卡具体信息
```

## TLDK安装

编译安装.

```bash
git clone git@github.com:FDio/tldk.git && cd tldk && make all
```

## vpp安装(可选)

直接安装编译好的包, 下面是debian的, 如果你的系统没有好的软件生态则需要自行编译.

```bash
curl -s https://packagecloud.io/install/repositories/fdio/release/script.deb.sh | sudo bash && apt update && apt install vpp vpp-plugin-core vpp-plugin-dpdk
```

## VScode设置(可选)

这步可自行选择编辑器和编译器, 我这里只提供VScode + make cmake gcc gdb的解决方案, 由于选择的dpdk版本为21.1.1, 所以也可以选择meson和ninja.

> 记得安装编译调试软件, 以及安装vscode所需要的插件.

```bash
apt install cmake make gdb gcc #软件安装
```

工作区根目录下新建.vscode文件夹和CMakeLists.txt, 在.vscode文件夹下新建tasks.json和launch.json.

tasks.json如下.

```json
{
    "tasks": [
        {
            "type": "shell",
            "label": "cmake",
            "command": "cmake",
            "args": [
                "-DCMAKE_BUILD_TYPE=DEBUG",
                "."
            ],
            "options": {
                "cwd": "${workspaceFolder}"
            }
        },
        {
            "type": "shell",
            "label": "make",
            "command": "make",
            "args": [],
            "options": {
                "cwd": "${workspaceFolder}"
            },
            "dependsOn": [
                "cmake"
            ]
        }
    ],
    "version": "2.0.0"
}
```

launch.json如下.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${command:cmake.launchTargetPath}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${command:cmake.launchTargetDirectory}",
            "environment": [
                {
                    "name": "PATH",
                    "value": "$PATH:${command:cmake.getLaunchTargetDirectory}"
                }
            ],
            "externalConsole": false,
            "avoidWindowsConsoleRedirection": false,            
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "make",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
```

CMakeList.txt如下.

```txt
# 声明要求的cmake最低版本
cmake_minimum_required( VERSION 3.13.4 )

# 声明项目名
set( PROJECT_NAME demo)

# 添加gcc标准支持
set( CMAKE_C_COMPLIE "gcc" )

# 声明一个cmake工程
project( ${PROJECT_NAME} )

# 开启调试模式
set(CMAKE_C_FLAGS_DEBUG "-g -DEBUG")
set(CMAKE_CXX_FLAGS_DEBUG "-g -DEBUG")

# 找到后面需要库和头文件的包
find_package( PkgConfig REQUIRED )
pkg_check_modules( dpdk REQUIRED IMPORTED_TARGET libdpdk )

# 编译器
set(CMAKE_CXX_COMPILER "/usr/bin/gcc")

# 头文件
include_directories(
 ${PROJECT_SOURCE_DIR}
 ${PROJECT_SOURCE_DIR}/include
 ${EIGEN3_INCLUDE_DIR}
)

# 源文件
aux_source_directory(${PROJECT_SOURCE_DIR}/src DIR_SRCS)

# 生成可执行文件
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
add_executable(${PROJECT_NAME} ${DIR_SRCS})

# 添加pkg-config库 
target_link_libraries(${PROJECT_NAME} PRIVATE PkgConfig::dpdk ${CMAKE_THREAD_LIBS_INIT})
```

netcope-common doxygen sphinx-build
apt install libelf-dev libmusdk-dev libmlx5-dev libcrypto-dev libbpf-dev libmlx4-dev libaarch64crypto-dev libisal-dev librxp_compiler-dev libturbo-dev libldpc_decoder_5gnr-dev libipsec-dev zlib1g-dev


wlop