#
# Copyright (C) 2008-2014 The LuCI Team <luci@lists.subsignal.org>
#
# This is free software, licensed under the Apache License, Version 2.0 .
#

include $(TOPDIR)/rules.mk

PKG_VERSION:=2.4.2
PKG_RELEASE:=1

LUCI_TITLE:=LuCI support for Sdwan
LUCI_DEPENDS:=+kmod-tun +luci-compat
LUCI_PKGARCH:=all

PKG_NAME:=luci-app-sdwan

define Package/$(PKG_NAME)/conffiles
/etc/sdwan/
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
if [ -f /etc/config/sdwan ] ; then
  echo "备份luci配置文件/etc/config/sdwan到/tmp/sdwan_backup"
  echo "不重启设备之前再次安装luci-app-sdwan 配置不丢失,无需重新配置"
  cp -rf /etc/config/sdwan /tmp/sdwan_backup
fi

if ls /etc/sdwan/* >/dev/null 2>&1; then
  echo "备份Sdwan配置目录/etc/sdwan到/tmp/et_config_backup"
  echo "不重启设备之前再次安装luci-app-sdwan 配置不丢失,无需重新配置"
  cp -rf /etc/sdwan /tmp/et_config_backup
fi

endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
chmod +x /etc/init.d/sdwan
if [ -f /tmp/sdwan_backup ] ; then
  echo "发现luci备份配置文件/tmp/sdwan_backup，开始恢复到/etc/config/sdwan"
  rm -rf /etc/config/sdwan
  mv -f /tmp/sdwan_backup /etc/config/sdwan
  echo "请前往 VPN - Sdwan 界面进行重启插件"
fi
if [ -d /tmp/et_config_backup ] ; then
  echo "发现Sdwan备份配置目录/tmp/et_config_backup，开始恢复到/etc/sdwan"
  rm -rf /etc/sdwan
  mv -f /tmp/et_config_backup /etc/sdwan
  echo "请前往 VPN - Sdwan 界面进行重启插件"
fi
endef

$(eval $(call Build/Template,Package/$(PKG_NAME)/prerm))
$(eval $(call Build/Template,Package/$(PKG_NAME)/postinst))

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
