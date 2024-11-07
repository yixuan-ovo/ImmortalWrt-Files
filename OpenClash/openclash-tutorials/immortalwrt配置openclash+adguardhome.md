11.7更新:
修改复写设置-常规设置，增加github地址修改配置项-L112-[复写常规设置](#复写-常规设置)

11.5更新:
增加测试结果-L120-[DNS](#复写-dns设置)

——————————————————*********************——————————————————

## tips:
网络-DHCP/DNS-静态地址分配

配置主机名和MAC地址绑定
![img.png](img.png)
自定义在线分流规则模板教程
- https://www.youtube.com/watch?v=D841V_xgykg&list=PLSbqX2QvapHk7VYlbyHUIOonIl7q1n410&index=3

七尺宇openclash所需资料
- https://qichiyu.blogspot.com/2024/07/openclash.html

openclash配置自定义策略集教程
- https://github.com/Aethersailor/Custom_OpenClash_Rules/wiki/OpenClash-%E8%AE%BE%E7%BD%AE%E6%95%99%E7%A8%8B

## 软件包替换源:
将源地址 https://downloads.immortalwrt.org 或 https://mirrors.vsean.net/openwrt

更改为 https://mirrors.cernet.edu.cn/immortalwrt

## 常用软件包名:
luci-app-argon			argon主题

luci-app-adguardhome		自行github下载，链接:https://github.com/kongfl888/luci-app-adguardhome/releases

建议下载20221023，自带中文。最新版安装英文后再安装中文好像有冲突

luci-app-arpbind			IP/MAC地址绑定

luci-app-openclash			不解释

——————————————————*********************——————————————————

# immortalwrt关闭ipv6:
## lan口
网络-接口:删除wan6接口，编辑br-lan接口
![img_1.png](img_1.png)
DHCP服务器-ipv6设置:禁用三个ipv6服务，不勾选指定的主接口
![img_2.png](img_2.png)
全局网络选项删除ipv6地址
![img_3.png](img_3.png)
## 网络-DHCP/DNS-过滤器
勾选过滤ipv6 AAAA记录
![img_4.png](img_4.png)
以上之后，局域网设备就不会被分配ipv6地址了

## wan口
br-lan的网段不可以和wan的网段相同

接口配置wan口禁用获取ipv6地址
![img_5.png](img_5.png)
![img_6.png](img_6.png)
——————————————————*********************——————————————————

# openclash插件设置:

## 插件-模式设置
勾选使用meta内核，需要提前下载并上传meta内核文件

运行模式:Fake-IP(混合)模式

网络栈类型:mixed(2024.11.7版本openclash仅剩meta内核)

勾选UDP流量转发

代理模式Rule
![img_7.png](img_7.png)
## 插件-流量控制
勾选路由本机代理、禁用QUIC、绕过服务器地址、实验性绕过中国大陆IP（配置延迟低的dns）、仅允许内网

仅允许内网下方选择wan接口名字为wan（个人配置不同）
![img_8.png](img_8.png)
## 插件-DNS设置
先选择使用Dnsmasq进行转发，彻底配置好之后选择停用

清理一下持久化缓存，勾选禁止Dnsmasq缓存DNS

## 插件-流媒体增强
忽略

## 插件-黑白名单（复写设置-规则设置）:
不走代理的wanip，设置该项为188，复写设置内编辑规则为

- SRC-IP-CIDR,192.168.7.233/32,DIRECT	（意为7.233ip设备走直连
- SRC-IP-CIDR,192.168.7.233/32,节点分组名	（意为7.233ip设备走指定节点分组。例如:- SRC-IP-CIDR,192.168.7.233/32,🚀 手动切换
经测试发现黑白名单和自定义规则都可以实现不走代理

区别在于黑白名单只能定义ip

自定义规则可以定义域名:

- DOMAIN-SUFFIX,google.com,（代理组名） #匹配域名后缀，意为xxx.google.com走代理
- DOMAIN-KEYWORD,google,DIRECT（代理组名） #匹配域名关键字，意为域名含有google的走DIRECT
- DOMAIN,google.com,DIRECT（代理组名） #匹配域名，意为全域名匹配成功的走DIRECT
  ![img_9.png](img_9.png)
## 插件-IPV6设置
取消勾选，不使用IPV6
![img_10.png](img_10.png)

## 插件-GEO数据库订阅

可以使用默认链接

geoipDat老版本数据库，文件太大，不采用

geoipDat老版本数据库，文件太大，不采用

geoipDat老版本数据库，文件太大，不采用


geoip mmdb更新url:
- https://raw.githubusercontent.com/Loyalsoldier/geoip/release/Country.mmdb
- https://cdn.jsdelivr.net/gh/Loyalsoldier/geoip@release/Country.mmdb

geosite更新url:
- https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat
- https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geosite.dat

每天或每周更新一次，设置完自定义URL后点击检查并更新进行更新，单纯点击保存配置没有用
![img_11.png](img_11.png)
## 插件-大陆白名单订阅:
勾选自动更新，其余默认即可

——————————————————*********************——————————————————

# openclash复写设置:
## 复写-常规设置:
如果更新订阅出现【tmp/yaml_sub_tmp_config.yaml】下载失败等无法连接github错误

可以在Github地址修改中自定义github的国内mirror前缀，链接参考《一个链接实现模板和订阅转换》

## 复写-DNS设置:
勾选Fake-ip持久化，Fake-IP-Filter

**_若为桥接模式，暂定不需要勾选自定义dns服务器，可能会导致软件刚打开解析dns时间过长_**

**_由此推断若为路由模式，则只需要自定义dns服务器为光猫dns，不需要追加上游dns_**

2024.11.4取消勾选自定义服务器，保留追加上游dns不需要等待

2024.11.5测试后发现关闭自定义DNS服务器、只保留追加上游DNS不需要加载等待（PPPOE拨号模式下），猜测如果为路由模式需要自定义DNS服务器，取消勾选追加上游DNS

## 复写-Meta设置:
勾选启用TCP并发、启用统一延迟（为了测速好看，可开可不开）、Fake-IP持久化、启用流量(域名)探测、探测(嗅探)纯IP连接

其余停用或不勾选

## 复写-规则设置:
参考上方黑白名单

## 复写-开发者选项:
找到下方一行，将最后的true改成false，取消注释，嗅探TLS作用为:**？**

ruby_edit "$CONFIG_FILE" "['experimental']" "{'sniff-tls-sni'=>false}"

# openclash配置订阅
【漏网之鱼不能选全球直连！选择直连会泄露DNS。此时在绕过大陆ip选项的作用下，国内ip不会走clash内核】

测试dns泄露网址：https://ipleak.net/


# 勾选自动更新，修改配置文件:
勾选在线订阅转换，订阅转换服务地址clash-meta，订阅转换模板为自定义模板
- https://mirror.ghproxy.com/https://raw.githubusercontent.com/yixuan-ovo/ImmortalWrt-Files/refs/heads/main/OpenClash/subscribe-ini/yx-clash.ini

参考《一个链接同时实现配置模板和后端订阅转换》

添加Emoji可开，UDP转发需要机场支持才可启用，否则无法过梯


# 系统-软件包下
上传安装AdGuardHome时，若提示/etc/crontabs/root no such dirctory，输入mkdir -p /etc/crontabs即可

root检测文件地址:

/etc/init.d/AdGuardHome status/restart/stop/start

            （服务名称）  （控制命令）

指定服务重启命令

reboot  系统重启命令

# 配置定时任务:
vim /etc/contabs/root

50 5 * * * [ -f /usr/bin/AdGuardHome/data/querylog.json.1 ] && rm /usr/bin/AdGuardHome/data/querylog.json.1

为每天五点五十分检测是否有querylog.json.1文件，有则删除

cd /usr/bin/AdGuardHome/data 为打开adguardhome数据文件夹。


# AdgrardHome:
工作目录不要修改到临时目录文件夹下，每次重启会消失

（初次设置需更新核心版本，刚才让先开渠道是为了防止获取核心版本失败

更新完后点击启用，重定向暂时先不用开启）

## 初始化界面
80端口改成8008（个人习惯更改），53端口（Dnsmasq默认占用端口）改为5335（个人习惯）

设置账号密码后一直下一步进入后台主界面即可。

## 设置-常规设置
常规设置下除了使用过滤器之外其余全部取消

## 日志配置
勾选启用日志，时长建议24小时/7天或自行设置，若没有更改位置则建议关闭或24小时以下。

## 统计配置
勾选启用统计数据，时长建议24小时或7天，太长会占用存储空间

## 设置-DNS设置
上游服务器首先默认不要动，选择并行请求

bootstrap输入自己光猫后台测试过的两个dns即可

延迟低的写在第一个/或者写自己网上查询的延迟低的dns，其余取消。

## DNS服务配置
速度限制改为0，勾选启用EDNS客户端子网、启用DNSSEC、禁用IPv6地址的解析

## DNS缓存配置
缓存大小根据自己设备缓存容量设置即可，默认4M

勾选乐观缓存

其余设置默认即可

## 过滤器-黑名单
删除默认的两个，自行添加规则即可

广告终结者
- http://sub.adtchrome.com/adt-chinalist-easylist.txt

EasyListChina
- https://easylist-downloads.adblockplus.org/easylistchina.txt

Anti-AD
- https://raw.githubusercontent.com/privacy-protection-tools/anti-AD/master/anti-ad-easylist.txt

屏蔽cookies相关的警告
- https://www.i-dont-care-about-cookies.eu/abp/

秋风广告
- https://raw.githubusercontent.com/TG-Twilight/AWAvenue-Ads-Rule/main/AWAvenue-Ads-Rule.txt

百万ADH广告拦截过滤规则
- https://raw.githubusercontent.com/BlueSkyXN/AdGuardHomeRules/master/all.txt


## 彻底配置完后
上游服务器输入127.0.0.1:7874

（此为openclash的默认端口，在openclash的系统设置里面查看）

（此时测试上游可能失败，不用管）

此时回到设备后台选择作为dnsmasq的上游服务器就可以使用了


————————————————************————————————————

# :
## redir_host存在的问题
连接不是私密链接的原因:ip被污染，节点服务器无法根据被污染的ip访问指定网站。

例如:假设google-ip为4.4.4.4，节点服务器向上请求谷歌首页ip获得5.5.5.5，此时拿5的ip去访问google则会出现网站证书和域名不匹配，显示为链接非私密。

## fake-ip
浏览器发起dns请求，请求获取谷歌的ip，请求向上走到路由器，

此时clash劫持该请求，此时模式为fake-ip模式，所以路由器从自身ip段选择一个fake-ip返回给浏览器，

并且于映射表记录fake-ip和域名的映射关系，

浏览器拿到这个fakeip之后，向这个fakeip发起谷歌的dns请求。

请求来到路由器之后clash根据该fakeip在对应表中查询对应的域名，使用域名进行规则匹配，根据google.com这个域名匹配到的规则决定走代理还是走直连。

此时根据规则把谷歌的域名交给远程节点服务器进行dns解析获取真实的谷歌ip（相当于将原来需要在本地做的dns解析放在了远程服务器，不存在dns泄露，少了一次dns请求，降低了延迟）

## 缺点为

某一次获取到了百度的fakeip，此时clash闪退，但电脑上缓存了百度的假ip。再次访问百度时会访问fakeip，此时将会无法访问。

虽然clash将ttl缓存设置为1s，但是程序不一定会遵守ttl的规定，为了防止频繁发起dns请求，可能会延长缓存时间。

还有一些程序会开启dns重绑保护，如果获取到私有ip则会认为非法dns劫持导致数据包丢弃，典型例子为:windows使用fakeip时电脑有网，但是联网图标显示没网。

解决办法是加入fake-ip-filter，放在里面的域名列表不会返回假的ip，会发起dns请求获取真实的ip地址，相当于这些域名的处理回退到redir-host模式。

另外由于udp在某些场景下必须使用真实ip，所以clash处理udp域名，即使使用fakeip模式也会发起dns请求，例如QUIC功能，解决办法是禁用浏览器的quic功能。