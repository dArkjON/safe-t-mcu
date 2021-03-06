APPVER = 1.0.0

NAME  = trezor

ifeq ($(EMULATOR),1)
OBJS += udp.o
else
OBJS += usb.o
OBJS += bl_check.o
endif

OBJS += u2f.o
OBJS += messages.o
OBJS += storage.o
OBJS += trezor.o
OBJS += pinmatrix.o
OBJS += fsm.o
OBJS += coins.o
OBJS += coin_info.o
OBJS += transaction.o
OBJS += protect.o
OBJS += layout2.o
OBJS += recovery.o
OBJS += reset.o
OBJS += signing.o
OBJS += crypto.o
OBJS += ethereum.o
OBJS += ethereum_tokens.o
OBJS += nem2.o
OBJS += nem_mosaics.o
OBJS += gettext.o

OBJS += debug.o

OBJS += ../vendor/trezor-crypto/address.o
OBJS += ../vendor/trezor-crypto/bignum.o
OBJS += ../vendor/trezor-crypto/ecdsa.o
OBJS += ../vendor/trezor-crypto/curves.o
OBJS += ../vendor/trezor-crypto/secp256k1.o
OBJS += ../vendor/trezor-crypto/nist256p1.o
OBJS += ../vendor/trezor-crypto/rand.o
OBJS += ../vendor/trezor-crypto/memzero.o

OBJS += ../vendor/trezor-crypto/ed25519-donna/curve25519-donna-32bit.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/curve25519-donna-helpers.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/modm-donna-32bit.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/ed25519-donna-basepoint-table.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/ed25519-donna-32bit-tables.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/ed25519-donna-impl-base.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/ed25519.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/curve25519-donna-scalarmult-base.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/ed25519-sha3.o
OBJS += ../vendor/trezor-crypto/ed25519-donna/ed25519-keccak.o

OBJS += ../vendor/trezor-crypto/hmac.o
OBJS += ../vendor/trezor-crypto/bip32.o
OBJS += ../vendor/trezor-crypto/bip39.o
OBJS += ../vendor/trezor-crypto/pbkdf2.o
OBJS += ../vendor/trezor-crypto/base32.o
OBJS += ../vendor/trezor-crypto/base58.o
OBJS += ../vendor/trezor-crypto/segwit_addr.o

OBJS += ../vendor/trezor-crypto/ripemd160.o
OBJS += ../vendor/trezor-crypto/sha2.o
OBJS += ../vendor/trezor-crypto/sha3.o
OBJS += ../vendor/trezor-crypto/blake256.o
OBJS += ../vendor/trezor-crypto/hasher.o
OBJS += ../vendor/trezor-crypto/groestl.o

OBJS += ../vendor/trezor-crypto/aes/aescrypt.o
OBJS += ../vendor/trezor-crypto/aes/aeskey.o
OBJS += ../vendor/trezor-crypto/aes/aestab.o
OBJS += ../vendor/trezor-crypto/aes/aes_modes.o

OBJS += ../vendor/trezor-crypto/nem.o

OBJS += ../vendor/trezor-qrenc/qr_encode.o

OBJS += ../vendor/nanopb/pb_common.o
OBJS += ../vendor/nanopb/pb_decode.o
OBJS += ../vendor/nanopb/pb_encode.o

OBJS += protob/messages.pb.o
OBJS += protob/types.pb.o

include ../Makefile.include

DEBUG_LINK ?= 0
DEBUG_LOG  ?= 0
DEBUG_GDB  ?= 0

UPDATE_BOOTLOADER ?= 0

CFLAGS += -Wno-sequence-point
CFLAGS += -I../vendor/nanopb -Iprotob -DPB_FIELD_16BIT=1
CFLAGS += -DQR_MAX_VERSION=0
CFLAGS += -DDEBUG_LINK=$(DEBUG_LINK)
CFLAGS += -DDEBUG_LOG=$(DEBUG_LOG)
CFLAGS += -DDEBUG_GDB=$(DEBUG_GDB)
CFLAGS += -DUPDATE_BOOTLOADER=$(UPDATE_BOOTLOADER)
CFLAGS += -DSCM_REVISION='"$(shell git rev-parse HEAD | sed 's:\(..\):\\x\1:g')"'
CFLAGS += -DUSE_ETHEREUM=1
CFLAGS += -DUSE_NEM=1

# Dependencies for the translations
intl/intl.h: intl/de.h intl/fr.h

intl/%.h: po/%.po
	echo make headers
	$(MAKE) -C po headers

define GENERATE_CODE
# $(1) - Basename for script and output header file
# $(2) - Dependencies
# $(3) - Additional output files

ifneq ($(3),)
$(3): $(1).h
endif

$(1).h: $(1).py $(2)
	$(PYTHON) $(1).py

clean::
	rm -f $(1).h $(3)
endef

$(eval $(call GENERATE_CODE,coin_info,coins.json,coin_info.c))
$(eval $(call GENERATE_CODE,nem_mosaics,nem_mosaics.json,nem_mosaics.c))
$(eval $(call GENERATE_CODE,bl_data,../bootloader/bootloader.bin))
