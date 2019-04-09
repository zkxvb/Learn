# apue库的安装
1. [参考链接](https://www.cnblogs.com/cherishry/p/6294549.html)
2. [源代码网址](http://www.apuebook.com/code3e.html)
3. 安装流程：
    - 下载源代码
    - 修改代码中对应头文件的位置：apue.3e/threadctl/getenv1.c 和 apue.3e/threadctl/getenv3.c 中头文件 limits.h 的位置
    - 安装所需的包：libbsd 和 libbsd-devel
    - 执行make生成静态库libapue.a
    - 将libapue.a拷贝到链接器的搜索路径，如 /usr/local/lib
    - 将头文件apue.h拷贝到链接器的搜索路径，如 /usr/include 
4. 编译程序时，要使用“-lapue”选项