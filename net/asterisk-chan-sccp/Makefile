#
# Copyright (C) 2016 OpenWrt.org
# Copyright (C) 2016 Cesnet, z.s.p.o.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=chan-sccp
PKG_VERSION:=v4.3.1-20181225
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://github.com/chan-sccp/chan-sccp.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=614fc3e7514c56b233431f92b87e6cf211f5d925
PKG_MIRROR_HASH:=9c56a5b9d0768bd79e5c2cc1c4704ffbbbe9c6d2a2f8d994ff145143d4814fda
PKG_SOURCE_PROTO:=git

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-1.0
PKG_LICENSE_FILES:=COPYING LICENSE
PKG_MAINTAINER:=Jiri Slachta <jiri@slachta.eu>

PKG_INSTALL:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
# chan-sccp needs iconv
include $(INCLUDE_DIR)/nls.mk

define Package/chan-sccp/Default
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  TITLE:=SCCP channel support
  URL:=https://github.com/chan-sccp/chan-sccp
  DEPENDS:=$(ICONV_DEPENDS) +libltdl
endef

define Package/asterisk13-chan-sccp
$(call Package/chan-sccp/Default)
  DEPENDS+=asterisk13
  VARIANT:=asterisk13
  CONFLICTS:=asterisk13-chan-skinny
endef

define Package/asterisk15-chan-sccp
$(call Package/chan-sccp/Default)
  DEPENDS+=asterisk15
  VARIANT:=asterisk15
  CONFLICTS:=asterisk15-chan-skinny
endef

define Package/description/Default
Replacement for the SCCP channel driver (chan_skinny) in Asterisk.
Extended features include shared lines, presence / BLF, customizable
feature buttons and custom device state.
endef

Package/asterisk13-chan-sccp/description = $(Package/description/Default)
Package/asterisk15-chan-sccp/description = $(Package/description/Default)

CONFIGURE_ARGS += \
	--disable-debug \
	--enable-advanced-functions \
	--enable-conference \
	--enable-video

ifeq ($(BUILD_VARIANT),asterisk13)
  CONFIGURE_ARGS += --with-asterisk=$(STAGING_DIR)/usr/include/asterisk-13
endif

ifeq ($(BUILD_VARIANT),asterisk15)
  CONFIGURE_ARGS += --with-asterisk=$(STAGING_DIR)/usr/include/asterisk-15
endif

define Package/conffiles/Default
/etc/asterisk/sccp.conf
endef

Package/asterisk13-chan-sccp/conffiles = $(Package/conffiles/Default)
Package/asterisk15-chan-sccp/conffiles = $(Package/conffiles/Default)

define Package/Install/Default
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/asterisk/sccp.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)/usr/lib/asterisk/modules
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/asterisk/modules/chan_sccp.so $(1)/usr/lib/asterisk/modules/
endef

Package/asterisk13-chan-sccp/install = $(Package/Install/Default)
Package/asterisk15-chan-sccp/install = $(Package/Install/Default)

$(eval $(call BuildPackage,asterisk13-chan-sccp))
$(eval $(call BuildPackage,asterisk15-chan-sccp))
