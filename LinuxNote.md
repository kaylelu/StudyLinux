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


