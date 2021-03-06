#
# Copyright (C) 2006-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=lirc
PKG_VERSION:=0.9.3a
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://lirc.git.sourceforge.net/gitroot/lirc/lirc
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=lirc-0_9_3a
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz

include $(INCLUDE_DIR)/package.mk

define Package/lirc/Default
	SECTION:=utils
	CATEGORY:=Utilities
	URL:=http://www.lirc.org/
	DEPENDS:= +libstdcpp
endef

define Package/lirc
	$(call Package/lirc/Default)
	TITLE:=LIRC - Linux Infrared Remote Control
	MENU:=1
endef

define Package/lirc/description
 LIRC is a package that allows you to decode and send
 infra-red signals of many (but not all) commonly used
 remote controls.

 This package contains only lirc daemon.
 You must select one or more drivers.
endef

define Package/lirc-tools
	$(call Package/lirc/Default)
	DEPENDS+= lirc
	TITLE:=LIRC tools
endef

define Package/lirc-tools/description
 This package contains those lirc-tools:
 -irw:     watch the codes as lircd recognize them.
endef

define Package/lirc-drv-devinput
	$(call Package/lirc/Default)
	DEPENDS+= lirc +kmod-hid
	TITLE:=Devinput driver
endef

define Package/lirc-drv-devinput/description
 This package contains LIRC driver for various devices that send keystrokes
endef

define Package/lirc-drv-default
	$(call Package/lirc/Default)
	DEPENDS+= lirc
	TITLE:=Default driver
endef

define Package/lirc-drv-default/description
 This package contains LIRC default driver
endef

define KernelPackage/mceusb
#  SUBMENU:=Remote control support
  TITLE:=Windows Media Center Ed. eHome Infrared Transceiver
#  DEPENDS:=@USB_SUPPORT
  KCONFIG:=CONFIG_IR_MCEUSB
  FILES:=$(LINUX_DIR)/drivers/media/rc/mceusb.ko
  AUTOLOAD:=$(call AutoProbe,mceusb)
endef

define KernelPackage/mceusb/description
  This package contains driver for the Windows Media Center Ed. eHome Infrared Transceiver
endef

CONFIGURE_ARGS += --without-x --disable-static 

DESTDIR = $(PKG_INSTALL_DIR)

define Build/Configure
	(cd $(PKG_BUILD_DIR); ./autogen.sh );
	$(call Build/Configure/Default,$(CONFIGURE_ARGS))
endef

define Build/Install
	$(MAKE) $(MAKE_FLAGS) -C $(PKG_BUILD_DIR) install DESTDIR=$(PKG_INSTALL_DIR)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/lirc $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc.{so*,la} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc_driver.{so*,la} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc_client.{so*,la} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libirrecord.{so*,la} $(1)/usr/lib/
endef

define Package/lirc/install
#	$(INSTALL_DIR) $(1)/etc/lirc
#	$(CP) ./files/etc/lirc/lirc_options.conf $(1)/etc/lirc/
#	$(CP) ./files/etc/lirc/lircd.conf $(1)/etc/lirc/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc.{la,so*} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc_driver.{la,so*} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc_client.{la,so*} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/lircd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/lirc.init $(1)/etc/init.d/lirc
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/lirc.conf $(1)/etc/config/lirc
endef

define Package/lirc-tools/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irw $(1)/usr/bin/
endef

define Package/lirc-drv-devinput/install
#	$(INSTALL_DIR) $(1)/etc/lirc/lircd.conf.d
#	$(CP) ./files/etc/lirc/lircd.conf.d/devinput.conf $(1)/etc/lirc/lircd.conf.d/
	$(INSTALL_DIR) $(1)/usr/lib/lirc/plugins
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lirc/plugins/devinput.{la,so*} $(1)/usr/lib/lirc/plugins/
endef

define Package/lirc-drv-default/install
#	$(INSTALL_DIR) $(1)/etc/lirc/lircd.conf.d
#	$(CP) ./files/etc/lirc/lircd.conf.d/devinput.conf $(1)/etc/lirc/lircd.conf.d/
	$(INSTALL_DIR) $(1)/usr/lib/lirc/plugins
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lirc/plugins/default.{la,so*} $(1)/usr/lib/lirc/plugins/
endef

$(eval $(call BuildPackage,lirc))
$(eval $(call BuildPackage,lirc-tools))
$(eval $(call BuildPackage,lirc-drv-devinput))
$(eval $(call BuildPackage,lirc-drv-default))
$(eval $(call KernelPackage,mceusb))
