# VS_Code内置Git的使用
- Git的命令使用：在VS_Code中使用组合键 "ctrl + `" 即可在下方调出win命令窗口（终端），这这个窗口里面正常使用Git命令即可

# VS_Code搭建C/C++编译器
1. [参考链接](https://www.jianshu.com/p/a0ae073e973b?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

2. [Vscode官网参考链接](https://code.visualstudio.com/docs/languages/cpp)

3. 需要的插件：
    - “C/C++”，该插件提供C/C++语言支持，简单的编辑，编译，调试等功能
    - “Run In Terminal”，程序的输出，运行是在内嵌的Terminal中，而不是在OUTPUT窗口中，可以进行输入操作

4. 额外的工具：MinGW-W64，注意不是MinGw，这个已经停止维护了，有很多坑 ，安装MinGW-W，并添加系统环境变量path

5. 添加配置文件:
    - take.json，用来build项目；打开命令面板(ctrl+shift+p)，输入tasks，选择Tasks:Configure Task，配置task.json，可参照vscode官网来写配置文件
    - launch.json，用来debug代码；最左边的一栏有个调式按钮(小虫子)，进入调式窗口，点击设置按钮，添加launch.json配置文件，详细配置可参照vscode官
    网

# Vs_Code使用
1. 更改主题快捷键：首先ctrl+k, 然后ctrl+t
2.


