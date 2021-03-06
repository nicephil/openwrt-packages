#
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=php
PKG_VERSION:=5.3.8
PKG_RELEASE:=5

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://www.php.net/distributions/
PKG_MD5SUM:=704cd414a0565d905e1074ffdc1fadfb

PKG_FIXUP:=libtool no-autoreconf
PKG_BUILD_PARALLEL:=1

PHP5_MODULES = \
	apc \
	ctype curl \
	fileinfo \
	dio dom \
	exif \
	ftp \
	gettext gd gmp \
	hash http \
	iconv \
	json \
	ldap libevent \
	mbstring mcrypt mysql \
	openssl \
	pcntl pdo pdo-mysql pdo-pgsql pdo-sqlite pgsql \
	session simplexml soap sockets sqlite sqlite3 sysvmsg sysvsem sysvshm \
	tokenizer \
	xml xmlreader xmlwriter \

PKG_CONFIG_DEPENDS:= \
	CONFIG_PACKAGE_php5-cgi CONFIG_PACKAGE_php5-cli \
	$(patsubst %,CONFIG_PACKAGE_php5-mod-%,$(PHP5_MODULES)) \
	CONFIG_PHP5_FILTER CONFIG_PHP5_LIBXML PHP5_SYSTEMTZDATA

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/php5/Default
  SUBMENU:=PHP
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=PHP5 Hypertext preprocessor
  URL:=http://www.php.net/
  MAINTAINER:=Michael Heimpold <mhei@heimpold.de>
  DEPENDS:=php5
endef

define Package/php5/Default/description
  PHP is a widely-used general-purpose scripting language that is especially 
  suited for Web development and can be embedded into HTML.
endef

define Package/php5/config
	config PHP5_FILTER
		bool "PHP5 Filter support"
		depends on PACKAGE_php5-cli || PACKAGE_php5-cgi

	config PHP5_LIBXML
		bool "PHP5 LIBXML support"
		depends on PACKAGE_php5-cli || PACKAGE_php5-cgi

	config PHP5_SYSTEMTZDATA
		bool "Use system timezone data instead of php's built-in database"
		depends on PACKAGE_php5-cli || PACKAGE_php5-cgi
		select PACKAGE_zoneinfo-core
		default y
		help
			Enabling this feature automatically selects the zoneinfo-core package
			which contains data for UTC timezone. To use other timezones you have
			to install the corresponding zoneinfo-... package(s).
endef

define Package/php5
  $(call Package/php5/Default)

  DEPENDS:=+libpcre +zlib \
           +PHP5_LIBXML:libxml2
endef

define Package/php5/description
  $(call Package/php5/Default/description)
  This package contains only the PHP config file. You must actually choose 
  your PHP flavour (cli, cgi or fastcgi).
endef

define Package/php5-cli
  $(call Package/php5/Default)
  TITLE+= (CLI)
endef

define Package/php5-cli/description
  $(call Package/php5/Default/description)
  This package contains the CLI version of the PHP5 interpreter.
endef

define Package/php5-cgi
  $(call Package/php5/Default)
  TITLE+= (CGI & FastCGI)
endef

define Package/php5-cgi/description
  $(call Package/php5/Default/description)
  This package contains the CGI version of the PHP5 interpreter.
endef

define Package/php5-fastcgi
  $(call Package/php5/Default)
  DEPENDS+= +php5-cgi
  TITLE:=FastCGI startup script
endef

define Package/php5-fastcgi/description
  As FastCGI support is now a core feature the php5-fastcgi package now depends
  on the php5-cgi package, containing just the startup script.
endef

CONFIGURE_ARGS+= \
	--enable-shared \
	--disable-static \
	--disable-rpath \
	--disable-debug \
	--without-pear \
	\
	--with-config-file-path=/etc \
	--with-config-file-scan-dir=/etc/php5 \
	--enable-magic-quotes \
	--disable-short-tags \
	\
	--with-zlib="$(STAGING_DIR)/usr" \
	  --with-zlib-dir="$(STAGING_DIR)/usr" \
	--with-pcre-regex="$(STAGING_DIR)/usr" \
	--disable-phar

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-cli),)
  CONFIGURE_ARGS+= --enable-cli
else
  CONFIGURE_ARGS+= --disable-cli
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-cgi),)
  CONFIGURE_ARGS+= --enable-cgi
else
  CONFIGURE_ARGS+= --disable-cgi
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-apc),)
  CONFIGURE_ARGS+= --enable-apc=shared --disable-apc-mmap --disable-apc-pthreadmutex
else
  CONFIGURE_ARGS+= --disable-apc
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-ctype),)
  CONFIGURE_ARGS+= --enable-ctype=shared
else
  CONFIGURE_ARGS+= --disable-ctype
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-curl),)
  CONFIGURE_ARGS+= --with-curl=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-curl
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-fileinfo),)
  CONFIGURE_ARGS+= --enable-fileinfo=shared
else
  CONFIGURE_ARGS+= --disable-fileinfo
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-gettext),)
  CONFIGURE_ARGS+= --with-gettext=shared,"$(STAGING_DIR)/usr/lib/libintl-full"
else
  CONFIGURE_ARGS+= --without-gettext
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-dio),)
  CONFIGURE_ARGS+= --enable-dio=shared
else
  CONFIGURE_ARGS+= --disable-dio
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-dom),)
  CONFIGURE_ARGS+= --enable-dom=shared
else
  CONFIGURE_ARGS+= --disable-dom
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-exif),)
  CONFIGURE_ARGS+= --enable-exif=shared
else
  CONFIGURE_ARGS+= --disable-exif
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-ftp),)
  CONFIGURE_ARGS+= --enable-ftp=shared
else
  CONFIGURE_ARGS+= --disable-ftp
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-gd),)
  CONFIGURE_ARGS+= \
	--with-gd=shared,"$(STAGING_DIR)/usr" \
	--without-freetype-dir \
	--with-jpeg-dir="$(STAGING_DIR)/usr" \
	--with-png-dir="$(STAGING_DIR)/usr" \
	--without-xpm-dir \
	--without-t1lib \
	--enable-gd-native-ttf \
	--disable-gd-jis-conv
else
  CONFIGURE_ARGS+= --without-gd
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-gmp),)
  CONFIGURE_ARGS+= --with-gmp=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-gmp
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-hash),)
  CONFIGURE_ARGS+= --enable-hash=shared
else
  CONFIGURE_ARGS+= --disable-hash
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-http),)
  CONFIGURE_ARGS+= \
	--enable-http=shared \
	--without-http-shared-deps \
	--with-http-curl-requests="$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --disable-http
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-iconv),)
  CONFIGURE_ARGS+= --with-iconv=shared,"$(ICONV_PREFIX)"
else
  CONFIGURE_ARGS+= --without-iconv
endif

ifneq ($(CONFIG_PACKAGE_php5-mod-json),)
  CONFIGURE_ARGS+= --enable-json=shared
else
  CONFIGURE_ARGS+= --disable-json
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-ldap),)
  CONFIGURE_ARGS+= \
	--with-ldap=shared,"$(STAGING_DIR)/usr" \
	--with-ldap-sasl="$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-ldap
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-libevent),)
  CONFIGURE_ARGS+= --with-libevent=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-libevent
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-mbstring),)
  CONFIGURE_ARGS+= --enable-mbstring=shared --enable-mbregex
else
  CONFIGURE_ARGS+= --disable-mbstring
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-mcrypt),)
  CONFIGURE_ARGS+=  --with-mcrypt=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-mcrypt
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-mysql),)
  CONFIGURE_ARGS+= --with-mysql=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-mysql
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-openssl),)
  CONFIGURE_ARGS+= \
	--with-openssl=shared,"$(STAGING_DIR)/usr" \
	--with-kerberos=no \
	--with-openssl-dir="$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-openssl
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-pcntl),)
  CONFIGURE_ARGS+= --enable-pcntl=shared
else
  CONFIGURE_ARGS+= --disable-pcntl
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-pdo),)
  CONFIGURE_ARGS+= --enable-pdo=shared
  ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-pdo-mysql),)
    CONFIGURE_ARGS+= --with-pdo-mysql=shared,"$(STAGING_DIR)/usr"
  else
    CONFIGURE_ARGS+= --without-pdo-mysql
  endif
  ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-pdo-pgsql),)
    CONFIGURE_ARGS+= --with-pdo-pgsql=shared,"$(STAGING_DIR)/usr"
  else
    CONFIGURE_ARGS+= --without-pdo-pgsql
  endif
  ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-pdo-sqlite),)
    CONFIGURE_ARGS+= --with-pdo-sqlite=shared,"$(STAGING_DIR)/usr"
  else
    CONFIGURE_ARGS+= --without-pdo-sqlite
  endif
else
  CONFIGURE_ARGS+= --disable-pdo
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-pgsql),)
  CONFIGURE_ARGS+= --with-pgsql=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-pgsql
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-session),)
  CONFIGURE_ARGS+= --enable-session=shared
else
  CONFIGURE_ARGS+= --disable-session
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-simplexml),)
  CONFIGURE_ARGS+= --enable-simplexml=shared
else
  CONFIGURE_ARGS+= --disable-simplexml
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-soap),)
  CONFIGURE_ARGS+= --enable-soap=shared
else
  CONFIGURE_ARGS+= --disable-soap
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-sockets),)
  CONFIGURE_ARGS+= --enable-sockets=shared
else
  CONFIGURE_ARGS+= --disable-sockets
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-sqlite),)
  CONFIGURE_ARGS+= --with-sqlite=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-sqlite
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-sqlite3),)
  CONFIGURE_ARGS+= --with-sqlite3=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --without-sqlite3
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-sysvmsg),)
  CONFIGURE_ARGS+= --enable-sysvmsg=shared
else
  CONFIGURE_ARGS+= --disable-sysvmsg
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-sysvsem),)
  CONFIGURE_ARGS+= --enable-sysvsem=shared
else
  CONFIGURE_ARGS+= --disable-sysvsem
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-sysvshm),)
  CONFIGURE_ARGS+= --enable-sysvshm=shared
else
  CONFIGURE_ARGS+= --disable-sysvshm
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-tokenizer),)
  CONFIGURE_ARGS+= --enable-tokenizer=shared
else
  CONFIGURE_ARGS+= --disable-tokenizer
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-xml),)
  CONFIGURE_ARGS+= --enable-xml=shared,"$(STAGING_DIR)/usr"
  ifneq ($(CONFIG_PHP5_LIBXML),)
    CONFIGURE_ARGS+= --with-libxml-dir="$(STAGING_DIR)/usr/include/libxml2"
  else
    CONFIGURE_ARGS+= --with-libexpat-dir="$(STAGING_DIR)/usr"
  endif
else
  CONFIGURE_ARGS+= --disable-xml
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-xmlreader),)
  CONFIGURE_ARGS+= --enable-xmlreader=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --disable-xmlreader
endif

ifneq ($(SDK)$(CONFIG_PACKAGE_php5-mod-xmlwriter),)
  CONFIGURE_ARGS+= --enable-xmlwriter=shared,"$(STAGING_DIR)/usr"
else
  CONFIGURE_ARGS+= --disable-xmlwriter
endif

ifneq ($(SDK)$(CONFIG_PHP5_FILTER),)
  CONFIGURE_ARGS+= --enable-filter
else
  CONFIGURE_ARGS+= --disable-filter
endif

ifneq ($(SDK)$(CONFIG_PHP5_LIBXML),)
  CONFIGURE_ARGS+= --enable-libxml
  CONFIGURE_ARGS+= --with-libxml-dir="$(STAGING_DIR)/usr/include/libxml2"
else
  CONFIGURE_ARGS+= --disable-libxml
endif

ifneq ($(CONFIG_PHP5_SYSTEMTZDATA),)
  CONFIGURE_ARGS+= --with-system-tzdata
else
  CONFIGURE_ARGS+= --without-system-tzdata
endif

CONFIGURE_VARS+= \
	LIBS="$(CONFIGURE_LIBS)" \
	ac_cv_c_bigendian_php=$(if $(CONFIG_BIG_ENDIAN),yes,no) \
	php_cv_cc_rpath="no" \
	iconv_impl_name="gnu_libiconv" \
	ac_cv_php_xml2_config_path="$(STAGING_DIR)/host/bin/xml2-config" \

define Package/php5/conffiles
/etc/php.ini
endef

define Package/php5/install
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/php.ini $(1)/etc/
endef

define Package/php5-cli/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/sapi/cli/php $(1)/usr/bin/php-cli
endef

define Package/php5-cgi/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/sapi/cgi/php-cgi $(1)/usr/bin/php-cgi
	ln -sf php-cgi $(1)/usr/bin/php-fcgi
endef

define Package/php5-fastcgi/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/php.init $(1)/etc/init.d/php
endef

define BuildModule

  define Package/php5-mod-$(1)
    $(call Package/php5/Default)

    ifneq ($(3),)
      DEPENDS+=$(3)
    endif

    TITLE:=$(2) shared module
  endef

  define Package/php5-mod-$(1)/install
	$(INSTALL_DIR) $$(1)/usr/lib/php
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/modules/$(subst -,_,$(1)).so $$(1)/usr/lib/php/
	$(INSTALL_DIR) $$(1)/etc/php5
	echo "extension=$(subst -,_,$(1)).so" > $$(1)/etc/php5/$(subst -,_,$(1)).ini
  endef

  $$(eval $$(call BuildPackage,php5-mod-$(1)))

endef

$(eval $(call BuildPackage,php5))
$(eval $(call BuildPackage,php5-cli))
$(eval $(call BuildPackage,php5-cgi))
$(eval $(call BuildPackage,php5-fastcgi))

#$(eval $(call BuildModule,NAME,TITLE[,PKG DEPENDS]))
$(eval $(call BuildModule,apc,APC,+PACKAGE_php5-mod-apc:librt))
$(eval $(call BuildModule,ctype,Ctype))
$(eval $(call BuildModule,curl,cURL,+PACKAGE_php5-mod-curl:libcurl))
$(eval $(call BuildModule,fileinfo,Fileinfo,+PACKAGE_php5-mod-fileinfo:libmagic))
$(eval $(call BuildModule,dio,Direct IO))
$(eval $(call BuildModule,dom,DOM,+@PHP5_LIBXML +PACKAGE_php5-mod-dom:libxml2))
$(eval $(call BuildModule,exif,EXIF))
$(eval $(call BuildModule,ftp,FTP))
$(eval $(call BuildModule,gettext,Gettext,+PACKAGE_php5-mod-gettext:libintl-full))
$(eval $(call BuildModule,gd,GD graphics,+PACKAGE_php5-mod-gd:libgd))
$(eval $(call BuildModule,gmp,GMP,+PACKAGE_php5-mod-gmp:libgmp))
$(eval $(call BuildModule,hash,Hash))
$(eval $(call BuildModule,http,HTTP,+PACKAGE_php5-mod-http:libcurl +PACKAGE_php5-mod-http:librt))
$(eval $(call BuildModule,iconv,iConv,+PACKAGE_php5-mod-iconv:libiconv))
$(eval $(call BuildModule,json,JSON))
$(eval $(call BuildModule,ldap,LDAP,+PACKAGE_php5-mod-ldap:libopenldap +PACKAGE_php5-mod-ldap:libsasl2))
$(eval $(call BuildModule,libevent,libevent,+PACKAGE_php5-mod-libevent:libevent))
$(eval $(call BuildModule,mbstring,MBString))
$(eval $(call BuildModule,mcrypt,Mcrypt,+PACKAGE_php5-mod-mcrypt:libmcrypt +PACKAGE_php5-mod-mcrypt:libltdl))
$(eval $(call BuildModule,mysql,MySQL,+PACKAGE_php5-mod-mysql:libmysqlclient))
$(eval $(call BuildModule,openssl,OpenSSL,+PACKAGE_php5-mod-openssl:libopenssl))
$(eval $(call BuildModule,pcntl,PCNTL))
$(eval $(call BuildModule,pdo,PHP Data Objects))
$(eval $(call BuildModule,pdo-mysql,PDO driver for MySQL,+php5-mod-pdo +PACKAGE_php5-mod-pdo-mysql:libmysqlclient))
$(eval $(call BuildModule,pdo-pgsql,PDO driver for PostgreSQL,+php5-mod-pdo +PACKAGE_php5-mod-pdo-pgsql:libpq))
$(eval $(call BuildModule,pdo-sqlite,PDO driver for SQLite 3.x,+php5-mod-pdo +PACKAGE_php5-mod-pdo-sqlite:libsqlite3 +PACKAGE_php5-mod-pdo-sqlite:libpthread))
$(eval $(call BuildModule,pgsql,PostgreSQL,+PACKAGE_php5-mod-pgsql:libpq))
$(eval $(call BuildModule,session,Session))
$(eval $(call BuildModule,simplexml,SimpleXML,+@PHP5_LIBXML +PACKAGE_php5-mod-simplexml:libxml2))
$(eval $(call BuildModule,soap,SOAP,+@PHP5_LIBXML +PACKAGE_php5-mod-soap:libxml2))
$(eval $(call BuildModule,sockets,Sockets))
$(eval $(call BuildModule,sqlite,SQLite 2.x,+PACKAGE_php5-mod-sqlite:libsqlite2))
$(eval $(call BuildModule,sqlite3,SQLite3,+PACKAGE_php5-mod-sqlite3:libsqlite3 +PACKAGE_php5-mod-sqlite3:libpthread))
$(eval $(call BuildModule,sysvmsg,System V messages))
$(eval $(call BuildModule,sysvsem,System V shared memory))
$(eval $(call BuildModule,sysvshm,System V semaphore))
$(eval $(call BuildModule,tokenizer,Tokenizer))
$(eval $(call BuildModule,xml,XML,+PHP5_LIBXML:libxml2 +!PHP5_LIBXML:libexpat))
$(eval $(call BuildModule,xmlreader,XMLReader,+@PHP5_LIBXML +PACKAGE_php5-mod-xmlreader:libxml2 +PACKAGE_php5-mod-xmlreader:libiconv))
$(eval $(call BuildModule,xmlwriter,XMLWriter,+@PHP5_LIBXML +PACKAGE_php5-mod-xmlwriter:libxml2 +PACKAGE_php5-mod-xmlwriter:libiconv))
