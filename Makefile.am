# Copyright (c) 2013-2018 The Bitcoin Core developers
# Distributed under the MIT software license, see the accompanying
# file COPYING or http://www.opensource.org/licenses/mit-license.php.

DIST_SUBDIRS = secp256k1

AM_LDFLAGS = $(LIBTOOL_LDFLAGS)
EXTRA_LIBRARIES =

BITCOIN_INCLUDES=-I$(builddir)

BITCOIN_INCLUDES += -I$(srcdir)/secp256k1/include

LIBBITCOIN=libbitcoin.a
LIBKERL=libkerl.a
LIBSECP256K1=secp256k1/libsecp256k1.la
LIBBITCOIN_DEB=libbitcoin_deb.a # bitcoin core functionality (debugger/*)

$(LIBSECP256K1): $(wildcard secp256k1/src/*) $(wildcard secp256k1/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

EXTRA_LIBRARIES += \
  $(LIBBITCOIN_DEB) \
  $(LIBBITCOIN) \
  $(LIBKERL)

bin_PROGRAMS = btcdeb btcc test-btcdeb tap
noinst_PROGRAMS =

# debugger #
LIBBITCOIN_DEB_H = \
	debugger/interpreter.h \
	debugger/script.h

.PHONY: FORCE check-symbols check-security
# bitcoin core #
BITCOIN_CORE_H = \
	$(LIBBITCOIN_DEB_H) \
	arith_uint256.h \
	attributes.h \
	base58.h \
	bech32.h \
	compat.h \
	compat/byteswap.h \
	compat/cpuid.h \
	compat/endian.h \
	consensus/amount.h \
	consensus/merkle.h \
	crypto/common.h \
	crypto/hmac_sha512.h \
	crypto/ripemd160.h \
	crypto/sha1.h \
	crypto/sha256.h \
	crypto/sha512.h \
	hash.h \
	policy/policy.h \
	prevector.h \
	primitives/transaction.h \
	pubkey.h \
	script/interpreter.h \
	script/script.h \
	script/script_error.h \
	serialize.h \
	span.h \
	streams.h \
	support/allocators/secure.h \
	support/allocators/zeroafterfree.h \
	support/cleanse.h \
	support/lockedpool.h \
	tinyformat.h \
	uint256.h \
	util/overflow.h \
	util/spanparsing.h \
	util/strencodings.h \
	util/vector.h \
	value.h \
	version.h

# debugger #

libbitcoin_deb_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libbitcoin_deb_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libbitcoin_deb_a_SOURCES = \
	debugger/hash.cpp \
	debugger/interpreter.cpp \
	debugger/script.cpp \
	$(BITCOIN_CORE_H)

# bitcoin: shared between all the tools
# libbitcoin_a_LIBADD = $(LIBSECP256K1)
libbitcoin_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libbitcoin_a_CXXFLAGS = $(AM_CXXFLAGS)
libbitcoin_a_SOURCES = \
	arith_uint256.cpp \
	base58.cpp \
	bech32.cpp \
	consensus/merkle.cpp \
	crypto/hmac_sha512.cpp \
	crypto/ripemd160.cpp \
	crypto/sha1.cpp \
	crypto/sha256.cpp \
	crypto/sha512.cpp \
	hash.cpp \
	primitives/transaction.cpp \
	pubkey.cpp \
	script/interpreter.cpp \
	script/script.cpp \
	script/script_error.cpp \
	support/cleanse.cpp \
	support/lockedpool.cpp \
	uint256.cpp \
	util/spanparsing.cpp \
	util/strencodings.cpp \
	value.cpp \
    $(BITCOIN_CORE_H)

# kerl library #
libkerl_a_CPPFLAGS = $(AM_CPPFLAGS) -Ikerl
libkerl_a_CFLAGS = -std=gnu99
libkerl_a_SOURCES = kerl/kerl.c kerl/kerl.h

# btcdeb binary #
btcdeb_SOURCES = \
	instance.h \
	instance.cpp \
	functions.h \
	functions.cpp \
	btcdeb.cpp \
	cliargs.h
btcdeb_CPPFLAGS = $(AM_CPPFLAGS)
btcdeb_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
btcdeb_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_AP_LDFLAGS)

btcdeb_LDADD = \
	$(LIBBITCOIN_DEB) \
	$(LIBBITCOIN) \
	$(LIBSECP256K1) \
	$(LIBKERL)

# tap binary #
tap_SOURCES = \
  instance.h \
  instance.cpp \
  functions.h \
  functions.cpp \
  tap.cpp \
  cliargs.h
tap_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
tap_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
tap_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_AP_LDFLAGS)

tap_LDADD = \
  $(LIBBITCOIN_DEB) \
  $(LIBBITCOIN) \
  $(LIBSECP256K1) \
  $(LIBKERL)

# btcc binary #
btcc_SOURCES = \
	btcc.cpp
btcc_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
btcc_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)

btcc_LDADD = \
	$(LIBBITCOIN_DEB) \
	$(LIBBITCOIN) \
	$(LIBSECP256K1)

# test-btcdeb binary #
test_btcdeb_SOURCES = \
	instance.h \
	instance.cpp \
	functions.h \
	functions.cpp \
	test/catch.hpp \
	test/catch.cpp \
	test/signing.cpp \
	test/test-btcdeb.cpp \
	test/utils.cpp \
	test/value.cpp
test_btcdeb_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
test_btcdeb_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
test_btcdeb_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_AP_LDFLAGS)

test_btcdeb_LDADD = \
	$(LIBBITCOIN_DEB) \
	$(LIBBITCOIN) \
	$(LIBKERL) \
	$(LIBSECP256K1)

clean-local:
	-rm -f config.h $(LIBBITCOIN) $(LIBKERL) $(LIBSECP256K1)

.rc.o:
	@test -f $(WINDRES)
	## FIXME: How to get the appropriate modulename_CPPFLAGS in here?
	$(AM_V_GEN) $(WINDRES) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(CPPFLAGS) -DWINDRES_PREPROC -i $< -o $@

check-symbols: $(bin_PROGRAMS)
if GLIBC_BACK_COMPAT
	@echo "Checking glibc back compat..."
	$(AM_V_at) READELF=$(READELF) CPPFILT=$(CPPFILT) $(top_srcdir)/contrib/devtools/symbol-check.py < $(bin_PROGRAMS)
endif

%.pb.cc %.pb.h: %.proto
	@test -f $(PROTOC)
	$(AM_V_GEN) $(PROTOC) --cpp_out=$(@D) --proto_path=$(<D) $<

