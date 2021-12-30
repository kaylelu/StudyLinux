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

    变量可以提升为全局，供其他Shell程序使用

    ```
    ```

    

  * set命令可以显示shell的所有变量

* 变量命名规则
  * 字母、数字、下划线，不能以数字开头
  * 等号两边不能有空格
  * 变量名称一般用大写
* 将命令的返回值赋值给变量
  * A=\`ls /home\` 用两个\`包裹就好，\`这个符号是英文输入法下的esc下面的键，反引号
  * A=$(ls /home)

