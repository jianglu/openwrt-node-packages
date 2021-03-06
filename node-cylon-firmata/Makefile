#
# Copyright (C) 2014-2017 Arduino LLC
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=cylon-firmata
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=0.24.0
PKG_RELEASE:=18

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=d38b39f08c291fc904dcc29da79408a1
PKG_HASH:=06ac7a8e2e6012577d2f4b043af766bf28a1d3e2a0d50e46629dab4f0bb65104

PKG_BUILD_DEPENDS:=node/host
PKG_USE_MIPS16:=0

PKG_MAINTAINER:=
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/node-cylon-firmata
  DEPENDS:=+node
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Cylon module for Arduino and other microcontrollers using the Firmata protocol
  URL:=https://www.npmjs.org/package/cylon-firmata
endef

define Package/node-cylon-firmata/description
 Cylon module for Arduino and other microcontrollers using the Firmata protocol
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(STAGING_DIR_HOSTPKG)/share/node/npm-cache \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install --build-from-source --target_arch=$(NODEJS_CPU) -g $(PKG_BUILD_DIR)
endef

define Package/node-cylon-firmata/install
	$(INSTALL_DIR) $(1)/usr/lib/node
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node/
endef

$(eval $(call BuildPackage,node-cylon-firmata))
