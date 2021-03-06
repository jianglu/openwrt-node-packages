#
# Copyright (C) 2016-2017 LEDE project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=mqtt
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=2.1.3
PKG_RELEASE:=6

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=70a684bddfafcc77ed6e166cf79dda2b
PKG_HASH:=862eea06f620326b58e9d91565cce18e4cf4371bf719efa4778e9b37c5b42789

PKG_BUILD_DEPENDS:=node/host node-uglify-js/host node-browserify/host
PKG_USE_MIPS16:=0

PKG_MAINTAINER:=
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE.md

include $(INCLUDE_DIR)/package.mk

define Package/node-mqtt
  DEPENDS:=+node
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=A library for the MQTT protocol
  URL:=https://www.npmjs.org/package/mqtt
endef

define Package/node-mqtt/description
 MQTT.js is a client library for the MQTT protocol, written in JavaScript for node.js and the browser
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
	NODE_PATH="$(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/node_modules" \
	npm install --build-from-source --target_arch=$(NODEJS_CPU) -g $(PKG_BUILD_DIR)
endef

define Package/node-mqtt/install
	$(INSTALL_DIR) $(1)/usr/lib/node
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node/
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../lib/node/mqtt/mqtt.js $(1)/usr/bin/mqtt
	$(LN) ../lib/node/mqtt/bin/pub.js $(1)/usr/bin/mqtt_pub
	$(LN) ../lib/node/mqtt/bin/sub.js $(1)/usr/bin/mqtt_sub
endef

$(eval $(call BuildPackage,node-mqtt))
