shell脚本可以实现自动化批量作业，减少人为参与，其底层是用C实现的，shell脚本是人与系统交互的语言，一般用来处理各种文件，处理管道等工作
### 输出
```shell
#! /bin/bash
echo "Hello World"
```

### 变量
```shell
#! /bin/bash
name=hello
echo $name #等价于echo ${name}
echo ${#name} #等价于cout << name.size();
echo ${name:0:3} #输出下标志0开始的三个字符
```
### 文件参数变量（类似于输入）

```$0,$1,$2```等等这些是你传给脚本的参数,$0是当前脚本名
```$# ```是脚本名后面传入参数的个数
```$?``` 是上一条命令返回值,是return状态
```$(command)```是返回命令输出值，是stdout
```'command'```和```$(command)```是一样的结果

### 数组
```shell
#! /bin/bash
array=(1 2 3 4)
echo ${array[0]}
echo ${array[*]} #输出全部内容
echo ${array[@]} #输出全部内容
echo ${array[*]} #输出数组长度,如果是${#array}的话输出1
```

### expr
表达式用空格隔开，expr命令执行stdout结果，在`exit code`中也反馈结果

**1.字符串表达式**
```shell
#! /bin/bash
str="Hello World"
echo $(expr length "$str")
#这里$str外面加双引号是将其作为一个整体传入，否则str内容中空格后面是新的参数
echo $(expr index "$str" aWd) #查询aWd中任意一个字符在字符串中出现的第一个下标
echo 'expr substr "$str" 1 2' #这里是指下标为1的情况下，输出从下表1开始长度为2的子串
```
**2.整数表达式**
```shell
#! /bin/bash
a=3
b=4

echo `expr $a + $b`  # 输出7
echo `expr $a - $b`  # 输出-1
echo `expr $a \* $b`  # 输出12，*需要转义
echo `expr $a / $b`  # 输出0，整除
echo `expr $a % $b` # 输出3
echo `expr \( $a + 1 \) \* \( $b + 1 \)`  # 输出20，值为(a + 1) * (b + 1)，这里的括号需要转译
```

### read
相当于cin
```shell
#! /bin/bash
read name
echo ${name}

read -p "What;s your name?" name #-p后面接提示信息\
read -t 5 name #5s时限输入，否则自动跳过
#注意这个name要放在最后，如果先name然后再-t 5是不符合语法的
```
### echo
```shell
echo -e Hello World '\c' #不换行输出
echo Bochi

echo "Hello" > output.md #将输出结果重定向到文件里
```

### printf
几乎和C/C++的一模一样
```shell
printf "%10d.\n" 123  # 占10位，右对齐
printf "%-10.2f.\n" 123.123321  # 占10位，保留2位小数，左对齐
printf "My name is %s\n" "yxc"  # 格式化输出字符串
printf "%d * %d = %d\n"  2 3 `expr 2 \* 3` # 表达式的值作为参数
```
### 逻辑运算符
利用短路功能来实现if判断

### test命令
 [test命令与判断符号[]](https://www.acwing.com/blog/content/9704/) 
expr是stdout，用1表示真
test是用exit code返回，所以用0来表示真

### 判断语句和case语句
if-else语句
```shell
if condition
then
    语句1
    语句2
    ...
fi
```
```shell
if condition
then
    语句1
    语句2
    ...
else
    语句1
    语句2
    ...
fi
```

case语句
```shell
case $变量名称 in
    值1)
        语句1
        语句2
        ...
        ;;  # 类似于C/C++中的break
    值2)
        语句1
        语句2
        ...
        ;;
    *)  # 类似于C/C++中的default
        语句1
        语句2
        ...
        ;;
esac
```
在shell脚本中，使用-eq、-ne、-gt、-ge、-lt、-le进行整数的比较。英文意思分别为：
`-eq` ：equal（相等）
`-ne` ：not equal（不等） 
`-gt` ：greater than（大于）
`-ge` ：greater than or equal（大于或等于）
`-lt` ：less than（小于）
`-le` ：less than or equal（小于或等于）

### 循环语句
**for**
```shell
#! /bin/bash
for i in 1 2 3 4
do
    echo $i
done
```

输出n in [x,y]
`{x..y}`代表一个序列，由[min,max]的这样一个序列

```shell
for i {1..20} #这里如果是{0..9}就会输出0到9

for i in $(seq 1 10)

for ((i = 0;i < n;i ++))

for i in $(ls)
```
**while**
```shell
while condition
do
    语句1
    语句2
    ...
done
```
until...do...done
until获得想要的答案才结束循环，yxc说这类似npy直到你认错才原谅你，而现实中的npy可比此复杂多了
```shell
#! /bin/bash
until [ "$name" == "Jack" ]
do
    read -p "input your name:" name
done
```
continue和C/C++的一样

### 函数
shell的return是0~255之间的数字
```shell
[function] func_name() {  # function关键字可以省略
    语句1
    语句2
    ...
}
```
递归求斐波那契
```shell
#! /bin/bash
f()
{
    if [ expr $1 -le 0 ]
    then
        echo 0
        return 0
    fi
    
    n=$(f $(expr $1 - 1))
    echo $(expr $n + $1)
    #相当于return n + f(n - 1),不过这里的f(n - 1)提前算出来传给n而已
}
echo $(f $1)
```
### exit
exit code

### 文件重定向
```shell
ls > output
```
`>`直接覆盖文件
`>>`追加方式重定向

### 引入外部脚本
```shell
. filename  # 注意点和文件名之间有一个空格

或

source filename
```
