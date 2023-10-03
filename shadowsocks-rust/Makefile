# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2017-2020 Yousong Zhou <yszhou4tech@gmail.com>
# Copyright (C) 2021-2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=shadowsocks-rust
PKG_VERSION:=1.16.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/shadowsocks/shadowsocks-rust/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=da4c6256247207b2579721046292bab1a2ac62301878c73ff778c168caa8a990

PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=rust/host

RUST_PKG_FEATURES:=local-redir

include $(INCLUDE_DIR)/package.mk
#include $(TOPDIR)/feeds/packages/lang/rust/rust-package.mk
include $(TOPDIR)/feeds/openmptcprouter/rust/rust-package.mk

define Package/shadowsocks-rust/Default
  define Package/shadowsocks-rust-$(1)
    SECTION:=net
    CATEGORY:=Network
    SUBMENU:=Web Servers/Proxies
    TITLE:=shadowsocks-rust $(1)
    URL:=https://github.com/shadowsocks/shadowsocks-rust
    DEPENDS:=$$(RUST_ARCH_DEPENDS)
  endef

  define Package/shadowsocks-rust-$(1)/install
	$$(INSTALL_DIR) $$(1)/usr/bin
	$$(INSTALL_BIN) $$(PKG_INSTALL_DIR)/bin/$(1) $$(1)/usr/bin/
  endef
endef

define Package/shadowsocks-rust-config
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=shadowsocks-rust config
  URL:=https://github.com/shadowsocks/shadowsocks-rust
  DEPENDS:=$$(RUST_ARCH_DEPENDS)
endef


define Package/shadowsocks-rust-config/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/ssr-rules $(1)/usr/bin
	$(INSTALL_BIN) ./files/ssr-rules6 $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DATA) ./files/firewall.ssr-rules $(1)/etc
	$(INSTALL_BIN) ./files/ssr-rules.defaults $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/shadowsocks-rust.config $(1)/etc/config/shadowsocks-rust
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/shadowsocks-rust.init $(1)/etc/init.d/shadowsocks-rust
endef

SHADOWSOCKS_COMPONENTS:=sslocal ssmanager ssserver ssurl ssservice
define shadowsocks-rust/templates
  $(foreach component,$(SHADOWSOCKS_COMPONENTS),
    $(call Package/shadowsocks-rust/Default,$(component))
  )
endef
$(eval $(call shadowsocks-rust/templates))

$(eval $(call BuildPackage,shadowsocks-rust-config))
$(foreach component,$(SHADOWSOCKS_COMPONENTS), \
  $(eval $(call BuildPackage,shadowsocks-rust-$(component))) \
)