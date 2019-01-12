# VS_Code内置Git的使用
- Git的命令使用：在VS_Code中使用组合键 "ctrl + `" 即可在下方调出win命令窗口（终端），这这个窗口里面正常使用Git命令即可

# VS_Code搭建C/C++编译器
1. [参考链接](https://www.jianshu.com/p/a0ae073e973b?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

2. 需要的插件：“C/C++”，该插件提供C/C++语言支持，简单的编辑，编译，调试等功能

3. 额外的工具：“MinGW（Minimalist GNU for Windows）”，因为Windows没有GNU工具包，安装好后需要设置Path环境变量，完成这一步骤时，若VS_Code开启，则需要重启VS_Code

    |包|说明|
    |:-:|:-:
    |mingw32-gcc.bin |GNU C 编译器
    |mingw32-gcc-g++.bin | GNU C++ 编译器
    |mingw32-gdb.bin | GNU 调试器

4. 添加配置文件:
    - 新建C/C++源文件，编写一个简单的程序，按下生成快捷键 "ctrl+shift+B"，第一次生成时,VS_Code会提示找不到生成的配置文件take.json，点击上方提示 "配置生成任务"
    - 


