SRC_NAME=libtirpc-1.3.2

all: config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	#autoreconf -f ; \
	sh autogen.sh ; \
	./configure \
		--prefix=$(STAGEDIR) \
		--enable-shared \
		--disable-static \
		--disable-gssapi \
		$(if $(CONFIG_IPV6),--enable-ipv6,--disable-ipv6) \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

install:
	$(MAKE) -C $(SRC_NAME) install DESTDIR=""

romfs:
	cp -fP $(DESTDIR)/lib/libtirpc.so* $(ROMFSDIR)/lib
	cp -fP $(DESTDIR)/etc/netconfig $(ROMFSDIR)/etc_ro
