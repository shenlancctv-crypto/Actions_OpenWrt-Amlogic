#!/bin/bash
# N1 盒子校园网旁路由定制版
# 基于 quanjindeng/Actions_OpenWrt-Amlogic 修改
# 使用前请确保 .config-full 已替换为定制版本

sed -i '/CYXluq4wUazHjmCDBCqXF/d' package/lean/default-settings/files/zzz-default-settings

# 修改默认主题为 Argon
sed -i 's/luci-theme-bootstrap/luci-theme-argon/g' ./feeds/luci/collections/luci/Makefile

# Add autocore support for armvirt
sed -i 's/TARGET_rockchip/TARGET_rockchip\|\|TARGET_armvirt/g' package/lean/autocore/Makefile

# 设置版本号
sed -i "s/OpenWrt /N1-Campus $(TZ=UTC-8 date "+%Y.%m.%d") @ OpenWrt /g" package/lean/default-settings/files/zzz-default-settings

# 修改默认 IP 为旁路由地址（主路由假设为 192.168.31.1）
sed -i 's/192.168.1./192.168.31./g' package/base-files/files/bin/config_generate
sed -i 's/192.168.1./192.168.31./g' package/base-files/luci2/bin/config_generate
sed -i 's/192.168.1./192.168.31./g' package/base-files/Makefile
sed -i 's/192.168.1./192.168.31./g' package/base-files/image-config.in

# 修改主机名
sed -i 's/OpenWrt/OpenWrt-N1/g' package/base-files/files/bin/config_generate

sed -i 's/invalid users = root/#invalid users = root/g' feeds/packages/net/samba4/files/smb.conf.template

# 修复插件自启动脚本权限
sed -i '/exit 0/i\chmod +x /etc/init.d/*' package/lean/default-settings/files/zzz-default-settings

# ============== 拉取软件包 ==============

# 晶晨宝盒（N1 管理必备）
git clone https://github.com/ophub/luci-app-amlogic.git package/luci-app-amlogic

# kenzok8 插件合集
git clone https://github.com/kenzok8/small-package package/small-package

# Argon 主题及配置
git clone https://github.com/jerrykuku/luci-app-argon-config.git package/luci-app-argon-config
git clone https://github.com/jerrykuku/luci-theme-argon.git package/luci-theme-argon

# 解锁网易云音乐
git clone https://github.com/UnblockNeteaseMusic/luci-app-unblockneteasemusic.git package/luci-app-unblockneteasemusic
svn co https://github.com/kiddin9/openwrt-packages/trunk/UnblockNeteaseMusic-Go package/UnblockNeteaseMusic-Go
svn co https://github.com/kiddin9/openwrt-packages/trunk/luci-app-unblockneteasemusic-go package/luci-app-unblockneteasemusic-go

# 流量监控
git clone --depth 1 https://github.com/brvphoenix/luci-app-wrtbwmon package/deng/luci-app-wrtbwmon
git clone --depth 1 https://github.com/brvphoenix/wrtbwmon package/deng/wrtbwmon

# ============== 校园网防检测插件 ==============

# UA2F（User-Agent 伪装，Zxilly 活跃维护版，2026年5月仍有更新）
git clone https://github.com/Zxilly/UA2F.git package/UA2F

# kmod-rkp-ipid（IPID 防检测）
git clone https://github.com/EOYOHOO/rkp-ipid.git package/rkp-ipid

# ============== 删除重复包 ==============
rm -rf package/small-package/luci-app-wechatpush
rm -rf feeds/luci/applications/luci-app-qbittorrent
rm -rf feeds/packages/net/qBittorrent-static
rm -rf feeds/packages/net/qBittorrent
rm -rf feeds/luci/themes/luci-theme-argon
rm -rf package/small-package/luci-app-openvpn-server
rm -rf package/small-package/openvpn-easy-rsa-whisky
rm -rf package/small-package/luci-app-koolproxyR
rm -rf package/small-package/luci-app-godproxy
rm -rf package/small-package/luci-app-argon*
rm -rf package/small-package/luci-theme-argon*
rm -rf package/small-package/luci-app-amlogic
rm -rf package/small-package/luci-app-unblockneteasemusic
rm -rf package/small-package/upx-static
rm -rf package/small-package/upx
rm -rf package/small-package/firewall*
rm -rf package/small-package/opkg
rm -rf package/feeds/packages/aliyundrive-webdav
rm -rf feeds/packages/multimedia/aliyundrive-webdav
rm -rf package/feeds/packages/perl-xml-parser
rm -rf package/feeds/packages/lrzsz
rm -rf package/feeds/packages/xfsprogs
rm -rf package/small-package/luci-app-netdata

# ============== 解锁网易云音乐配置 ==============
NAME=$"package/luci-app-unblockneteasemusic/root/usr/share/unblockneteasemusic" && mkdir -p $NAME/core
curl 'https://api.github.com/repos/UnblockNeteaseMusic/server/commits?sha=enhanced&path=precompiled' -o commits.json
echo "$(grep sha commits.json | sed -n "1,1p" | cut -c 13-52)">"$NAME/core_local_ver"
curl -L https://github.com/UnblockNeteaseMusic/server/raw/enhanced/precompiled/app.js -o $NAME/core/app.js
curl -L https://github.com/UnblockNeteaseMusic/server/raw/enhanced/precompiled/bridge.js -o $NAME/core/bridge.js
curl -L https://github.com/UnblockNeteaseMusic/server/raw/enhanced/ca.crt -o $NAME/core/ca.crt
curl -L https://github.com/UnblockNeteaseMusic/server/raw/enhanced/server.crt -o $NAME/core/server.crt
curl -L https://github.com/UnblockNeteaseMusic/server/raw/enhanced/server.key -o $NAME/core/server.key

# 晶晨宝盒配置
sed -i 's#https://github.com/breakings/OpenWrt#https://github.com/quanjindeng/Actions_OpenWrt-Amlogic#g' package/luci-app-amlogic/luci-app-amlogic/root/etc/config/amlogic
sed -i 's#ARMv8#openwrt_armvirt_v8#g' package/luci-app-amlogic/luci-app-amlogic/root/etc/config/amlogic
sed -i 's#opt/kernel#kernel#g' package/luci-app-amlogic/luci-app-amlogic/root/etc/config/amlogic

sed -i 's#mount -t cifs#mount.cifs#g' feeds/luci/applications/luci-app-cifs-mount/root/etc/init.d/cifs

# golang 版本修复
rm -rf feeds/packages/lang/golang
git clone https://github.com/sbwml/packages_lang_golang feeds/packages/lang/golang

# mosdns
find ./ | grep Makefile | grep v2ray-geodata | xargs rm -f
find ./ | grep Makefile | grep mosdns | xargs rm -f
git clone https://github.com/sbwml/luci-app-mosdns package/mosdns
git clone https://github.com/sbwml/v2ray-geodata package/geodata

echo "=== diy-part2.sh 完成 ==="
