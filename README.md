# macos L2TP分流方案

## 黑名单模式

1. 设置 -> 网络 -> 目标VPN -> 高级 -> 取消勾选`通过VPN连接发送所有流量`
2. 将`blacklist`下的两个文件复制到`/etc/ppp/`下
3. `sudo chmod a+x /etc/ppp/ip-up /etc/ppp/ip-down`
4. 修改脚本中的ip规则
5. 重连vpn即可

## 白名单模式

1. 设置 -> 网络 -> 目标VPN -> 高级 -> 勾选`通过VPN连接发送所有流量`
2. 将`whitelist`下的两个文件复制到`/etc/ppp/`下
3. `sudo chmod a+x /etc/ppp/ip-up /etc/ppp/ip-down`
4. 修改脚本中的ip规则
5. 重连vpn即可

## 具体实现
来源于[chnroutes](https://github.com/fivesheep/chnroutes)，白名单模式未改动chnroutes的脚本，
黑名单模式仅修改路由规则的目标ip为vpn的网关

实现原理就是`ip-up`脚本会在连接vpn后自动执行，通过修改路由表来做分流。
而`ip-down`脚本会在断开vpn时自动执行，对路由表进行恢复

## 注意事项

- 如果刚开始已勾选`通过VPN连接发送所有流量`，建议重新创建vpn，我测试下取消勾选不会生效
- 需要追加规则直接在两个脚本内追加ip，然后重连即可
- 出现问题检查流程：
  1. 改完规则是否重连VPN
  2. `netstat -nr`，检查脚本内的路由规则是否已添加
  3. `ls -l /etc/ppp`，检查脚本文件是否可执行
