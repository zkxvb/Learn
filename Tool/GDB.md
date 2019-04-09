1. [参考链接](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/gdb.html)
2. 设置系统生成coredump文件
    - 命令ulimit
    ```
        ulimit -a       //查看系统所有文件的限制大小
        ulimit -c [kbytes]      //设置系统允许生成的core文件大小
        ulimit -c unlimited     //不限制core文件的大小

        直接在命令行输入命令，修改core文件的大小，只在当前shell中生效，系统重启后失效
        在文件 /etc/profile 中添加上述命令（ulimit -c unlimited），并重新读入该配置文件（source /etc/profile）即可永久生效
    ```