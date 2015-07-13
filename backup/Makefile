##############################################
# OpenWrt Makefile for backupworld program
#
#
# Most of the variables used here are defined in
# the include directives below. We just need to
# specify a basic description of the package,
# where to build our program, where to find
# the source files, and where to install the
# compiled program on the router.
#
# Be very careful of spacing in this file.
# Indents should be tabs, not spaces, and
# there should be no trailing whitespace in
# lines that are not commented.
#
##############################################

include $(TOPDIR)/rules.mk

# Name and release number of this package
PKG_NAME:=backup
PKG_RELEASE:=1

# This specifies the directory where we're going to build the program.
# The root build directory, $(BUILD_DIR), is by default the build_mipsel
# directory in your OpenWrt SDK directory
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

# Specify package information for this program.
# The variables defined here should be self explanatory.
define Package/$(PKG_NAME)
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=RSync script
  	DEPENDS:=+rsync
	PKG_SOURCE_PROTO:=git
	PKG_SOURCE_URL:=https://github.com/jfilaa/backup.git
	PKG_SOURCE:=$(PKG_NAME).tar.gz
	PKG_SOURCE_VERSION:=e733f271f8fa949b8e1d45b571c90e4b9c11e615
	PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.gz
	PKG_SOURCE_SUBDIR:=$(PKG_NAME)
	VERSION:=$(LINUX_VERSION)-$(PKG_RELEASE)
	PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)
	URL:=http://www.jfila.cz
	MAINTAINER:=Ing. Jan Fíla <jfila@jfila.cz>
#	DESCRIPTION:=\
#	If you can't figure out what this program does, \\
#	you're probably brain-dead and need immediate \\
#	medical attention.
endef

define Package/backup/description
	Script for sync data on two HDDs.
endef


# Specify what needs to be done to prepare for building the package.
# In our case, we need to copy the source files to the build directory.
# This is NOT the default.  The default uses the PKG_SOURCE_URL and the
# PKG_SOURCE which is not defined here to download the source from the web.
# In order to just build a simple program that we have just written, it is
# much easier to do it this way.
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	#$(CP) ./srcp/* $(PKG_BUILD_DIR)/
	#true # tady se nic nebuildí, tak buildění přeskočíme
endef

# We do not need to define Build/Configure or Build/Compile directives
# The defaults are appropriate for compiling a simple program such as this one

# Specify where and how to install the program. Since we only have one file,
# the backupworld executable, install it by copying it to the /bin directory on
# the router. The $(1) variable represents the root directory on the router running
# OpenWrt. The $(INSTALL_DIR) variable contains a command to prepare the install
# directory if it does not already exist.  Likewise $(INSTALL_BIN) contains the
# command to copy the binary file from its current location (in our case the build
# directory) to the install directory.
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/backup $(1)/bin/
	#$(INSTALL_BIN) $(PKG_BUILD_DIR)/backup.sh $(1)/usr/bin/
endef

define Package/$(PKG_NAME)/postinst
	#!/bin/sh
	set -x
	# díky tomuto se následujcí script pustí jen na cílovém systému
	[ -n "$$IPKG_INSTROOT" ] ||
	{
		rm /etc/config/backup && touch /etc/config/backup
		uci add backup global
		uci set backup.@global[0].source=/tmp/pokus/HDD1/
		uci set backup.@global[0].destination=/tmp/pokus/HDD2
		uci set backup.@global[0].md5file=/tmp/backup_check_sums.md5
		uci set backup.@global[0].logfile=/tmp/log/backup
		uci commit && cat /etc/config/backup
	}
endef

# This line executes the necessary commands to compile our program.
# The above define directives specify all the information needed, but this
# line calls BuildPackage which in turn actually uses this information to
# build a package.
$(eval $(call BuildPackage,$(PKG_NAME)))
