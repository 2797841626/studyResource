1. **g++**:
    
    - g++ 是 GNU Compiler Collection (GCC) 中的 C++ 编译器。
    - 它用于将 C++ 源代码编译成可执行程序或库文件。
    - g++ 支持各种 C++ 语言特性和编译优化选项。
2. **gcc**:
    
    - gcc 是 GCC 中的 C 语言编译器。
    - 它用于将 C 语言源代码编译成可执行程序或库文件。
    - gcc 与 g++ 类似,但针对的是 C 语言而不是 C++。
3. **gdb**:
    
    - gdb 是 GNU 调试器,用于调试 C/C++ 程序。
    - 它允许开发者在程序运行时检查变量值、设置断点、单步执行等,以帮助发现和修复程序中的错误。
4. **MinGW**:
    
    - MinGW (Minimalist GNU for Windows) 是一个在 Windows 上使用 GCC 编译器的环境。
    - 它提供了 gcc、g++、gdb 等 GNU 工具的 Windows 版本,使得在 Windows 上进行 C/C++ 开发变得更加方便。
5. **Ninja**:
    
    - Ninja 是一个快速的构建系统生成器。
    - 它使用简单的构建规则文件 (build.ninja) 来描述如何构建目标文件,并提供高效的并行构建能力。
    - Ninja 通常与 CMake 一起使用,以生成跨平台的构建脚本。
6. **CMake**:
    
    - CMake 是一个跨平台的构建系统生成工具。
    - 它允许开发者编写一份独立于构建工具的构建脚本 (CMakeLists.txt),CMake 可以根据该脚本生成适用于各种平台和编译器的构建文件,如 Makefiles、Visual Studio 解决方案、Xcode 项目等。
    - CMake 可以自动检测系统环境,并根据环境配置构建过程。
这些工具都在 C/C++ 开发过程中扮演着重要的角色:g++/gcc 负责编译源代码,gdb 用于调试,MinGW 提供了 Windows 下的 GNU 工具链,Ninja 和 CMake 则简化了跨平台的构建过程。

在Windows下使用MinGW手动编译c++程序的方式
在build文件下输入 cmake -G "MinGW Makefiles" ..
之后输入 mingw32-make


##### VSCode CMake 安装与配置详解


在windows上使用cmake编译C/C++程序时，首先需要CMake,安装gcc/g++编译环境，然后使用VSCode 以及配置下CMakelist.txt。
所需工具：

VSCode (需要安装以下插件)
C/C++
C++ Intellisense
CMake
CMake tools
CMake Tools Helper

CMake
MinGW
安装CMake
下载链接：
https://cmake.org/download/

尽量选择Latest Release版本，比较稳定。
如图中红框所示，下载后缀为.msi的安装文件，然后直接安装。


安装目录选择默认：C:\Program Files\CMake\

验证安装成功
在命令行 输入如下指令

cmake -version
1


###### 安装MinGW
在ubuntu系统上我们可以直接安装gcc/g++，但在windows上无法直接安装g++,这时候就需要用到MinGW啦，MinGW是从Cygwin（1.3.3版）基础上发展而来。GCC支持的语言大多在MinGW也受支持，其中涵盖C、C++、Objective-C、Fortran及Ada。对于C语言之外的语言，MinGW使用标准的GNU运行库，如C++使用GNU libstdc++。
下载链接：

https://sourceforge.net/projects/mingw/

这个是在线安装器，需要在线下载安装内容安装，才会完成安装，安装地址建议使用默认路径，避免出现一些莫名的问题：

C:\MinGW
1
如果下的是兼容32/64位的版本，下载安装默认路径是"C:\Program Files(x86)…",安装的时候需要删去"Program Files(x86)",把==“mingw32-make.exe”重命名为"make.exe，==这样才能用make命令正常使用。否则就mingw32-make使用。

根据需要选择你的组件。右键选择“Mark for Installation”,之后选择"Installation -> Apply Changes”。等待下载完成。

验证安装成功
在命令行 输入如下指令

gcc -v
make -v

###### VSCode中配置CMake

一般刚安装CMake插件后 会自动提示你选择一个编译工具链，如果没有提示或者想更换其他编译工具链，那么可以通过ctrl+shifl+p,输入以下指令，然后在弹出框中选择自己安装的编译工具链。
![[Pasted image 20240712212925.png]]
CMake:Select a Kit
1

如果想重新配置本地的编译工具链的安装位置，那么可以打开如下配置

`CMake:Edit user-local CMake kits`
1
配置完毕~

编写测试代码：
main.cpp

`#include <iostream>`
`using namespace std;`

`int main(int agec, char **argv) {`
    `cout << "hello word ,form vscode cmake" << endl;`
`}`

编写CMake文件
CMakeLists.txt

`cmake_minimum_required(VERSION 3.0)`
`project(vscode_cmake_Test)`
`aux_source_directory(. DIR_TOOT_SRCS)`
`set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")`
`add_executable(${PROJECT_NAME} ${DIR_TOOT_SRCS})`

生成Make file
第一次需要输入"cmake -G"Unix Makefiles" …/",尤其是电脑装了Visual Studio如果直接"cmake …"会生成VS的工程文件，所以这里需要指定下。

mkdir build
cd build
cmake -G "Unix Makefiles" ../



编译
mingw32-make
1
运行程序
> .\vscode_cmake_Test.exe
hello word ,form vscode cmake
