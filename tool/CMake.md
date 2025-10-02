# CMake

## 1 安装

[CMake下载地址](https://cmake.org/download/)，[MinGW下载地址](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/)



* CMake 是一个开源的跨平台构建系统生成工具，用于自动化构建过程

    它通过读取配置文件（通常是 `CMakeLists.txt` 文件）来生成适合目标平台的本地构建文件

* MinGW是一个开源的编译器套件，旨在将GNU工具链（包括GCC编译器）引入到Windows平台



为了能在终端中使用他们，需要将他们目录下的 `bin` 文件夹加入到环境变量中

若安装了 Qt 可以不重复下载，将 Qt 目录下的 `/Tools/CMake_64/bin` 和 `/Tools/mingwxxxx_64/bin` 加入环境变量

VS 中自带 `MSVC`，构建的时候如果显示的是 VS 的路径，说明调用了 `MSVC` 的 `cl.exe`， `MSVC` 的编译输出路径和 `MinGW` 不同



使用下述指令来查看是否成功配置：

```sh
gcc -v
cmake
```











## 2 构建

[CMake官方使用文档](https://cmake.org/cmake/help/latest/)



在使用 CMake 构建的程序中，像 `src`、`build`、`include` 这些文件夹有不同的作用，通常它们会按照下面的结构来组织：

1. `src`（源代码目录）

    - 存放项目的源代码文件（如`.cpp`、`.h`、`.c` 等文件）
    - 这里包括了项目的核心实现逻辑代码，通常 CMake 的 `CMakeLists.txt` 会指定该目录中的文件，以便进行编译

2. `build`（构建目录）

    - 用来存放构建过程中的中间文件和最终的可执行文件

    - `build`目录是 CMake 生成的构建目录，其中包含了 CMake 生成的 Makefile、构建脚本和中间文件（如编译的对象文件 `.o` 或 `.obj`）

        通常不会直接在 `src` 目录下进行编译，而是在 `build` 目录中创建一个单独的构建环境，这样可以保持源代码目录的干净

3. `include`（头文件目录）

    - 存放项目的头文件（`.h`、`.hpp` 等）

    - 头文件定义了类、函数的接口，通常会被 `src` 中的源文件引用

    - 在 `CMakeLists.txt` 文件中，通常会通过 `include_directories()` 或 `target_include_directories()` 告诉 CMake 头文件的路径

        以便编译器能够找到这些文件



**项目结构示例：**

```cmake
project/				 # 根目录
├── CMakeLists.txt       # CMake配置文件
├── src/                 # 源代码目录
│   ├── main.cpp
│   └── some_file.cpp
├── build/               # 构建目录
│   ├── CMakeCache.txt   # CMake缓存文件
│   ├── Makefile         # Makefile文件（如果使用Make构建）
│   └── ...
├── include/             # 头文件目录（可选）
└── ...
```



cmake 模板：

```cmake
cmake_minimum_required(VERSION 3.10)

project(项目名)

add_executable(项目名 main.cpp)
```



在 `build` 目录下输入以下指令：

```sh
cmake ../src		# 构建系统
cmake -G "MinGW Makefiles" ../src
# 安装了VS可能会优先调用

cmake --build .		# 编译

./项目名.exe		  # 运行
```



注意：

- cmake 命令不区分大小写，但是参数、变量区分大小写
- 参数用空格或分号隔开
- 使用 `${VAR}` 引用变量
- 引号可加可不加，但如果字符串中有空格必须加







## 3 常用命令

cmake 相关概念：

- 目标文件（`target`）：可执行文件（`add_executable`）、库文件（`add_library`）
- 命令（cmake-command）：下面要讲的函数
- 变量（cmake-variable）：以 `CMAKE_` 开头的变量名
- 属性（cmake-properties）：文件/文件夹都有各自的属性



### 3.1 cmake_minimum_required

设置最低 cmake 版本

```cmake
cmake_minimum_required(VERSION <min>)

# 例
cmake_minimum_required(VERSION 3.10)
```



### 3.2 project

设置项目名

```cmake
project(<PROJECT-NAME>					# 设置项目名
        [VERSION <major>				# 设置版本号
        [LANGUAGES <language-name>		# 设置使用语言
		...])

# 项目名会被存储在变量 PROJECT_NAME 和 CMAKE_PROJECT_NAME 中
# PROJECT_SOURCE_DIR 等价于 <PROJECT-NAME>_SOURCE_DIR , 指项目的源文件所在的目录, 默认为包含根 CMakeLists.txt 文件的目录
# PROJECT_BINARY_DIR 等价于 <PROJECT-NAME>_BINARY_DIR , 指项目的构建目录, 通常为 /build

# 例
project(Tutorial)
project(Tutorial C CXX)
project(Tutorial VERSION 2.3 LANGUAGES CXX)
```



### 3.3 add_executable

用指定的源文件为项目添加可执行文件，添加 `target`

```cmake
add_executable(<name> [WIN32] [MACOSX_BUNDLE]
               [EXCLUDE_FROM_ALL]
               [source1] [source2 ...])

# <name>即生成可执行文件的名字（与项目名没有关系），在一个项目中必须唯一
# 如windows系统会生成<name>.exe文件

# 例
add_executable(Tutorial tutorial.cxx)
```



### 3.4  message

打印信息

```cmake
message([<mode>] "message text" ...)

# STATUS 前缀为--的信息
# SEND_ERROR 产生错误，跳过生成过程
# FATAL_ERROR 产生错误，终止运行

# 例
message(${PROJECT_NAME})	# 打印项目名
message(STATUS "Hello world!")
```



### 3.5 set

将变量设置为指定值

```cmake
set(<variable> <value>)

# 例
set(CMAKE_CXX_STANDARD 11)		#  设置C++标准
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)		# 设置运行时目标文件（exe、dll）的输出位置
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)		# 设置存档目标文件（lib、a）的输出位置
```



### 3.6 target_include_directories

指定头文件搜索路径

```cmake
include_directories(dir1 [dir2 ...])
# 该路径会被添加到当前目录和其子目录的头文件搜索路径中, 并保存在 INCLUDE_DIRECTORIES 属性中
# 每个目标文件可以有多个搜索路径, 可以使用 dir2 参数

target_include_directories(<target> <INTERFACE|PUBLIC|PRIVATE> <dir> [...])
# 为特定的 target 设置头文件搜索路径, 它不影响其他目标的编译过程, 更推荐使用

# 例
include_directories(/include)
include_directories(${PROJECT_BINARY_DIR})	# 为整个项目添加头文件搜索路径
target_include_directories(my_executable PRIVATE /path/to/include)
# 使用 PUBLIC 时, 表示该头文件路径只影响当前目标的编译
target_include_directories(my_library PUBLIC /path/to/include /another/path)
# 使用 PRIVATE 时, 链接到该目标的其他目标会继承此头文件路径
```



### 3.7 target_link_libraries

为目标链接库

```cmake
target_link_libraries(<target> <PRIVATE|PUBLIC|INTERFACE> <library1> <library2> [...])

# library 是要链接到目标的库, 可以是 target 或绝对路径
# 将指定的库链接到一个目标上, 确保该目标在编译时可以找到并正确链接到这些库

# 例
target_link_libraries(myapp PRIVATE mylib)	# 将 mylib 库链接到 myapp 可执行文件
```



### 3.8 其他

[CMake命令文档](https://cmake.org/cmake/help/latest/manual/cmake-commands.7.html)







