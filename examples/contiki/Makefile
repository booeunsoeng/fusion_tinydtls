########################################################################
# platform-specific options

CFLAGS += -DUIP_CONF_ND6_SEND_NS=1
#CFLAGS += -DCC2538DK_AES=1

ifeq ($(TARGET), econotag)
CFLAGS += -DUIP_CONF_TCP=0
endif

ifeq ($(TARGET), minimal-net)
UIP_CONF_IPV6_RPL=0
CFLAGS += -DUIP_CONF_IPV6_RPL=0 -DRPL_BORDER_ROUTER=0
endif

# usually, you should not need changing anything beyond this line
########################################################################

# the library's version
VERSION:=0.8.2

# tools

SHELL = /bin/sh
MKDIR = mkdir

abs_builddir = /home/user/contiki/apps/tinydtls/examples/contiki
top_builddir = ../..
top_srcdir:= ../..
DISTDIR=$(top_builddir)/tinydtls
DTLS_SOURCES:=dtls-server.c dtls-client.c
FILES:=Makefile.in $(DTLS_SOURCES)

all: dtls-server dtls-client sensor receiver
	$(MAKE) $(MAKEFLAGS) ROLE=server dtls-server
	$(MAKE) $(MAKEFLAGS) clean
	$(MAKE) $(MAKEFLAGS) ROLE=client dtls-client

CONTIKI=$(top_srcdir)/../..

WITH_UIP6=1
UIP_CONF_IPV6=1
MODULES += core/net/ipv6/multicast

ifneq ($(ROLE),client)
	CFLAGS+= -DHARD_CODED_ADDRESS=\"aaaa::02:232\"
else
	CFLAGS+= -DUDP_CONNECTION_ADDR="fe80::ff:fe02:232" \
		 -DHARD_CODED_ADDRESS=\"aaaa::02:230\"
endif

CFLAGS += -ffunction-sections
LDFLAGS += -Wl,--gc-sections,--undefined=_reset_vector__,--undefined=InterruptVectors,--undefined=_copy_data_init__,--undefined=_clear_bss_init__,--undefined=_end_of_init__

CFLAGS += -DSHA2_USE_INTTYPES_H

APPS += tinydtls/aes tinydtls/sha2 tinydtls/ecc tinydtls

ccm-test: tests/ccm-test

dist:	$(FILES)
	test -d $(DISTDIR)/examples/contiki || $(MKDIR) -p $(DISTDIR)/examples/contiki
	cp $(FILES) $(DISTDIR)/examples/contiki

ifeq ("1", "1")
include $(CONTIKI)/Makefile.include
endif


