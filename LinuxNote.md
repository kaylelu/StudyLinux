# 第37小节 权限管理
* 修改权限-chmod
  基本说明：通过chmod指令，可以修改文件或者目录的权限
  使用方式：u表示user(所有者)\g表示group(所有组)\o表示other(其他)\a表示all(全部)
    1. 使用+、-、=方式来变更
        * chmod u=rwx,g=rx,o=x file
        * chmod o+w file
        * chmod a-x file
    2. 使用数字变更  r=4,w=2,x=1
        * chmod 751 file
        * chmod 753 file
        * chmod 642 file


* 修改文件所有者-chown
  基本说明：通过chown指令，可以修改文件或者目录的所有者
    1. 改变所有者
        * chown newowner file
    2. 改变所有者和所在组
        * chown newowner:newgroup file
    3. 递归改变子文件和目录
        * chown -R newowner file


* 修改文件所有组-chgrp
  基本说明：通过chgrp指令，可以修改文件或者目录的所有组
    1. 改变所有组
        * chgrp newgroup file
    2. 递归改变子文件和目录
        * chgrp -R newgroup file



# 第39小节 任务调度基本说明

* Linux可以定时的调度我们的脚本或者代码-crontab
    1. -e 编辑定时任务
    2. -l 查询定时任务
    3. -r 删除当前用户的所有定时任务
    
* 简单使用
    ```
    crontab -e
    执行上一条指令后会打开一个编辑器，输入如下指令
    */1 * * * * ll /etc/ >> /tmp/to.txt
    注意1后面的*中间的空格
    然后每隔一分钟/tmp/to.txt文件就会追加一个ll /etc/的内容了
    ```
    
* 5个占位符的说明
    |项目|含义|范围|
    |:---:|:---:|:---:|
    |第一个|一小时的第几分钟|0-59|
    |第二个|一天中的第几个小时|0-23|
    |第三个|一个月中的第几天|1-31|
    |第四个|一年当中的第几个月|1-12|
    |第五个|一周当中的星期几|0-7（0和7都表示星期日）|
    
* 占位符的特殊符号说明
    |特殊符号|含义|
    |:---:|:---:|
    |\*|代表任何时间。比如第一个的\*就代表每个小时的每分钟都执行|
    |,|代表不连续时间。比如0 2,6,8 \* \* \*就代表每天2点0分、6点0分、8点0分都执行一次|
    |-|代表连续时间。比如0 5 \* \* 1-6就代表周一至周六的5点0分执行一次|
    |\*/n|代表每隔多久执行一次。比如\*/1 * * * * *就代表每隔1分钟执行一次|

* 增强使用

  ```
  1.先在home目录下创建一个task.sh
  touch task.sh
  2.在task.sh内写入cal >> /tmp/mycal
  vim task.sh
  3.给task.sh添加执行权限
  chomd u+x task.sh
  4.执行crontab -e
  5.写入*/1 * * * * /home/task.sh
  6.每隔一分钟，/tmp/mycal里面就会加入当前的日历了
  可以使用tail -f /tmp/mycal查看更新
  ```




# 第58小节 Shell快速入门

* 第一行#!/bin/bash
* 命名规范：file.sh
* 执行方式
  * 相对路径：./file.sh
  * 绝对路径：/文件路径/file.sh
* 必须添加可执行权限才能执行
* 也可以用sh + 文件的方式执行

```
#!/bin/bash
echo "helloworld"
```



# 第59小节 Shell的变量

* Shell变量的介绍

  * 系统变量：$HOME、$PWD、$SHELL、$USER等

    $HOME:当前用户的home目录`echo $HOME`

    $PWD:当前文件所在的目录`echo $PWD`

    $SHELL:Shell的目录`echo $SHELL`

    $USER:当前用户名称`echo $USER`

  * 用户自定义变量：直接用名称就行

    普通变量直接使用就行，unset撤销

    ```
    A=100
    echo "A=$A"
    unset A
    echo "A=$A"
    ```

    静态变量不能unset，使用readonly修饰，不能撤销

    ```
    readonly A=99
    ehco "A=$A"
    unset A
    ehco "A=$A"
    ```

    变量可以提升为全局，供其他Shell程序使用（即第60节 环境变量）

    ```
    修改/etc/profile,增加如下语句
    MYNAME="LUTAO"
    exprot MYNAME
    修改后要生效可以source /etc/profile或者重启机器
    echo "MYNAE=$MYNAME"
    ```

  * set命令可以显示shell的所有变量

* 变量命名规则
  * 字母、数字、下划线，不能以数字开头
  * 等号两边不能有空格
  * 变量名称一般用大写

* 将命令的返回值赋值给变量
  * A=\`ls /home\` 用两个\`包裹就好，\`这个符号是英文输入法下的esc下面的键，反引号
  * A=$(ls /home)

* 注释

  ```
  #单行注释
  : '
  冒号 空格 单引号开始
  多
  行
  注
  释
  开始和结束必须独占一行
  单引号结束
  '
  ```
  



# 第61小节 位置参数变量

* $n

  * n为数字，$0表示命令本身，$1-$9表示第一到第九个参数，第10个以上的要用${n}，比如${10}

* $*

  * 这个变量代表命令行中的所有参数，把所有参数看成一个整体

* $@

  * 这个变量代表命令行中的所有参数，但是把参数区别对待

* $#

  * 这个变量代表命令行中的参数个数

  ```
  #!/bin/bash
  ehco "$0 $1 $2"
  ehco "$*"
  ehco "$@"
  ehco "$#"
  ```

  

# 第62小节 预定义变量

即Shell设计者事先定义好了的变量

* \$\$
  * 当前进程的进程号
* $!
  * 后台运行的最后一个进程的进程号
* $?
  * 最后一次执行的命令的返回状态，0为成功，其他为失败。失败代码可自定义



# 第63小节 预算符

* $((运算式)) 或者 $[运算式]

* expr m + n 

  * 注意运算符间要用空格
  *  +，-，\\*，/，%
  * 注意乘号前面的反斜杠

  ```
  #!/bin/bash
  RESULT1=$[(1+2)*3]
  echo "$RESULT1"
  RESULT2=$(((1+2)*3))
  echo "$RESULT2"
  TEMP=`expr 1 + 2`
  RESULT3=`expr $TEMP \* 3`
  echo "$RESULT3"
  RESULT4=$[$1*$2]
  echo "$RESULT4"
  ```

  

# 第64小节 条件判断

* 基本语法

  [ 条件 ]  一定主要条件前后的空格

  非空返回true，可以用%?验证，0位true，其他为false

* 常用判断语句
  * 整数比较
    * = 字符串比较
    * -lt 小于
    * -le 小于等于
    * -eq 等于
    * -gt 大于
    * -ge 大于等于
    * -ne 不等于
  * 文件权限判断
    * -r 可读
    * -w 可写
    * -x 可执行
  * 文件类型判断
    * -f 文件存在且是常规文件
    * -e 文件存在
    * -d 文件存在且是一个目录

* 应用实例

  * [ atguigu ]					返回true
  * \[\]                                   返回false
  * [ 条件 ] && ehco OK || echo NotOk   条件满足则执行第一条语句

  ```
  #!/bin/bash
  if [ "OK" = "OK" ]
  then
      echo "equal"
  fi
  
  if [ 23 -ge 22 ]
  then 
      echo "大于等于"
  fi
  
  if [ -e /opt/helloworld.sh ]
  then
      echo "文件存在"
  fi
  ```



# 第65小节 流程控制if

* 基本语法1

  ```
  if [ 条件 ];then
  	程序
  fi
  ```

* 基本语法2

  ```
  if [ 条件 ]
  then
  	程序
  elif [ 条件 ]
  then 
  	程序
  fi
  ```

```
#!/bin/bash
if [ 34 -gt 33 ];then
    echo "34大于33"
fi

if [ 34 -gt 33 ]
then
    echo "34大于33"
fi
```



# 第66小节 流程控制case

* 基本语法

  ```
  case 变量 in
  "值1")
  	程序1
  ;;
  "值2")
  	程序2
  ;;
  *)
  	程序3
  ;;
  esac
  ```

```
#!/bin/bash
case $1 in
"1")
	echo "周一"
;;
"2")
	ehco "周二"
;;
*)
	echo "其他"
;;
esac
```



# 第67小节 流程控制for

* 基本语法1

  ```
  for 变量 in 值1 值2 值3
  do
  	程序
  done
  ```

* 基本语法2

  ```
  for((初始值;控制条件;变量变化))
  do
  	程序
  done
  ```

```
#!/bin/bash
for i in "$*"
do
	echo "变量i=$i"
done

for j in "$@"
do
	echo "变量j=$j"
done

read -p "请输入累加值" N
SUM=0
for((k=0;k<=N;k++))
do
	SUM=$[$SUM+$k]
done
echo "累加结果为$SUM"
```



# 第68小节 流程控制while

* 基本语法

  ```
  while [ 控制条件 ]
  do
  	程序
  done
  ```

```
#!/bin/bash
SUM=0
i=0
read -p "请输入累加值" N
while [ $i -le $N ]
do
	SUM=$[$SUM+$i]
	i=$[$i+1]
done
echo "累加结果为$SUM"
```



# 第69小节 从控制台读取

* read [选项] (参数)
* 选项
  * -p 指定读取时的提示符
  * -t 指定读取等待时间

```
#!/bin/bash
read -p "请输入一个数" NUM
echo "您输入的为$NUM"
read -t 5 -p "请输入另一个数" NUM2
echo "您输入的为$NUM2"
```



# 第70小节 系统函数

* basename

  返回完整字符串(路径)最后一个/以后的部分，如果带了后缀则不显示后缀部分字符串

  ```
  #!/bin/bash
  basename /opt/helloworld.sh
  basename /opt/helloworld.sh sh
  basename /opt/helloworld.sh .sh
  ```

* dirname

  返回完整字符串(路径)最后一个/以前的部分

  ```
  #!/bin/bash
  dirname /opt/helloworld.sh
  ```

  

# 第71小节 自定义函数

* 基本语法

  ```
  function 函数名 ()
  {
  	程序
  	return 返回值;   #返回值可以省略
  }
  函数的返回值通过$?的方式读取
  ```

```
#!/bin/bash
fuction GetSum()
{
	SUM=$[$1+$2]
	return $SUM;
}

SUM=$(GetSum(1 2))
echo "$SUM"function GetSum()
{
	SUM1=$[$1+$2]
    echo "$SUM1"
	return $SUM1;
}
GetSum 1 2
SUM=$?
echo "$SUM"
```

