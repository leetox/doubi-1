# 如果遇到问题可以到原作者大佬Toyo的逗比根据地博客评论区找解决办法：
https://doub.io/ss-jc60/
# 系统要求
CentOS 6+ / Debian 6+ / Ubuntu 14.04 +   
推荐 Debian 7 x64，这个是我一直使用的系统，我的脚本在这个系统上面出错率最低。并且最容易安装锐速（锐速不支持OpenVZ）    
CentOS 7 自带防火墙问题(firewalld)自行解决，其他版本没有做测试。  
# 安装：
wget -N --no-check-certificate https://raw.githubusercontent.com/Ache1123/doubi/master/ssrmu.sh && chmod +x ssrmu.sh && bash ssrmu.sh
# 卸载：
ssrmu.sh脚本里有卸载选项
# 文件位置
安装目录：/usr/local/shadowsocksr   
配置文件：/usr/local/shadowsocksr/user-config.json    
数据文件：/usr/local/shadowsocksr/mudb.json
# 其他说明
ShadowsocksR 安装后，自动设置为 系统服务，所以支持使用服务来启动/停止等操作，同时支持开机启动。    
启动 ShadowsocksR：/etc/init.d/ssrmu start     
停止 ShadowsocksR：/etc/init.d/ssrmu stop     
重启 ShadowsocksR：/etc/init.d/ssrmu restart    
查看 ShadowsocksR状态：/etc/init.d/ssrmu status    
ShadowsocksR 默认支持UDP转发，服务端无需任何设置。      
本脚本已经集成了 安装/卸载 锐速(ServerSpeeder)/Lotserver，但是是否支持请查看 Linux支持内核列表 。（锐速、LotServer不支持OpenVZ）    
注意：本脚本中的 显示链接信息中的 获取IP归属地功能使用的是 IPIP.NET 的免费API接口，因为限速所以每秒只能检测一次，同时 IPIP.NET 的免费API接口并不会保证稳定性，可能什么时候就突然暂时失效了，这是本人不可控的，有条件可以自建API接口。
# ShadowsocksR目前支持的协议和混淆：
协议（Protocol）：origin，auth_sha1_v4，auth_aes128_md5，auth_aes128_sha1，auth_chain_a，auth_chain_b    
混淆（Obfs）：plain，http_simple，http_post，random_head，tls1.2_ticket_auth，tls1.2_ticket_fastauth(这个是客户端用的，而服务端需要选择tls1.2_ticket_auth)    
origin 和 plain 是原版，加粗的是推荐使用的。     
如果你想要使用 tls1.2_ticket_fastauth 混淆插件，那么服务端选择 tls1.2_ticket_auth，客户端选择 tls1.2_ticket_fastauth 即可。    
如果服务端 设置混淆参数为：tls1.2_ticket_auth_compatible (兼容原版)     
那么客户端 可使用的混淆为：plain / tls1.2_ticket_auth / tls1.2_ticket_fastauth      
tls1.2_ticket_auth 与 tls1.2_ticket_fastauth 的区别为，后者不会等待服务器回应，所以不会增加延迟。适合于，因为混淆插件增加延迟的原因不得不选择原版混淆 plain，但是又因为QOS等因素而处于延迟与干扰/限速等之间抉择的时候，可以选择 tls1.2_ticket_fastauth 客户端混淆插件！   
# 使用阿里云/腾讯云等存着安全组或规则组一类外部防火墙的请注意
因为阿里云/腾讯云的服务器还有一个外部的防火墙也就是叫 安全组或规则组。    
一般来说默认是只开放 22(SSH)端口，所以一些人在搭建ShadowsocksR服务端后，会出现无法访问的情况，ShadowsocksR客户端的统计窗口显示超时。    
同时ShadowsocksR服务端开启详细日志模式(其他功能中)后，ShadowsocksR客户端访问ShadowsocksR账号无日志输出。   
# 定时重启
一些人可能需要定时重启ShadowsocksR服务端来保证稳定性等，所以这里用 crontab 定时。     
crontab -l > "crontab.bak"   
sed -i "/ssrmu restart/d" "crontab.bak"   
echo -e "\n10 3 * * * /etc/init.d/ssrmu restart" >> "crontab.bak"    
crontab "crontab.bak"    
rm -r "crontab.bak"   
# 下面的只是让你对照理解用于修改上面第三行的定时间隔，只需要执行上面的代码即可。
# 如果你需要修改定时时间，那么重复执行上面代码就行了（记得修改第三行的定时间隔）。
# 如果你要删除定时重启任务，那么还是重复上面的代码，但是要跳过第三行代码。
# 下面代码前面的 * * * * * 分别对应：分钟 小时 日 月 星期
10 3 * * * /etc/init.d/ssrmu restart
# 这个代表 每天3点10分重启一次 ShadowsocksR
10 2 */2 * * /etc/init.d/ssrmu restart
# 这个代表 每隔2天的2点10分重启一次 ShadowsocksR
10 */4 * * * /etc/init.d/ssrmu restart
# 这个代表 每隔4小时的第10分重启一次 ShadowsocksR
