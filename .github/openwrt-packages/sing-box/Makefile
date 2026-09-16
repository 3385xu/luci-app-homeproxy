# SPDX-License-Identifier: GPL-2.0-only

include $(TOPDIR)/rules.mk

PKG_NAME:=sing-box
PKG_VERSION:=1.13.21
PKG_RELEASE:=2
PKG_REV:=628cb31ffa79cffffd34c2f9cde6cae044e4fc12

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/SagerNet/sing-box/archive/refs/tags
PKG_HASH:=skip
PKG_BUILD_DIR:=$(BUILD_DIR)/sing-box-$(PKG_VERSION)

# 纯静态模式下支持的最全特性 Tags（剔除必须依赖 CGO 的 with_naive_outbound 和 with_musl）
BUILD_TAGS:=with_gvisor,with_quic,with_dhcp,with_wireguard,with_utls,with_acme,with_clash_api,with_tailscale,with_ccm,with_ocm,badlinkname,tfogo_checklinkname0

include $(INCLUDE_DIR)/package.mk

define Package/sing-box
  SECTION:=net
  CATEGORY:=Network
  TITLE:=The universal proxy platform
  URL:=https://github.com/SagerNet/sing-box
  DEPENDS:=+ca-bundle
endef

define Package/sing-box/description
  sing-box $(PKG_VERSION) source-built static package for x86_64 OpenWrt with full pure-go features.
endef

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
		GOTOOLCHAIN=local \
		CGO_ENABLED=0 \
		GOOS=linux \
		GOARCH=amd64 \
		go build -v -trimpath \
			-tags "$(BUILD_TAGS)" \
			-ldflags "-X 'github.com/sagernet/sing-box/constant.Version=$(PKG_VERSION)' \
			          -X 'github.com/sagernet/sing-box/constant.Commit=$(PKG_REV)' \
			          -s -w -buildid=" \
			-o sing-box ./cmd/sing-box
endef

define Package/sing-box/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/sing-box $(1)/usr/bin/sing-box
endef

$(eval $(call BuildPackage,sing-box))
