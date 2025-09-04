## 本项目为同步上游代码自动编译SmartDNS with webui Debian Amd64 Deb安装包
SmartDNS官网：[https://pymumu.github.io/smartdns](https://pymumu.github.io/smartdns)

仪表盘
![SmartDNS-WebUI](doc/smartdns-webui.png)

使用说明  
1、把deb安装文件上传至tmp目录，分配755权限，执行SSH命令。  

2、安装deb安装包，SSH命令（smartdns.1.2025.09.02-1245.amd64.deb文件名自行修改）。  
cd /tmp  
apt install ./smartdns.1.2025.09.02-1245.amd64.deb  

3、修改配置文件/root/smartdns/smartdns.conf内容如下  
#DNS服务器名称  
server-name smartdns  
#DNS端囗号  
bind [::]:53  
#设置日志  
log-level error  
log-file /var/log/smartdns/smartdns.log  
log-size 1M  
log-num 8  
log-file-mode 644  
log-console yes  
#开启审计,可在log看到哪个客户端取到了哪个IP，可用来判断SmartDNS生效与否。  
audit-enable yes  
audit-file /var/log/smartdns/smartdns-audit.log  
audit-size 1M  
audit-num 1  
log-file-mode 644  
audit-console  yes  
#DNS缓存，开启域名预获取、开启缓存过期。  
cache-size 10000  
rr-ttl-min 600  
prefetch-domain yes  
serve-expired yes  
cache-file /root/smartdns/smartdns.cache  
#测速，最快ping响应地址模式，DNS上游最快查询时延+ping时延最短，查询等待与链接体验最佳。  
response-mode first-ping  
speed-check-mode ping,tcp:80,tcp:443  
#扩展功能，禁用ipv6、禁用SOA 65、启用DNS查询。  
force-AAAA-SOA yes  
force-qtype-SOA 65  
mdns-lookup yes  
#以下数字形式和DOH二选择一  
#数字形式上游UDP DNS cn  
server 223.5.5.5 -group cn  
server 119.29.29.29 -group cn  
#WEB UI面版  
plugin smartdns_ui.so  
smartdns-ui.www-root /usr/share/smartdns/wwwroot  
smartdns-ui.ip http://0.0.0.0:6080  
martdns-ui.token-expire 600  
martdns-ui.max-query-log-age 86400  
smartdns-ui.enable-terminal yes  
smartdns-ui.enable-cors yes  
smartdns-ui.user admin  
smartdns-ui.password password  

#如需要DOH解析自行替换数字形式的DNS----------------------------------------------------------  
#Bootstrap Servers (传统UDP服务器，仅用于解析DoH域名)  
server 223.5.5.5 -bootstrap-dns  
server 119.29.29.29 -bootstrap-dns  
#DoH Servers (加密服务器，用于所有真实查询)  
server-https https://dns.alidns.com/dns-query -group cn  
server-https https://doh.pub/dns-query -group cn  

4、开机启动并立即启动服务  
systemctl enable smartdns.service --now  

5、WEB UI访问  
http://IP:6080  
默认用户名：admin  
默认密码：password  

6、卸载  
卸载软件（保留配置文件）  
apt remove smartdns  
彻底卸载（推荐，删除软件及其配置文件）  
apt purge smartdns -y  
#或者  
apt remove --purge smartdns  
卸载完后，可以运行以下命令删除不再需要的依赖包：  
apt autoremove  

7、捐赠原作者pymumu  
如果你觉得此项目对你有帮助，请捐助我们，使项目能持续发展和更加完善。  
PayPal贝宝  
[![Support via PayPal](https://cdn.rawgit.com/twolfson/paypal-github-button/1.0.0/dist/button.svg)](https://paypal.me/PengNick/)  
AliPay支付宝  
![alipay](doc/alipay_donate.jpg)  
WeChat Pay微信支付  
![wechat](doc/wechat_donate.jpg)  
#开源声明  
SmartDNS 基于 GPL V3 协议开源。
