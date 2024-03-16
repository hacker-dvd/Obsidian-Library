
1. 读取当前目录的 CMakeLists.txt，并在 build 文件夹下生成 build/Makefile:`cmake -B build`，
	 若想指定 clang++为编译器，使用命令`cmake -Bbuild -DCMAKE_CXX_COMPILER=clang++`
	 若想指定c++版本，同样在后面添加参数：`-DCMAKE_CXX_STANDARD=23` 
	 也可以在 CMakeLists.txt 中添加`set(CMAKE_CXX_STANDARD 23)`
1. 让 make 读取 build/Makefile，并开始构建 a.out：`make -C build` 
	1. 更跨平台的写法：`cmake --build build`
	2. 不执行，查看构建命令：`make --dry-run -C build`
2. 执行生成的可执行文件(例如，a.out)：`build/a.out`

# 静态库和动态库

CMake 使用`add_library`生成库文件：

```CMake
add_library(test STATIC source1.cpp source2.cpp) # 生成静态库 libtest.a
add_library(test SHARED source1.cpp source2.cpp) # 生成动态库 libtest.so
```

创建库以后，要在某个可执行文件中使用该库，只需要：

`target_link_libraries(myexec PUBLIC test)`为`myexec`链接刚刚制作的库`libtest.a`

这时 CMakeLists.txt 的写法如下：

```CMake
cmake_minimum_required(VERSION 3.25)
project(hellocmake LANGUAGES CXX)

add_library(hellolib STATIC hello.cpp)
add_executable(a.out main.cpp)
target_link_libraries(a.out PUBLIC lib1)
```

# cmake 的子模块

以 cmake_project1 为例

复杂的工程中，我们需要划分子模块，通常一个库一个目录，比如：这里我们把 lib1 库的东西移到 hellolib 文件夹下了，里面的 CMakeLists.txt 定义了 lib1 的生成规则。要在根目录使用他，可以用 CMake 的 add_subdirectory 添加子目录，子目录也包含一个 CMakeLists.txt，其中定义的库在 add_subdirectory 之后就可以在外面使用。

因为 hello.h 被移到了 hellolib 子文件夹里，因此 main.cpp 里也要改成：`#include "hellolib/hello.h"`，如果要避免修改代码，我们可以通过 target_include_directories 指定
a.out 的头文件搜索目录：

![[target_include_directories指定目录.png]]

这样甚至可以用 <hello.h> 来引用这个头文件了，因为通过 target_include_directories 指定的路径会被视为与系统路径等价

但是这样如果另一个 b.out 也需要用 lib1 这个库，难道也得再指定一遍搜索路径吗？
不需要，其实我们只需要定义 lib1 的头文件搜索路径，引用他的可执行文件 CMake 会自动添加这个路径：在 hellolib/CMakeLists.txt 添加`target_include_directories(lib1 PUBLIC .)`

这里用了 . 表示当前路径，因为子目录里的路径是相对路径，类似还有 .. 表示上一层目录。
此外，如果不希望让引用 lib1 的可执行文件自动添加这个路径，把 PUBLIC 改成 PRIVATE 即可。这就是他们的用途：决定一个属性要不要在被 link 的时候传播。

# cmake 的其他一些选项：

![[cmake选项.png]]

# cmake 选项

## `DCMAKE_BUILD_TYPE`
  
在 CMake 中，`DCMAKE_BUILD_TYPE` 是一个常用的变量，用于指定构建类型。通过设置这个变量，你可以控制编译器的优化级别以及是否包含调试信息。这对于在不同阶段的开发中调整构建配置非常有用，比如进行调试或准备最终的发布版本。

这个变量通常在运行 CMake 配置命令时作为命令行参数提供，如下所示：
`cmake .. -DCMAKE_BUILD_TYPE=Release`

这里 `-D` 是用来定义一个变量的，`CMAKE_BUILD_TYPE` 就是变量名，后面的 `Release` 是这个变量的值。

`CMAKE_BUILD_TYPE` 可以有几个不同的值，常用的包括：

- `Debug`：开启调试信息，不优化编译代码。这对于开发和调试过程很有用。
- `Release`：不包含调试信息，开启所有优化。这用于准备最终的产品发布，因为它提供了最佳的性能。
- `RelWithDebInfo`：在 Release 模式的基础上添加调试信息，优化编译代码。这对于需要进行性能分析同时又需要调试信息的场合很有用。
- `MinSizeRel`：优化编译代码以最小化大小，不包含调试信息。这适用于对可执行文件大小有严格限制的情况。






