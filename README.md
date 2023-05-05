# 怎么用在 Visual Studio Code 中使用 Cmake 编译 C++ 代码

> 以下步骤需要在 macOS 上

### 1. 安装 g++, clang, git, make 等工具
`xcode-select --install`

### 2. 安装 cmake
`brew install cmake`

### 3. VS Code 安装扩展
安装 C/C++ extension，Cmake 相关扩展

### 3. 创建工程
新建一个文件夹，在里面创建一个 main.cpp 文件，可以添加一些简单的测试代码，比如下面这样：
```c++
#include <iostream>

using namespace std;

int main() {
    cout << "Test cpp by cmake." << endl;
    return 0;
}
```

### 4. 生成 CMakeLists.txt 文件
按 `command + shift + p` 然后输入 `Cmake: Quick Start`，
然后根据提示选择 `Clang` 或者 `GCC`，这边我们选择 `Clang` 为例，
然后输入 CMake 项目名称（比如：test_cpp），
最后选择 `Executable`

这样，就在项目根目录生成了如下的 CMakeLists.txt：
```
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

add_definitions(-std=c++11)

set(CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS, "${CXX_FLAGS}")

set(CMAKE_BUILD_TYPE Debug)

project(test_cpp)

add_executable (main main.cpp)
# 如果有多个文件，一个一个写不方便，可以用以下简便写法
# aux_source_directory(. SOURCES)
# add_executable(main ${SOURCES})
```

### 5. 测试一下 CMake 编译
执行如下命令

先在 build 目录执行 cmake 命令生成相关编译用的文件
```
mkdir build
cd build
cmake ..
```

执行 make 命令去编译，当然也可以在后面加 -j2 -j4 指定同时运行的作业数量
```
make
```

最后执行 make 命令生成的 main 文件
```
./main
```

### 6. 需要打断点 debug 这么设置呢？
菜单栏 `Run → Start Debugging → C++(GDB+lldb) → Default Configuration` 会自动生成 `launch.json` 文件。
如果没有自动生成或者内容不对，直接在 `.vscode` 目录下新建 `launch.json` 文件，然后加入如下代码：
> 注意：program 部分要修改为自己的可执行文件
> 再注意：打断点需要电脑上安装了 gdb，可以通过 which gdb，没找到的话可以通过 brew install gdb 命令安装
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(lldb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/main",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "lldb"
        }
    ]
}
```

最后按 F5 或者左侧的三角按钮进行 debug
