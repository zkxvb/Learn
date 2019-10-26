# 2019-09-20
1. Mse算子取消分段对齐
  - 申请CT资源时，以行为单位比较方便处理c补齐的问题，及findlimit返回的是多少个c
  - writeConst指令写CT时，以行为单位; 使用for循环，结合storeGreg指令，可以写ram，而且不以行为单位，主义storeGreg的offset参数是以字节为单位
2. Mse算子output的shape与input的shape不一致
  - 当ouput的shape设为（1，1，1，1）时，结果出错，一直是65536
  - 解决：在申请CT资源时，要使用output自己的shape参数来申请，不要使用input的shape参数

# 2019-09-24
1. Mse operator（算子）support input and output with different shape cause bug
- 写越界导致的bug，写越界导致修改了别的变量的值 
```
  NG: src/kernel/MseOp.cpp
  auto total_size = layer().getData("INPUT").getShape().num();  // error, total_size 会传给下面的ioStoreGreg指令，用来将result_reg置0
  auto total_size = layer().getData("OUTPUT").getShape().num(); // correct
  ioStoreGreg(result_reg, 0, total_size);
  # 上述指令使用的total_size不对，是从input中得到的，当input与output的shape相同时，没什么影响
  # 当input与output的shape不同时，就会出现bug: total_size的大小可能超过output的大小，导致写入
  # 的0值改变了其他变量的内存，使得其他变量的值发生了改变。
  # 在当前情况中，使得input的数值发生了改变，前一段数值都变为了0，导致计算结果错误

  Sopa: core/src/ops/mse.cpp 
  auto count = inputVec.num();
  for(int i = 0; i < count; i++) {      // count of input
    ...;
    if (mse_param_ == MseParam::none) {   // first, mse_param_ = MseParam::sum
      ...;
    } else {
      ...;
      output[i] = 0;          // init output to 0
    }
  }
  # 语句 output[i] = 0 没有进行范围检查，循环使用的count是从input得到的，如果output的size小于
  # input的size，就会导致output写越界，把其他变量的内存置为0，具体何时显现出错误的结果，要看
  # malloc申请的地址空间。
  # 在当前的情况中，在循环1130000次时，发现变量mse_param_的值被改变了，进入了错误的逻辑，导致
  # 结果出错
```
2. L1loss: How to use active fuction
```
  NG: src/kernel/L1LossOp.cpp;
  loadTable(): load active table and const table;
  激活函数的原理就是将一段曲线切分，用直线来逼近曲线; 每段直线可以表示为y = kx + b;
  k is the active table, b is the const table;

  active指令，将数据结合激活表和常数表，得到对应的结果；x为输入，y为输出
  注意：
    在使用指令的时候，要弄清楚每个参数的意思，在实现abs的激活函数时，就是弄错了active最后一个
    参数(is_relu)的含义导致结果一直错误。

  激活表和常数表的配置是在sopa层面完成的，ng层面通过loadTable导入相应的激活表和常数表
  然后调用active指令，即可完成激活函数的运算

  关于激活表使用的内存：在实现l1loss算子时，遇到了激活表Read0越界的错误
    详情参考1M指令文档；
    激活表按照128字节对齐，在实现时为激活表申请的CT资源太小了，导致激活表Read0错误
```

# 2019-09-26
1.Conv operator 
- Attention of use declareReg
```
  the "Name" of the Reg which we declare must be different in the .cpp file.
  it means the first argument of the declareReg() function cannot repeat.
  if reg have the same name with before, the later reg's value will "Rewrite" the former reg's value.
```
- make sure you have a correct branch, otherwise it's possible that your work cannot get a right result even you on the right way.
- Pipeline Error
```
  流水线UL和L导致的错误：stage Tree
    要学习stage Tree的构造，才不会出现这个问题
    对于UL节点，loopBegin不会新建一个节点，会附加在一个L节点上
    对于L节点，loopBegin会新建一个节点
    由于stage Tree对于L节点和UL节点的处理不同，所以当之前的指令都是UL属性时，如果新写一条L属性的指令，就会导致错误，具体原因要学习stage Tree
```
- 指令操作尤其要注意数据类型，因为实现很底层，编译器不会对数据类型自动转换，需要编程人员自行考虑
```
  DDR中数据类型是Float32，若直接将该数据直接load到一个Int类型的寄存器中，编译器会按照Int类型的来解释Float32的数据，导致错误
  Float32的数据按照IEEE32标准存储在内存中，需要使用scalarUnary指令将数据类型转换成INT类型，然后load到Reg中
```
 
# 2019-10-2 command: expect
```
  expect: used to deal with auto interaction in scripts
  usage example:
    expect -c " 
      set timeout -1; 
      spawn scp -r filename root@10.2.5.251:~/zkx
      expect {
        \"*password*\" {send \"hello123\r\"}              # \r: must have, it same as the enter key when you execute you command
        \"yes/no\" {send \"yes\r\"; exp_continue;}        # exp_continue: must have
      }
      expect eof"
```






9. summory
- 阅读代码的逻辑，可以从标志位开始，结合标志位，将代码的逻辑理清楚
- 可以使用使用GDB单步调试跟着程序走一遍，理清代码的逻辑
- 算子调试总结
```
  1. 检查流水线是否按照LCS排序，对应文件：nglogautosavec20
  2. 检查指令的操作数类型是否正确，对应文件：insts/int_inst_ngpf.cfg
  3. 打开GBLOG，查看实际的数据‘
  4. 发生段错误，产生Core文件，使用core文件解析工具分析，查看第几条指令出什么错：core_parse.py
  5. nglogautosavec20文件还可以检查数据块，检索 "] "，可以找到相应的数据结构
  6. 在sopa的build目录下建立data目录，执行cnmlTest时会自动将数据，以及摆数的log存储在该目录中
```
