#
# Copyright (C) 2015-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=hid
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=0.5.4
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/node-hid/node-hid.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=2bd91dda3619ed16018e7406c91d42907258c84b
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=0f2cafab7df5452dfbf65c4649af7076b8e105c0f38c2cbeeea928e6920a9cd8

PKG_BUILD_DEPENDS:=node/host
PKG_USE_MIPS16:=0

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=MIT or X11
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/node-hid
  DEPENDS:=+node +libusb-1.0 +hidapi
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Access USB HID devices from node.js
  URL:=https://github.com/node-hid/node-hid
endef

define Package/node-hid/description
 Access USB HID devices from node.js
endef

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include/libusb-1.0

define Build/Compile
	git init $(PKG_BUILD_DIR)
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(STAGING_DIR_HOSTPKG)/share/node/npm-cache \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install --build-from-source --target_arch=$(NODEJS_CPU) -g $(PKG_BUILD_DIR)
endef

define Package/node-hid/install
	$(INSTALL_DIR) $(1)/usr/lib/node
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node/
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../lib/node/node-hid/src/show-devices.js $(1)/usr/bin/hid-showdevices
endef

$(eval $(call BuildPackage,node-hid))
