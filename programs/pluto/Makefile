# Pluto Makefile
# Copyright (C) 1997 Angelos D. Keromytis.
# Copyright (C) 1998-2001  D. Hugh Redelmeier
#
# This program is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by the
# Free Software Foundation; either version 2 of the License, or (at your
# option) any later version.  See <http://www.fsf.org/copyleft/gpl.txt>.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
# or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
# for more details.

# All of the USE_ and HAVE_ variables are controlled from openswan/Makefile.inc


# relative path to top directory of Openswan source
# Note: referenced in ${OPENSWANSRCDIR}/Makefile.inc
OPENSWANSRCDIR?=$(shell cd ../..; pwd)
srcdir?=./

include ${OPENSWANSRCDIR}/Makefile.inc
include ${srcdir}Makefile.options
-include ${srcdir}Makefile.extra
include ${srcdir}../Makefile.manpages

FMANDIR=$(MANTREE)/man5
PMANDIR=$(MANTREE)/man8

BINNAMEPLUTO = pluto
BINNAMEWHACKINIT = whackinit

OSDEP?=$(shell uname -s | tr 'A-Z' 'a-z')
SYSDEP_SRC=sysdep_${OSDEP}.c
SYSDEP_OBJ=sysdep_${OSDEP}.o

BUILDER=${shell echo ${USER}@`hostname` }
# End of configuration coping options.

CPPFLAGS = $(HDRDIRS) $(DEFINES) \
	-DSHARED_SECRETS_FILE=\"${FINALCONFDIR}/ipsec.secrets\" \
	-DPOLICYGROUPSDIR=\"${FINALCONFDDIR}/policies\" \
	-DPERPEERLOGDIR=\"${FINALLOGDIR}/pluto/peer\"

# must turn this off due to myid.c
CFLAGS+= -Wno-error=cast-qual

# some files need -Wno-error-cpp, calculated in Makefile.common
CFLAGS+=${GCC_CPP_FLAG}

ifeq ($(HAVE_BROKEN_POPEN),true)
CFLAGS+=-DHAVE_BROKEN_POPEN
endif

ifeq ($(HAVE_NO_FORK),true)
CFLAGS+=-DHAVE_NO_FORK
endif

# Enable ALLOW_MICROSOFT_BAD_PROPOSAL
CFLAGS+=-DALLOW_MICROSOFT_BAD_PROPOSAL

ALLFLAGS = $(CPPFLAGS) $(CFLAGS) ${CROSSFLAGS}

ifneq ($(LD_LIBRARY_PATH),)
LDFLAGS+=-L$(LD_LIBRARY_PATH)
endif

RM = /bin/rm
RMFLAGS = -f

.SUFFIXES:
.SUFFIXES: .c .o

# files for a (source) distribution

DISTMISC = CHANGES PLUTO-CONVENTIONS TODO ipsec.secrets Makefile routing.txt \
	 pluto.8 ipsec.secrets.5 .cvsignore

DISTGCRYPT = \
	gcryptfix.c gcryptfix.h \
	dsa.c dsa.h \
	elgamal.c elgamal.h \
	primegen.c \
	smallprime.c

DISTSRC = \
        connections.c initiate.c terminate.c connections.h \
	pending.c pending.h \
	foodgroups.c foodgroups.h \
	cookie.c cookie.h \
	crypto.h crypto.c \
	db_ops.c \
	demux.c demux.h msgdigest.c \
	dnskey.c dnskey.h \
	dpd.c dpd.h \
	myid.c hmac.c \
	hostpair.c \
	ipsec_doi.c ipsec_doi.h ikev1.c ikev1_quick.c ikev1_continuations.h \
	ikev1_crypto.c ikev1_nss.c \
        ikev2.c ikev2_parent.c ikev2_parent_I1.c ikev2_parent_R1.c ikev2_child.c ikev2_derived_keys.c ikev2_notify.c \
	ikev2_prfplus.c spdb_v2_struct.c \
	kernel.c kernel.h \
	${NETKEY_SRCS} \
	${KLIPS_SRCS} ${MAST_SRCS} ${WIN2K_SRCS} \
	kernel_noklips.c kernel_noklips.h \
	ike_alg.c ike_alg_status.c ike_alg.h ikeping.c \
	ike_alg_aes.c ike_alginit.c ikev2_rsa.c ikev2_psk.c ikev2_x509.c  \
	rcv_whack.c rcv_whack.h \
	$(IPSECPOLICY_DIST_SRCS) \
	${EXTRA_CRYPTO_SRCS} ike_alg_sha2.c \
	log.c log.h \
	plutomain.c plutoalg.c \
	pluto_crypt.c crypt_utils.c pluto_crypt.h \
	build_ke.c crypt_ke.c crypt_dh.c crypt_start_dh.c \
	keys.c \
	server.c server.h \
	spdb.c spdb_struct.c spdb_v1_struct.c spdb_print.c spdb.h \
	state.c state.h \
	stubs.c \
	${SYSDEP_SRC} \
	timer.c timer.h \
	$(DISTGCRYPT) \
	$(X509_DIST_SRCS) \
	$(HAVE_THREADS_DIST_SRCS) \
	vendor.c nat_traversal.c virtual.c \
	adns.c adns.h \
	whackinit.c \
	security_selinux.c \
	${XAUTH_DIST_SRCS} \
	${AGGRESSIVE_DIST_SRCS}

DIST = $(DISTMISC) $(DISTSRC)


# start of support for DSS/DSA.  Not currently used.
# OBJSGCRYPT =  gcryptfix.o dsa.o elgamal.o primegen.o smallprime.o
OBJSGCRYPT =

OBJSPLUTO  = connections.o initiate.o terminate.o
OBJSPLUTO += pending.o cookie.o crypto.o dpd.o
OBJSPLUTO += foodgroups.o log.o state.o plutomain.o plutoalg.o server.o
OBJSPLUTO += timer.o hmac.o hostpair.o
OBJSPLUTO += myid.o ipsec_doi.o
OBJSPLUTO += ikev1.o ikev1_main.o   ikev1_quick.o
OBJSPLUTO += ikev2.o ikev2_parent.o ikev2_child.o spdb_v2_struct.o ikev2_notify.o
OBJSPLUTO += ikeping.o kernel.o
OBJSPLUTO += $(NETKEY_OBJS) $(BSDKAME_OBJS) ${KLIPS_OBJS} ${MAST_OBJS} ${WIN2K_OBJS} ${PFKEYv2_OBJS}
OBJSPLUTO += kernel_noklips.o rcv_whack.o
OBJSPLUTO += ${IPSECPOLICY_OBJS} demux.o msgdigest.o keys.o dnskey.o
OBJSPLUTO += pluto_crypt.o crypt_utils.o build_ke.o crypt_ke.o crypt_dh.o crypt_start_dh.o
OBJSPLUTO += ikev2_derived_keys.o ikev2_prfplus.o
OBJSPLUTO += spdb.o spdb_struct.o spdb_v1_struct.o spdb_print.o security_selinux.o
OBJSPLUTO += vendor.o nat_traversal.o virtual.o
OBJSPLUTO += ike_alg_aes.o ike_alginit.o ikev2_rsa.o ikev2_psk.o ikev2_x509.o
OBJSPLUTO += ${EXTRA_CRYPTO_OBJS} ike_alg_sha2.o
OBJSPLUTO += ike_alg.o ike_alg_status.o db_ops.o
OBJSPLUTO += ${XAUTH_OBJS}
OBJSPLUTO += ${AGGRESSIVE_OBJS} ${CRYPTO_OBJS}
OBJSPLUTO += ${X509_OBJS} ${HAVE_THREADS_OBJS}
OBJSPLUTO += ${OBJSLIBPLUTO}
OBJSPLUTO += ${SYSDEP_OBJ} stubs.o
OBJSPLUTO += adns.o

OBJSWHACKINIT = whackinit.o

# we can't do tests with two conditions, but we can concatenate the strings
SHOULDWERESTART=${USE_OBJDIR}$(wildcard plutomain.c)
ifeq ($(SHOULDWERESTART),trueplutomain.c)
all programs clean install:
	cd ${OPENSWANSRCDIR} && cd ${OBJDIRTOP}/programs/pluto && $(MAKE) $@
else
all: $(BINNAMEPLUTO) #$(BINNAMEWHACKINIT)
programs: $(BINNAMEPLUTO) #$(BINNAMEWHACKINIT)
clean:	cleanall
install: doinstall
endif

doinstall: all
	mkdir -p ${LIBEXECDIR} ${LIBDIR}
	mkdir -p -m 755 $(CONFDIR)/ipsec.d
	mkdir -p -m 755 $(CONFDIR)/ipsec.d/cacerts
	mkdir -p -m 755 $(CONFDIR)/ipsec.d/aacerts
	mkdir -p -m 755 $(CONFDIR)/ipsec.d/ocspcerts
	mkdir -p -m 755 $(CONFDIR)/ipsec.d/certs
	mkdir -p -m 755 $(CONFDIR)/ipsec.d/crls
	mkdir -p -m 700 $(CONFDIR)/ipsec.d/private
	mkdir -p -m 700 $(VARDIR)/run/pluto
	$(INSTALL) $(INSTBINFLAGS) $(BINNAMEPLUTO) $(LIBEXECDIR)
	#$(INSTALL) $(INSTSUIDFLAGS) $(BINNAMEWHACKINIT) $(LIBEXECDIR)
	$(INSTALL) $(INSTMANFLAGS) ${srcdir}pluto.8 $(PMANDIR)/ipsec_pluto.8
	sh ${OPENSWANSRCDIR}/packaging/utils/manlink ${srcdir}pluto.8 | \
		while read from to ; \
		do \
			ln -s -f ipsec_$$from $(PMANDIR)/$$to; \
		done
	$(INSTALL) $(INSTMANFLAGS) ${srcdir}ipsec.secrets.5 $(FMANDIR)
	sh ${OPENSWANSRCDIR}/packaging/utils/manlink ${srcdir}ipsec.secrets.5 | \
		while read from to ; \
		do \
		 ln -s -f $$from $(FMANDIR)/$$to; \
		done

install_file_list:
	@echo $(LIBEXECDIR)/$(BINNAMEPLUTO)
	@echo $(PMANDIR)/ipsec_pluto.8
	@sh ${OPENSWANSRCDIR}/packaging/utils/manlink pluto.8 | \
		while read from to; \
		do\
			 echo $(PMANDIR)/$$to; \
		done
	@echo $(FMANDIR)/ipsec.secrets.5
	@sh ${OPENSWANSRCDIR}/packaging/utils/manlink ipsec.secrets.5 | \
		while read from to; \
		do \
			echo $(FMANDIR)/$$to; \
		done

alg_info_test: alg_info_test.o ike_alg.o constants.o defs.o log.o db_ops.o crypto.o $(LIBDESLITE) ${LIBOPENSWAN}
	$(CC) -o $@ $^ $(LDFLAGS) $(LIBSPLUTO)

${LIBTPM}: $(wildcard ${srcdir}tpm/*.c) $(wildcard ${srcdir}tpm/*.h) $(wildcard ${srcdir}tpm/*.i)
	@(cd tpm && ${MAKE} srcdir=${srcdir}tpm/ libtpm.a )

$(BINNAMEPLUTO): $(OBJSPLUTO) $(ALG_LIBS) ${LIBOPENSWAN} ${LIBTPM}
	@echo LD ${BINNAMEPLUTO}
	@$(CC) -o $(BINNAMEPLUTO) $(LDFLAGS) $(USERLINK) $(OBJSPLUTO) $(LIBSPLUTO) ${PLUTOMINUSL} $(LIBDESLITE)

$(BINNAMEWHACKINIT): $(OBJSWHACKINIT)
	@echo LD ${BINNAMEWHACKINIT}
	@$(CC) -o $(BINNAMEWHACKINIT) $(LDFLAGS) $(OBJSWHACKINIT) $(USERLINK) $(LIBSWHACK)

distlist:
	@echo $(DIST)

cleanall:
	$(RM) $(RMFLAGS) *.core core *~ a.out ktrace.out \
		$(OBJSPLUTO) $(BINNAMEPLUTO)

distclean: clean

mostlyclean: clean

realclean: clean

options:
	echo ${USERCOMPILE} ${PORTINCLUDE} ${COPTS} ${ALLFLAGS}

check:
	echo no checks in lib right now.

checkprograms:

.c.o:
	@echo CC $(notdir $<)
	@$(CC) $(USERCOMPILE) ${PORTINCLUDE} $(COPTS) $(ALLFLAGS) -c $<

%.E: %.c
	@echo CPP $(notdir $<)
	@$(CC) -E $(USERCOMPILE) ${PORTINCLUDE} $(COPTS) $(ALLFLAGS) -c $< -o $@

%.s: %.c
	@echo CC-TO-AS $(notdir $<)
	@$(CC) -S $(USERCOMPILE) ${PORTINCLUDE} $(COPTS) $(ALLFLAGS) -c $<

%.i: %.c
	@echo CC $(notdir $<)
	@$(CC) $(USERCOMPILE) ${PORTINCLUDE} $(COPTS) $(ALLFLAGS) -E -o $@ $<


# Gather dependencies caused by explicit #includes within .c files
depend:
	@(ls $(DISTSRC) | grep '\.c' | xargs $(CC) -MM ${COPTS} ${PORTINCLUDE} ${ALLFLAGS} ) | sed -e 's,${OPENSWANSRCDIR},../..,g' >Makefile.depend.${OSDEP}

-include ${srcdir}Makefile.depend.${OSDEP}

# DO NOT DELETE

