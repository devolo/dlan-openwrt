#
# Copyright (C) 2015-2023 devolo GmbH
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dlan-fw
PKG_VERSION:=1.0-c
PKG_RELEASE:=1

PKG_LICENSE:=devolo QCA

PKG_MAINTAINER := "Günther Kelleter" <guenther.kelleter@devolo.de>

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/Default
  SECTION:=firmware
  CATEGORY:=Firmware
  TITLE:=PLC firmware for devolo
  FW_SOURCE:=
  DEPENDS:=+dlan-plc +open-plc-utils
endef

define Package/dlan-plc
  $(eval $(call Package/$(PKG_NAME)/Default))
  SECTION:=net
  CATEGORY:=Network
  TITLE:=PLC runtime support for devolo dLAN devices
  DEPENDS:=+open-plc-utils +open-plc-utils-chkpib +open-plc-utils-getpib \
	   +open-plc-utils-modpib +open-plc-utils-setpib +open-plc-utils-hpavkey \
	   +open-plc-utils-plctool \
	   +ebtables +@BUSYBOX_CUSTOM +@BUSYBOX_CONFIG_SETSID
endef

define Package/dlan-plc/conffiles
/lib/firmware/plc/user.pib
/lib/firmware/plc/user.pib.tmp
/lib/firmware/plc/security-id
/lib/firmware/plc/dak
endef

define Package/dlan-plc/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) files/plc.init $(1)/etc/init.d/plc
	$(INSTALL_DIR) $(1)/etc/rc.button
	$(INSTALL_BIN) files/etc/rc.button/* $(1)/etc/rc.button
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DATA) files/etc/uci-defaults/* $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) files/usr/sbin/* $(1)/usr/sbin
endef

define Package/$(PKG_NAME)-hotspot
  $(call Package/$(PKG_NAME)/Default)
  TITLE += dLAN Hotspot
  FW_TARGET:=dlan-hotspot
  FW_HOSTTOOL:=plchost
  DEPENDS+=+open-plc-utils-$$(FW_HOSTTOOL)
endef

define Package/$(PKG_NAME)-pro-500-wp
  $(call Package/$(PKG_NAME)/Default)
  TITLE += dLAN pro 500 Wireless+
  FW_TARGET:=dlan-pro-500-wp
  FW_HOSTTOOL:=amphost
  DEPENDS+=+open-plc-utils-$$(FW_HOSTTOOL)
endef

define Package/$(PKG_NAME)-pro-1200-ac
  $(call Package/$(PKG_NAME)/Default)
  TITLE += dLAN pro 1200+ WiFi ac
  FW_TARGET:=dlan-pro-1200-ac
  FW_HOSTTOOL:=plchost
  DEPENDS+=+open-plc-utils-$$(FW_HOSTTOOL)
endef

define Package/$(PKG_NAME)-pro-1200-n
  $(call Package/$(PKG_NAME)/Default)
  TITLE += dLAN pro 1200+ WiFi n
  FW_TARGET:=dlan-pro-1200-n
  FW_HOSTTOOL:=plchost
  DEPENDS+=+open-plc-utils-$$(FW_HOSTTOOL)
endef

define InstallFirmware
	$(INSTALL_DIR) $(1)/lib/firmware/plc/$(FW_TARGET)
ifneq ($(FW_SOURCE),)
	$(INSTALL_DATA) qca/$(FW_SOURCE)/*.{pib,nvm} $(1)/lib/firmware/plc/$(FW_TARGET)
	if [ -d qca/$(FW_SOURCE)/bin ] ; then $(INSTALL_BIN) qca/$(FW_SOURCE)/bin/* $(1)/lib/firmware/plc/$(FW_TARGET); fi
else
	$(INSTALL_DATA) qca/$(FW_TARGET)/*.{pib,nvm} $(1)/lib/firmware/plc/$(FW_TARGET)
	if [ -d qca/$(FW_TARGET)/bin ] ; then $(INSTALL_BIN) qca/$(FW_TARGET)/bin/* $(1)/lib/firmware/plc/$(FW_TARGET); fi
endif
	echo "PLCFW_PATH=/lib/firmware/plc" >$(1)/lib/firmware/plc/$(FW_TARGET)/fwconfig
	echo "PLCFW_NVM='$(FW_TARGET)/$$$$(basename $$$$(ls -1 $(1)/lib/firmware/plc/$(FW_TARGET)/*.nvm|sed -n -e "1p"))'" >>$(1)/lib/firmware/plc/$(FW_TARGET)/fwconfig
	echo "PLCFW_PIB='$(FW_TARGET)/$$$$(basename $$$$(ls -1 $(1)/lib/firmware/plc/$(FW_TARGET)/*.pib|sed -n -e "1p"))'" >>$(1)/lib/firmware/plc/$(FW_TARGET)/fwconfig
	for pib in $$$$(cd $(1)/lib/firmware/plc/$(FW_TARGET)/; ls -1 *.pib); do \
		echo "PLCFW_PIB_$$$$(echo $$$$pib | sed -e 's,.*mt\([0-9]*\).*,\1,')='$(FW_TARGET)/$$$$pib'"; \
	done >>$(1)/lib/firmware/plc/$(FW_TARGET)/fwconfig
	echo "PLCFW_HOST=/usr/bin/$(FW_HOSTTOOL)" >>$(1)/lib/firmware/plc/$(FW_TARGET)/fwconfig
	echo "PLCFW_SCHEMES=\$$$$PLCFW_PATH/$(FW_TARGET)" >>$(1)/lib/firmware/plc/$(FW_TARGET)/fwconfig
endef

Package/$(PKG_NAME)-hotspot/install = $(InstallFirmware)
Package/$(PKG_NAME)-pro-500-wp/install = $(InstallFirmware)
Package/$(PKG_NAME)-pro-1200-ac/install = $(InstallFirmware)
Package/$(PKG_NAME)-pro-1200-n/install = $(InstallFirmware)

define Build/Compile
endef

define Build/Install
endef

$(eval $(call BuildPackage,dlan-plc))
$(eval $(call BuildPackage,$(PKG_NAME)-hotspot))
$(eval $(call BuildPackage,$(PKG_NAME)-pro-500-wp))
$(eval $(call BuildPackage,$(PKG_NAME)-pro-1200-ac))
$(eval $(call BuildPackage,$(PKG_NAME)-pro-1200-n))
