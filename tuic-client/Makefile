# SPDX-License-Identifier: GPL-2.0-or-later
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=tuic-client
PKG_VERSION:=1.8.7
PKG_RELEASE:=1

PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

include $(INCLUDE_DIR)/package.mk

ifeq ($(ARCH),aarch64)
	TUIC_ARCH:=tuic-client-aarch64-linux-musl
	PKG_HASH:=9a602996a5af910aa8def33d5a6211cbcb43a5b33abca994ebc2fd009b578bde
else ifeq ($(ARCH),arm)
# Referred to golang/golang-values.mk
	ARM_CPU_FEATURES:=$(word 2,$(subst +,$(space),$(call qstrip,$(CONFIG_CPU_TYPE))))
	ifeq ($(ARM_CPU_FEATURES),)
		TUIC_ARCH:=tuic-client-armv7-linux-musl
		PKG_HASH:=922f8b70f99d96a810f3ac1501ea34e5830e534979f31437fda2934ab2b2d85d
	else
		TUIC_ARCH:=tuic-client-armv7-linux-muslhf
		PKG_HASH:=2fe5ee13226b91eef7938651c5a533e9c037654709a58a5aa522b17f657ec956
	endif
else ifeq ($(ARCH),i386)
	TUIC_ARCH:=tuic-client-i686-linux-musl
	PKG_HASH:=8389ed99c1ee70d8b91fb8f8563855c5180dfa0baf9d2d7f549ddfb5193d0b88
else ifeq ($(ARCH),x86_64)
	TUIC_ARCH:=tuic-client-x86_64-linux-musl
	PKG_HASH:=d01a62eba5107943651de0ad18070d73577a485ed694b7935f4aa7ee13604069
# Set the default value to make OpenWrt Package Checker happy
else
  PKG_SOURCE:=dummy
  PKG_HASH:=dummy
endif

define Download/tuic-client
	URL:=https://github.com/Itsusinn/tuic/releases/download/v$(PKG_VERSION)/
	URL_FILE:=$(TUIC_ARCH)
	FILE:=$(TUIC_ARCH)-$(PKG_VERSION)
	HASH:=$(PKG_HASH)
endef

$(eval $(call Download,tuic-client))

define Package/tuic-client
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=Delicately-TUICed 0-RTT proxy protocol
	URL:=https://github.com/Itsusinn/tuic
	DEPENDS:=@USE_MUSL @(aarch64||arm_v7||i386||x86_64) @!(TARGET_x86_geode||TARGET_x86_legacy)
endef

define Build/Compile
endef

define Package/tuic-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(DL_DIR)/$(TUIC_ARCH)-$(PKG_VERSION) $(1)/usr/bin/tuic-client
endef

$(eval $(call BuildPackage,tuic-client))
