#
# Copyright (C) 2026 Router OS Project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=router-net
PKG_RELEASE:=1

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=Router OS Project

include $(INCLUDE_DIR)/package.mk

define Package/router-net
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Router OS Networking Extensions
  DEPENDS:=+netifd +firewall4 +kmod-nft-core
  DEFAULT:=n
endef

define Package/router-net/description
  Networking extensions for Router OS including monitoring framework
  and interfaces for future packet processing optimizations.
endef

define Build/Compile
	# No compilation needed for this package skeleton
endef

define Package/router-net/install
	$(INSTALL_DIR) $(1)/usr/libexec/router-net
	$(INSTALL_DIR) $(1)/etc/config
endef

$(eval $(call BuildPackage,router-net))
