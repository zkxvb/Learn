# 零碎知识点记录
1. 字符数组的注意事项
    - 赋值：必须通过strcpy或for循环等的形式来完成，不能直接使用等号来赋值

    - 比较：必须通过strcmp等函数的形式来完成，不能直接比较，直接比较的话，比较的是两个指针是否相等

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