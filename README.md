这个版本 2021.10.09-0959 是okay的，只有 trojan，没有 ss-libev。Phicomm K2 PSG1218 k2-v22.4 以下可用
https://github.com/gcp0923/PSG1218/releases/download/2021.10.09-0959/openwrt-ramips-mt7620-phicomm_k2-v22.4-squashfs-sysupgrade.bin
去掉了 PPP，ipv6，在helloworld 里面加了 ssr plus 需要的组件
openwrt的include 的 target 里把一些不要的删除了。

配置 PSG1218 .config
openwrt include/target.mk
helloworld ssrplus/makefile

从openwrt + helloworld 重新编译的，ssr-plus是复制到helloworld里了，还有其它几个组件
只有trojan
https://github.com/gcp0923/PSG1218/blob/master/openwrt-ramips-mt7620-phicomm_k2-v22.4-squashfs-sysupgrade%20(2).bin
这个应该是okay了，不会重启的

shadowsocks-libev 没有能装上去，估计空间也不够


改到默认的 openwrt
https://github.com/gcp0923/openwrt

连接数多了挂住，然后会重启

自制测试用，fork 来自大佬，为了K2 瞎改，自用, 改到 https://github.com/gcp0923/lede

shadowsocks-libev 来自 https://github.com/kenzok8/openwrt-packages/blob/master/luci-app-ssr-plus/Makefile  的配置好像有问题，无法加载shadowsocks-libev-ss-redir，这个模块已经在config中引用了

折衷解决方法:
https://github.com/gcp0923/openwrt-packages/blob/master/luci-app-ssr-plus/Makefile

LUCI_DEPENDS:=+coreutils +coreutils-base64 +dns2socks +dnsmasq-full +ipset \
	+ip-full +iptables-mod-tproxy +lua +libuci-lua +microsocks +pdnsd-alt \
	+tcping +resolveip +shadowsocksr-libev-ssr-check +uclient-fetch \
	+shadowsocks-libev-ss-redir +simple-obfs \

原文
LUCI_DEPENDS:=+coreutils +coreutils-base64 +dns2socks +dnsmasq-full +ipset \
	+ip-full +iptables-mod-tproxy +lua +libuci-lua +microsocks +pdnsd-alt \
	+tcping +resolveip +shadowsocksr-libev-ssr-check +uclient-fetch \
	+PACKAGE_$(PKG_NAME)_INCLUDE_Kcptun:kcptun-client \
	+PACKAGE_$(PKG_NAME)_INCLUDE_NaiveProxy:naiveproxy \
	+PACKAGE_$(PKG_NAME)_INCLUDE_Redsocks2:redsocks2 \
	+PACKAGE_$(PKG_NAME)_INCLUDE_Shadowsocks_Libev_Client:shadowsocks-libev-ss-local \
	+PACKAGE_$(PKG_NAME)_INCLUDE_Shadowsocks_Libev_Client:shadowsocks-libev-ss-redir \
	+PACKAGE_$(PKG_NAME)_INCLUDE_Shadowsocks_Libev_Server:shadowsocks-libev-ss-server \
	
我看不懂，只是为了我的K2能跑 SS-Libev 和 Trojan，删了网易，删了 nlbmonitor, 还有别的什么玩意，K2 内存CPU 不行，不要这些花哨
但是 Shadowsocks-Libev 被认成 Shadowsocks New Version? 


# OpenWrt firmware for K2-PSG1218-A
固件采用GitHub Actions不定时自动云编译斐讯K2-PSG1218-A。  
Auto build OpenWrt firmware for K2-PSG1218-A via GitHub Actions

# 致谢大佬&Thanks

https://github.com/coolsnowwolf/lede

https://github.com/P3TERX/Actions-OpenWrt/

https://github.com/Lienol/openwrt-package

.....



# 最新版下载&Download Latest
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/leopardciaw/PSG1218?style=for-the-badge&label=Download)](https://github.com/leopardciaw/PSG1218/releases/latest)


[所有已发布 & All Release](https://github.com/leopardciaw/PSG1218/releases)

# 请注意
1.集成的插件只是自己需要用到的，请多多包涵。  
2.源码来自https://github.com/coolsnowwolf/lede，
除了增减插件和主题、修改管理IP外，未做其他修改，有问题请直接到
https://github.com/coolsnowwolf/lede/issues 这里提交issue，提交issues请注意基本的礼仪和格式，翻看之前是否有大佬已提出相关issue。  
3.才疏学浅、时间有限，回答不了任何技术问题哈，只是定时或不定时更新下大雕的源码编译。
