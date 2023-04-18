<!--
 * @Author: yangshaoqing yangshaoqing@haomo.ai
 * @Date: 2023-04-18 14:06:37
 * @LastEditors: yangshaoqing yangshaoqing@haomo.ai
 * @LastEditTime: 2023-04-18 14:23:50
 * @FilePath: /dpc/home/dpc/Documents/find_core.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# 查找core步骤
## 前期准备
1. 车上产出对应的debug版本产出
2. 集成环境&&docker qnx 镜像
## 关键步骤
### 进集成qnx docker
``` 
./docker.sh -qnx -R -i
```
### 还原车上环境 
将debug版本产出解压到集成目录，并将core文件放在集成文件夹下
### 进gdb,加载core文件
1. cd到debug版本的output文件夹下，进gdb（此过程会卡住，直接ctrl+c，进gdb，进入gdb的标志是命令行最前面有（gdb））
```
./gdb_qnx.sh
```
2. 加载core文件（此处注意自己的core文件目录）
```
core-file 自己的文件目录/node.core
```
### 查看core的几个关键步骤
1. 查看所有线程，找出异常线程
```
info threads
```
2. 进入异常线程，比如129线程
```
thread 129
```
3. 打开当前线程堆栈
```
bt
```
4. 进入打印出的堆栈里的具体调用函数（比如堆栈2，此处建议0往后看，越小调用越低层，可以对着最近的修改，找到问题）
```
f 2
```
5. trick
```
i locals ## 打印当前所有变量
```

