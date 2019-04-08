# Cmake学习
1. [参考链接](https://www.hahack.com/codes/cmake/)
2. [官方文档](https://cmake.org/cmake/help/cmake2.4docs.html)
3. 预定义选项的使用
    - 使用编译选项时，ccmake设置预定义变量，使用上下键选择选项，使用enter键来修改选项的值（t按键不知道是干什么的）
    - 通过“cmake -i”成功设置预定义变量
    - 通过“cmake -DUSE_MYMATH=OFF”也成功设置预定义变量
4. 添加额外的库
```
    # C语言在使用头文件math.h时，需要添加静态库libm.a
        - gcc方式：gcc -lm test.c
        - cmake方式：target_link_libraries(Demo m)  //Demo是生成的可执行程序，m是静态库
    
```
5. 支持GDB
```
    # 让CMake支持gdb，只需要指定Debug模式下开启-g选项
        set(CMAKE_BUILD_TYPE "Debug")       //默认CMAKE_BUILD_TYPE值为Debug，每种模式(Debug，Release ...)都有其各自的选项
        set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")      //Debug模式下的编译选项
        set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -03 -Wall")     //Release模式下的编译选项
```