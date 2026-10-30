#
# Copyright (C) 2026 Router OS Project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=router-ui
PKG_RELEASE:=1

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=Router OS Project

include $(INCLUDE_DIR)/package.mk

define Package/router-ui
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=2. Modules
  TITLE:=Router OS LuCI Customizations
  DEPENDS:=+luci-base +luci-mod-admin-full
  DEFAULT:=n
endef

define Package/router-ui/description
  Custom LuCI modules for Router OS including dashboard,
  custom branding, and management interface extensions.
endef

define Build/Compile
	# No compilation needed for this package skeleton
endef

define Package/router-ui/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci
	$(INSTALL_DIR) $(1)/www/luci-static/resources
endef

$(eval $(call BuildPackage,router-ui))
