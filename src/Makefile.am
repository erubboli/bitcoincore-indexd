# Copyright (c) 2018 Jonas Schnelli
# Distributed under the MIT software license, see the accompanying
# file COPYING or http://www.opensource.org/licenses/mit-license.php.

DIST_SUBDIRS = univalue libbtc

AM_LDFLAGS = $(PTHREAD_CFLAGS) $(LIBTOOL_LDFLAGS) $(HARDENED_LDFLAGS)
AM_CXXFLAGS = $(HARDENED_CXXFLAGS) $(ERROR_CXXFLAGS)
AM_CPPFLAGS = $(HARDENED_CPPFLAGS)
EXTRA_LIBRARIES =

if EMBEDDED_UNIVALUE
LIBUNIVALUE = univalue/libunivalue.la

$(LIBUNIVALUE): $(wildcard univalue/lib/*) $(wildcard univalue/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)
else
LIBUNIVALUE = $(UNIVALUE_LIBS)
endif

LIBBTC = libbtc/libbtc.la
$(LIBBTC): $(wildcard libbtc/src/*) $(wildcard univalue/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

bin_PROGRAMS = bitcoincore-indexd

BITCOINCORE_INDEXD_H = \
    btcnode.h \
    compat.h \
    dbinterface.h \
    hash.h \
    httpserver.h \
    logging.h \
    netaddress.h \
    shutdown.h \
    sync.h \
    threadsafety.h \
    tinyformat.h \
    utils.h

DATABASE_LEVELDB_SOURCES = \
    db_leveldb.h \
    db_leveldb.cpp
    
BITCOINCORE_INDEXD_SOURCES = \
    btcnode.cpp \
    httpserver.cpp \
    logging.cpp \
    main.cpp \
    netaddress.cpp \
    shutdown.cpp \
    sync.cpp \
    utils.cpp \
    $(BITCOINCORE_INDEXD_H) \
    $(DATABASE_LEVELDB_SOURCES)

# application binary #
bitcoincore_indexd_CPPFLAGS = $(AM_CPPFLAGS) -I$(builddir) $(UNIVALUE_CFLAGS) $(LIBUNIVALUE_CFLAGS) $(LIBBTC_CFLAGS) $(LEVELDB_CPPFLAGS)
bitcoincore_indexd_CXXFLAGS = $(AM_CXXFLAGS)
bitcoincore_indexd_SOURCES = $(BITCOINCORE_INDEXD_SOURCES)
bitcoincore_indexd_LDADD = $(LIBUNIVALUE) $(LIBBTC) $(LIBLEVELDB) $(LIBLEVELDB_SSE42) $(LIBMEMENV) $(EVENT_LIBS) $(EVENT_PTHREADS_LIBS)
bitcoincore_indexd_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)
bitcoincore_indexd_LIBTOOLFLAGS = --tag CXX


clean-local:
	-$(MAKE) -C univalue clean
	-$(MAKE) -C libbtc clean
	-rm -f leveldb/*/*.gcda leveldb/*/*.gcno leveldb/helpers/memenv/*.gcda leveldb/helpers/memenv/*.gcno
	-rm -f config.h

include Makefile.leveldb.include
