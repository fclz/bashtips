# Linux Shell
Use bash

*   基础介绍
*   基本变量
*   流程控制
*   字符串处理
*   变量（环境变量）
*   数学计算
*   数组（列表）
*   函数
*   文件操作
*   多线程（进程）
*   其它常用操作


## 基础介绍
*   以`.sh`为后缀名
*   以`#`为注释开头
*   会在首行指定shell脚本解析器  
    demo.sh
    ```
    #!/bin/bash
    # this file is using bash to run
    # main start

    ```

## 基本变量
Shell Script是一种弱类型语言
+ 使用变量的时候无需首先声明其类型。
+ 新的变量会在本地数据区分配内存进行存储，这个变量归当前的Shell所有，任何子进程都不能访问本地变量。
+ 这些变量与环境变量不同，环境变量被存储在另一内存区，叫做用户环境区，这块内存中的变量可以被子进程访问。
+ 变量赋值的方式是：
```
variable_name = variable_value
```

+ 如果对一个已经有值的变量赋值，新值将取代旧值
+ 变量取值的时候要在变量名前加`$`
+ `$variable_name` 可以在引号中使用，这一点和其他高级语言是明显不同的。如果出现混淆的情况，可以使用 `花括号` 来区分，例如：
```
a=hello worlds
echo "Hi, $as"
```
+ 就不会输出 `"Hi, hello worlds"` ，而是输出 `"Hi，"` 。这是因为Shell把 `$as` 当成一个变量，而 `$as` 未被赋值，其值为空。
+ 正确的方法是：

echo "Hi, ${a}s"

+ 单引号中的变量不会进行变量替换操作。 

+ 关于变量，还需要知道几个与其相关的Linux命令。
    1. env用于显示用户环境区中的变量及其取值；
    2. set用于显示本地数据区和用户环境区中的变量及其取值；
    3. unset用于删除指定变量当前的取值，该值将被指定为NULL；
    4. export命令用于将本地数据区中的变量转移到用户环境区。




## 流程控制
bash 主要有以下几种流程控制方式，和其他高级语言基本类似
+ `if else` 控制语句
+ `while` 循环
+ `for` 循环

首先介绍条件的测试语句
shell script 中通常使用 `[ ]` 来表示条件测试，下面列车部分常用的条件测试：
+ 文件 `$"$file"` 相关
    ```
    [ -a "$file" ] 如果 "$file" 存在则为真。
    [ -b "$file" ] 如果 "$file" 存在且是一个块特殊文件则为真。
    [ -c "$file" ] 如果 "$file" 存在且是一个字特殊文件则为真。
    [ -d "$file" ] 如果 "$file" 存在且是一个目录则为真。
    [ -e "$file" ] 如果 "$file" 存在则为真。
    [ -f "$file" ] 如果 "$file" 存在且是一个普通文件则为真。
    [ -g "$file" ] 如果 "$file" 存在且已经设置了SGID则为真。
    [ -h "$file" ] 如果 "$file" 存在且是一个符号连接则为真。
    [ -k "$file" ] 如果 "$file" 存在且已经设置了粘制位则为真。
    [ -p "$file" ] 如果 "$file" 存在且是一个名字管道(F如果O)则为真。
    [ -r "$file" ] 如果 "$file" 存在且是可读的则为真。
    [ -s "$file" ] 如果 "$file" 存在且大小不为o则为真。
    [ -t FD ] 如果文件描述符 FD 打开且指向一个终端则为真。
    [ -u "$file" ] 如果 "$file" 存在且设置了SUID (set user ID)则为真。
    [ -w "$file" ] 如果 "$file" 如果 "$file" 存在且是可写的则为真。
    [ -x "$file" ] 如果 "$file" 存在且是可执行的则为真。
    [ -O "$file" ] 如果 "$file" 存在且属有效用户ID则为真。
    [ -G "$file" ] 如果 "$file" 存在且属有效用户组则为真。
    [ -L "$file" ] 如果 "$file" 存在且是一个符号连接则为真。
    [ -N "$file" ] 如果 "$file" 存在 and has been mod如果ied since it was last read则为真。
    [ -S "$file" ] 如果 "$file" 存在且是一个套接字则为真。
    [ "$file1" -nt "$file2" ] 如果 "$file1" has been changed more recently than "$file2", or 如果 "$file1" exists and "$file2" does not则为真。
    [ "$file1" -ot "$file2" ] 如果 "$file1" 比 "$file2" 要老, 或者 "$file2" 存在且 "$file1" 不存在则为真。
    [ "$file1" -ef "$file2" ] 如果 "$file1" 和 "$file2" 指向相同的设备和节点号则为真。
    [ -o OPTIONNAME ] 如果 shell选项 “OPTIONNAME” 开启则为真。
    ```
+ 变量 `$var` 相关
    1. 字符串
        1. 无空格的字符串，可以加`" "`,也可以不加
        2. `=` 作为等于时，其两边都必须加空格，否则失效；等号也是操作符，必须和其他变量，关键字，用空格格开 (等号做赋值号时正好相反，两边不能有空格）
    ```
    [ -z "$var" ] $var长度为零则为真
    [ -n "$var" ] or [ "$var" ] $var 的长度为非零 non-zero则为真
    [ STRING1 == STRING2 ] 如果2个字符串相同。 “=” may be used instead of “==” for strict POSIX compliance则为真。
    [ STRING1 != STRING2 ] 如果字符串不相等则为真。
    [ STRING1 < STRING2 ] 如果 “STRING1” sorts before “STRING2” lexicographically in the current locale则为真。
    [ STRING1 > STRING2 ] 如果 “STRING1” sorts after “STRING2” lexicographically in the current locale则为真。
    ```
    2. 整数
        1. `-eq  -ne  -lt  -nt` 只能用于整数，不适用于字符串，字符串等于用赋值号 `=`
    ```
    [ ARG1 OP ARG2 ] “OP” is one of -eq, -ne, -lt, -le, -gt or -ge. 
    These arithmetic binary operators return true if “ARG1” is equal to, not equal to, 
    less than, less than or equal to, greater than, or greater than or equal to “ARG2”, 
    respectively. “ARG1” and “ARG2” are integers.
    ```
+ 逻辑相关
    1. 逻辑非 `!`    条件表达式的相反
        ```
        if [ ! 表达式 ]
        ```
    2. 逻辑与 `–a`   条件表达式的并列
        ```
        if [ 表达式1  –a  表达式2 ]
        ```
    3. 逻辑或 `-o`    条件表达式的或
        ```
        if [ 表达式1  –o 表达式2 ]
        ```
    4. 表达式与前面的 `= != -d –f –x -ne -eq -lt` 等合用;  
        逻辑符号就正常的接其他表达式，没有任何括号（ ），就是并列
        ```
        if [ -z "$JAVA_HOME" -a -d $HOME ]
        ```
        注意逻辑与 `-a` 与逻辑或 `-o` 很容易和其他字符串或文件的运算符号搞混了

    5. 以 test 条件表达式 作为 `if` 条件
        ```
        if test $num -eq 0      等价于   if [ $num –eq 0 ]
        ```
        test  表达式,没有 `[  ]`

    6. if简化语句,最常用的简化if语句  

        a. `&&` 如果是“前面”，则“后面”
        ```
        [ -f /var/run/dhcpd.pid ] && rm /var/run/dhcpd.pid    检查 文件是否存在，如果存在就删掉
        ```
        b. `||`   如果不是“前面”，则后面
        ```
        [ -f /usr/sbin/dhcpd ] || exit 0    检验文件是否存在，如果存在就退出
        ```

+ `if else` 控制语句
    ```
    if [ condition1 ] ; then
    # do someting
    elif [ condition2 ] ; then
    # do someting 
    else
    # do someting
    fi
    ```
+ `while` 循环
    ```
    while [ condition1 ] && [ condition2 ] ; do
    # do someting
    done
    ```
+ `for` 循环
    1. 形式1
    ```
    for var in $list ; do
    # do someting
    done
    ```
    2. 形式2
    ```
    for (( condition1;condition2;condition3 )) do
    # do something
    done
    ```
+ `util` 循环
    ```
    until [ condition1 ] && [ condition2 ] ; do
    # do someting
    done
    ```
+ `case` 语句
    ```
    case $var in

    pattern 1 )

    … ;;

    pattern 2 )

    … ;;

    *)

    … ;;

    esac
    ```
+ bash select 扩展  
    Bash提供了一种用于交互式应用的扩展select，用户可以从一组不同的值中进行选择
    ```
    select var in $list ; do
    break;
    done
    ```
    
## 字符串处理
+ 字符串截取（分割）
shell 字符串截取的8种方法
对于变量 `var=https://www.google.com/index.html`
    1. `#` 号截取，删除左边字符，保留右边字符。
        ```
        echo ${var#*//}
        ```
        其中 var 是变量名，`#` 号是运算符，`*//` 表示从左边开始删除第一个 `//` 号及左边的所有字符
        结果是：`www.google.com/index.html`

    2. `##` 号截取，删除左边字符，保留右边字符。
        ```
        ehco ${var##*/}
        ```
        `##*/` 表示从左边开始删除最后（最右边）一个 `/` 号及左边的所有字符  
        即删除 `https://www.www.google.com/`  
        结果是：`index.html`
    3. `%` 号截取，删除右边字符，保留左边字符
        ```
        echo ${var%/*}
        ```
        `%/*` 表示从右边开始，删除第一个 `/` 号及右边的字符  
        结果是：`https://www.www.google.com`
    4. `%%` 号截取，删除右边字符，保留左边字符
        ```
        echo ${var%%/*}
        ```
        `%%/*` 表示从右边开始，删除最后（最左边）一个 `/` 号及右边的字符  
        结果是：`https:`
    5. 从左边第几个字符开始，及字符的个数
        ```
        echo ${var:0:5}
        ```
        其中的 `0` 表示左边第一个字符开始，`5` 表示字符的总个数。  
        结果是：`https`
    6. 从左边第几个字符开始，一直到结束。
        ```
        echo ${var:8}
        ```
        其中的 `8` 表示左边第 `9` 个字符开始，一直到结束。
        结果是：`www.google.com/index.html`
    7. 从右边第几个字符开始，及字符的个数.
        ```
        echo ${var:0-10:5}
        ```
        其中的 `0-10` 表示右边算起第 `10` 个字符开始，`5` 表示字符的个数。  
        结果是：`index`
    8. 从右边第几个字符开始，一直到结束。
        ```
        echo ${var:0-10}
        ```
        其中的 `0-10` 表示右边算起第 `10` 个字符开始，一直到结束。  
        结果是：`index.html`
    9. 注：（左边的第一个字符是用 0 表示，右边的第一个字符用 0-1 表示）
+ 字符串比较
    1. `=` 等于,如：
        ```
        if [ "$a" = "$b" ] ;then
        ```
    2. `==` 等于，如
        ```
        if [ "$a" == "$b" ] ;then
        ```
        此时和 `=` 等价  
        `==` 的功能在 `[[]]` 和 `[]` 中的行为是不同的,如下：
        * ``` [[ $a == z* ]]    # 如果$a以"z"开头(模式匹配)那么将为true ```
        * ``` [[ $a == "z*" ]]  # 如果$a等于z*(字符匹配),那么结果为true ```
        * ``` [ $a == z* ]      # File globbing 和word splitting将会发生 ```
        * ``` [ "$a" == "z*" ]  # 如果$a等于z*(字符匹配),那么结果为true ```

        一点解释,关于File globbing是一种关于文件的速记法,比如 `"*.c"` 就是,再如 `~` 也是.  
        但是file globbing并不是严格的正则表达式,虽然绝大多数情况下结构比较像. 
    3. `!=` 不等于,如:
        ```
        if [ "$a" != "$b" ] 
        ```
        这个操作符将在 `[[]]` 结构中使用模式匹配. 
    4. `<` 小于,在ASCII字母顺序下.如
        ```
        if [[ "$a" < "$b" ]] 
        if [ "$a" \< "$b" ] 
        ```
        注意:在 `[]` 结构中 `"<"` 需要被转义. 
    5. `>`  大于,在ASCII字母顺序下.如
        ```
        if [[ "$a" > "$b" ]] 
        if [ "$a" \> "$b" ] 
        ```
        注意:在 `[]` 结构中 `">"` 需要被转义.
    6. `-z` 字符串为 `"null"`.就是长度为 `0`
    7. `-n` 字符串不为 `"null"`  
        注意:  
        使用 `-n` 在 `[]` 结构中测试必须要用 `""` 把变量引起来.使用一个未被 `""` 的字符串来使用 `! -z`  
        或者就是未用 `""` 引用的字符串本身,放到 `[]` 结构中。虽然一般情况下可以工作.
        但这是不安全的.习惯于使用""来测试字符串是一种好习惯.

+ 字符串长度

## 变量（环境变量）

## 数学计算
shell中的赋值和操作默认都是字符串处理，在shell种进行数学计算需要用到几个特殊的方法
+ 使用 `let`
    1. `let` 几乎支持所有的运算符，包括自加、自减、以及括号的优先级
    2. 幂次方使用 `**`
    3. 参数在表达式种自己访问，不用加 `$`
    4. 一般情况下算数表达式可以不加双引号，但是若表达式中有 `bash` 中的关键字则需要加上
    5. `let` 后的表达式只能进行整数运算
        ```
        test@localhost$ a=1
        test@localhost$ let b=a+2
        test@localhost$ echo $b
        3
        test@localhost$ let b++
        test@localhost$ echo $b
        4
        test@localhost$ let b--
        test@localhost$ echo $b
        3
        test@localhost$ let c=b**3
        test@localhost$ echo $c
        27
        test@localhost$ let c+=0.3
        bash: let: c+=0.3: syntax error: invalid arithmetic operator (error token is ".3")
        test@localhost$
        ```
+ 使用 `(())`
    此法使用方式和 `let` 完全相同
+ 使用 `$[]`
    1. `$[]` 将中括号内的表达式作为数学运算先计算结果再输出.
    2. 对 `$[]` 中的变量进行访问时前面需要加 `$`
    3. `$[]` 支持的运算符与 `let` 相同，但也只支持整数运算
        ```
        test@localhost$ d=$[$a+3]
        test@localhost$ echo $d
        4
        test@localhost$
        ```
+ 使用 `expr`
    1. `expr` 后的表达式个符号间需用空格隔开
    2. `expr` 支持的操作符有：`|、&、<、<=、=、!=、>=、>、+、-、*、/、%`
    3. `expr` 支持的操作符中所在使用时需用\进行转义的有：`|、&、<、<=、>=、>、*`
    4. `expr` 同样只支持整数运算
        ```
        test@localhost$ var=$(expr $d + 100)
        test@localhost$ echo $var
        104
        test@localhost$
        ```
+ 使用 `bc`
    bc是linux下的一个简单计算器，支持浮点数计算，在命令行下输入bc即进入计算器程序，  
    而我们想在程序中直接进行浮点数计算时，利用一个简单的管道即可解决问题
    1. `bc` 支持除位操作运算符之外的所有运算符,具体可以 `man bc`
    2. `bc` 中要使用 `scale` 进行精度设置
        ```
        test@localhost$ pi=3.14159
        test@localhost$ r=3
        test@localhost$ s=$(echo "$pi*$r*$r" | bc)
        test@localhost$ echo $s
        28.27431
        test@localhost$
        ```
+ 使用 `awk`
    `awk` 是一种文本处理工具，同时也是一种程序设计语言，作为一种程序设计语言，`awk` 支持多种运算，  
    而我们可以利用 `awk` 来进行浮点数计算，和上面 `bc` 一样，通过一个简单的管道，我们便可在程序中直接调用 `awk` 进行浮点数计算。
    1. `awk` 支持除微操作运算符之外的所有运算符
    2. `awk`内置有 `atan2,cos,exp,int,log,rand,sin,sqrt,srand` 等等函数
        ```
        test@localhost$ s=$(echo "$pi $r" | awk '{printf("%g",$1*$2*$2)}')
        test@localhost$ echo $s
        28.2743
        test@localhost$
        ```
## 数组（列表）
+ Array
    Bash shell只支持一维数组，初始化时候不需要定义数组的大小，默认下标从0开始。
    1. 语法格式
        用括号表示，元素用`空格`分割开。
        ```
        name_array=(hello world)
        ```
    2. 赋值
        ```
        # 整体赋值
        name_array=(hello world)
        # 单个元素赋值
        name_array[0]=hello
        name_array[1]=world
        ```
    3. 读取数组元素
        ```
        #获取第 `n` 个元素
        ${name_array[n]}
        ```
    4. 读取所有元素
        使用@ 或 * 可以获取数组中的所有元素
        ```
        # 使用 `@`
        echo "array number list:${name_array[@]}"
        # 使用 `*`
        echo "array number list:${name_array[*]}"
        ```
    5. 获取数组长度
        类似于获取字符串长度，首先获取所有元素，然后再获取其长度
        ```
        echo "array size:${#name_array[@]}"
        echo "array size:${#name_array[*]}"
        ```
    6. 实例Demo
        ```
        #!/bin/bash
        name_array=(hello world)
        echo "first:${name_array[0]}
        echo "second:${name_array[1]}
        name_array[1]=world!
        echo "all:${name_array[@]}"
        echo "all:${name_array[*]}"
        echo "array size:${#name_array[@]}"
        echo "array size:${#name_array[*]}"

        array_list=(zero first second third forth fifth)
        echo "array list:${array_list[*]}"
        # 从下标 `2` 开始赋值，前两个下标为空；
        array_list=([2]=second third forth fifth)
        echo "array_list[0]:${array_list[0]}"
        echo "array_list[1]:${array_list[1]}"
        echo "array_list[2]:${array_list[2]}"
        echo "array_list[3]:${array_list[3]}"
        echo "array_list[4]:${array_list[4]}"
        echo "array_list[5]:${array_list[5]}"
        # 数组的长度是实际内容的长度，不是指下标的长度
        echo "array_list size:${#array_list[*]}"
        echo "array_list:${array_list[*]}"
        # 采用 `for` 循环获取数组非空元素
        for i in ${array_list[*]}
        do
            echo $i
        done
        # 同样可以指定任意元素的下标，不分先后
        array_lis=([5]=fifth [1]=first [0]=zero [3]=third)
        ```

+ Array 其它用法
    1. 字符串操作  
        数组可以使用字符串操作的操作符，意义一致；唯一的不同在于*所有的操作都是针对所有数组元素逐个进行的*，可以这么理解
        > 数组是字符串的一个特例，数组中的每个元素都相当于字符串中的一个字符。


## 函数
shell 可以用户定义函数，然后在shell脚本中可以随便调用
+ 定义函数
    语法
    ```
    [ function ] function_name [()]
    {
        do someting;
        [ return int;]
    }
    ```

    1. 可以带 `function function_name()` 定义，也可以直接 `function_name()` 定义，不带任何参数
    2. 参数返回，可以显示加：`return ret` ,如果不加，将以最后一条命令运行结果，作为返回值。`return` 后跟数值 `int(0-255)`
    3. 必须在调用函数地方之前，声明函数, shell脚本是逐行运行
    4. 函数参数可以使用：`$0,$1,...,$n` 得到，其中 `$0` 代表函数本身
    5. 调用函数只需给出函数名称即可，不需要加括号；
    6. 函数传递参数只需要在调用函数的函数名称后面，以空格隔开接上即可
    7. 函数可以显示通过 `return` 返回 `0-255` 的整数，如果返回其它值，会得到错误提示：`numeric argument required`
    8. 函数的返回值只能通过 `$?` 获取，必须紧跟在函数调用之后。
    9. 如果需要获取函数隐式的返回值（非 `0-255` 整数），即最后一个命令的返回值，可以使用变量赋值方式，将函数作为一个命令赋值给变量。可参照如下 `Demo`
        ```
        #!/bin/bash
        function int_add()
        {
            return $(expr $1 + $2)
        }
        function float_add()
        {
            echo "$1+$2" | bc
        }
        # 返回0-255整数的函数，可以通过$?获取函数return返回值
        int_add 3 4
        int_sum=$?
        # 如果返回的不是0-255的整数值，无法通过return返回。
        # 可选的方式如下，通过echo命令输出，在通过shell命令方式把输出“接住”
        float_sum=$(float_add 2.4 3.5)
        echo "integer sum 3+4=$int_sum"
        echo "float sum 2.4+3.5=$float_sum"
        ```

+ 函数作用域，变量作用范围
    1. shell脚本中定义的变量是global的，其作用域从被定义的地方开始，一直到shell结束或者被显示删除的地方为止。
    2. 函数定义的变量可以被显示定义成local的，其作用域局限于函数内。但请注意，函数的参数是local的。
    3. 函数定义的变量默认是global的，其作用域从“函数被调用时执行变量定义的地方”开始，直到shell结束为止。注意，不是从定义函数的地方开始，而是从调用函数的地方开始
    4. 如果同名，Shell函数定义的local变量会屏蔽脚本定义的global变量。
        ```
        #!/bin/bash

        ```

+ 函数的返回值
    函数的返回值在很多地方都会用到，除了在前面提到的几点外，还可以使用下面几种方式。
    1. return 语句
        1. 函数可以显示通过 `return` 返回 `0-255` 的整数，如果返回其它值，会得到错误提示：`numeric argument required`
        2. `return` 返回值必须通过 `$?` 获取，紧跟在函数调用点之后
    2. 全局变量（环境变量）
        1. 函数里面通过修改全局变量方式返回值
        2. 函数调用后，可以在外部获取该全局变量
        3. 全局变量在子进程中被修改，子进程的修改是无法反应到父进程中，需特别注意。特别是使用到管道命令时。
    3. echo 返回值
        1. 通过命令 `echo` 作为最后一个命令，将返回值输出到控制台。这样就可以通过 `shell` 命令方式接住控制台输出。
        2. 需要将多余的输出到控制台的信息重定向到 `/dev/null`，防止出现不必要的结果。


## 文件操作
文档部分操作涉及很多，包含很多操作

## 多线程（进程）

## 其它常用操作
+ find 命令
+ tar 命令
    1. 5个独立命令
        - -c 建立压缩档案
        - -x 解压档案
        - -t 查看档案内容
        - -r 向文档末尾增加文件，同名不覆盖
        - -u 更新原压缩包文件；同名覆盖；若不存在，则追加
    2. 可选参数
        - -z：有gzip属性的
        - -j：有bz2属性的
        - -Z：有compress属性的
        - -v：显示所有过程
        - -O：将文件解开到标准输出
    3. 必选参数  
        -f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。
+ grep 命令
+ cut 命令

