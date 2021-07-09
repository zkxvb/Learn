# 零碎知识点记录
1. 字符数组的注意事项
    - 赋值：必须通过strcpy或for循环等的形式来完成，不能直接使用等号来赋值

    - 比较：必须通过strcmp等函数的形式来完成，不能直接比较，直接比较的话，比较的是两个指针是否相等

2. C标准的相关问题
    - C89标准：纯C语言环境，要求局部变量定义必须在函数或局部空间的开头
    - C99标准：对变量的定义位置没有限制
    ```
    //示例1
    #include<stdio.h>
    int main()
    {
        int a = 1;
        printf("a = %d\n", a);
        int b = 2;
        printf("b = %d\n", b);

        return 0;
    }

    //示例2
    #include<stdio.h>
    int main()
    {
        int a = 1;
        int b = 2;
        printf("a = %d\n", a);
        printf("b = %d\n", b);

        return 0;
    }
    示例1的程序在纯C语言的环境下（.c文件）而且在C89标准下（如vs2012及之前）编译会报错，情况可能如下：
        - "error C2143: 语法错误：缺少 ";"(在"类型"的前面)
        - "error C2065: "b";未声明的标识符
    
    示例1的程序在C++的环境下编译会通过（C++使用的不是C89标准）
    示例2的程序在C89和C99的环境下都可以编译通过
    ```
3. 命名空间
    - 使用using声明，可以使用命名空间中的成员
    - 每个名字都需要独立的using声明
    - 头文件不应包含using声明：头文件的内容会拷贝到所有引用它的文件中去，因此可能会引起名字冲突
    ```
        #include <iostream>

        using std::cin;
        
        int main(){
            int i;
            cin >> i;
            cout << i;          //错误，没有对应的using声明，必须使用完整的名字
            std::cout << i;     //正确，显示地从std中使用cout
        }
    ```
4. 

# Windows下实践遇到的问题
1. （基于VS环境开发的）应用程序无法正常启动0xc00007b，或是“无法启动此程序，因为计算机丢失**.dll，重新安装解决此问题”
    - [参考链接](https://blog.csdn.net/VisualMan_whu/article/details/79599602)

    - 这种错误是由于缺乏对应的".dll"库导致的，找到对应的库，安装并设置好搜索路径，让程序能够找到这个库即可

    - 方法1：可以通过[Dependency](http://www.dependencywalker.com/)来查询exe可执行程序的依赖库，然后通过[Everything](https://www.voidtools.com/zh-cn/)来查找本地是否有该dll，若有，可将其复制到程序的搜索路径；若没有，可以在[Open Dll](https://www.opendll.com/index.php)中搜索该dll并安装。

    - 方法2：针对dll是Release版本的（应用程序是什么版本，dll就是什么版本），可以直接从[微软官网](https://www.visualstudio.com/zh-hans/vs/older-downloads/)的"可再发行组建和生成工具"栏目里面下载“对应的”（报错程序开发使用的开发环境）运行时库，安装即可。但对于dll是Debug版本的，需要通过方法1来解决。

2. 编译时选择Debug和Release的区别
    - 

3. 出现问题时，相关的社区可以搜索
    - [VS msdn](https://social.msdn.microsoft.com/forums/vstudio/en-us/home?category=visualstudio%2cvslanguages%2cvstfs%2cnetdevelopment%2cvsarch)
    - 

4. 解决问题时一定要控制好变量，这样才能知道出现问题的原因以及如何解决这个问题

5. 

# Linux下实践遇到的问题
1. 



--------------------------

# C++ concurrency

## 线程管理

1. Std::thread() 接受任何可调用类型

2. Using RAII to wait for a thread to complete，确保thread.join在正确的时机被调用

3. 给线程传递参数时，需要注意，std::thread接口只拷贝值，即使传一个引用；不会自动转换类型（需要我们自己提前转换好）

   ```c++
   // 如果我们想传递一个引用，并在线程中更新这个参数，需要结合 std::ref
   std::thread t(update_date_for_widget, w, std::ref(data));
   
   // 绑定类的一个成员函数
   X my_x;
   std::thread t(&X::do_something, &my_x);				--> my_x.do_something()
   ```

4. 