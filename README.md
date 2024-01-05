personal user
《第一步》使用脚本：
1. 使用WinSCP上传脚本文件到root目录下
2. 设置脚本文件权限 0755或0777

《第二步》权限设置：
1. 使用WinSCP软件到root目录下
2. 选择脚本文件，鼠标右键->属性，八进制表=0755或0777

《第三步》shell测试脚本是否有效：
openwrt->网络->接口，WAN点击关闭，手动断开后执行下列命令，成功会自动恢复WAN的连接
注意命令首尾无空格
./my_pppoe.sh runA
  
《第四步》openwrt计划任务设置，系统->计划任务，添加下列命令中的一条，按需设置
注意命令首尾无空格
1分钟执行一次，异常重启wan
*/1 * * * * /root/my_pppoe.sh runA >> /root/my_pppoe.log 2>&1
2分钟执行一次，异常重启wan
*/2 * * * * /root/my_pppoe.sh runA >> /root/my_pppoe.log 2>&1
5分钟执行一次，异常重启wan
*/5 * * * * /root/my_pppoe.sh runA >> /root/my_pppoe.log 2>&1
5分钟执行一次，异常重启网络连接
*/5 * * * * /root/my_pppoe.sh runB >> /root/my_pppoe.log 2>&1
5分钟执行一次，异常重启路由器
*/5 * * * * /root/my_pppoe.sh runC >> /root/my_pppoe.log 2>&1
runA=重启WAN（极力推荐），支持1-30分钟
runB =重启网络连接（不推荐），支持5-30分钟，最少需要5分钟（设置太短可能无法进入路由器后台）
runC =重启路由（不推荐），支持5-30分钟，最少需要5分钟（设置太短可能无法进入路由器后台）

《第五步》判断计划任务是否成功执行脚本：
1. WinSCP到root目录下查看是否有日志文件（如果设置的是5分钟执行一次，会在设置后的5分钟才有记录显示）
