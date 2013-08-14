#################################################################
# Makefile for FIREBIRD BBS v3.0K                               #
#################################################################
# 1. This package is derived from Firebird BBS 1.5, which was
#    developed by the Department of Computer Science & Information
#    Engineering, Chung Chen University of Taiwan, ROC.
#    This package is developed by the Board of System Administrators,
#    Malaysian Students' Association In Taiwan.
#
# 2. Our BBS location is at MSIA.pine.ncu.edu.tw (140.115.210.77)
#
# 3. The developers of this package can be reached by
#    SYSOP.bbs@MSIA.pine.ncu.edu.tw
#
# 4. This Makefile is for "LINUX"

#############################################################
####   Setup the following site-specific information     ####
#############################################################

BASEPATH = ..
include $(BASEPATH)/Makefile.Base

#############################################################
###        You needn't modify the following stuff      ######
#############################################################
PROGNAME = bbs
CFILE    = bbs.c record.c delete.c main.c stuff.c mail.c register.c \
           xyz.c talk.c read.c comm_lists.c namecomplete.c chat.c \
           maintain.c edit.c more.c help.c bcache.c boards.c \
           smth_screen.c io.c term.c userinfo.c vote.c announce.c sendmsg.c\
	   bm.c list.c goodbye.c bbsgopher.c fileshm.c postheader.c \
	   convcode.c five.c tmachine.c addressbook.c\
	   backnumber.c common.c sec_hand.c regular.c 1984.c\
	   moneycenter.c editboard.c power_select.c tmpl.c logger.c identify.c

COBJS    :=$(CFILE:.c=.o)

#COBJSTEST:=$(CFILE:.c=.t)

OTHERS   =

GARBAGE  = a.out core installchatd bbs.chatd bbsd thread

# The -DINVISIBLE makes bbs users invisible to 'finger' and 'who'.
# Simply remove it if you don't want this feature.

#SITE_DEF = -DBBSHOME='"$(BBSHOME)"' -DBBSUID=$(BBSUID) -DBBSGID=$(BBSGRP)

#DEFINES	=  -DBBSMAIN $(CSIE_DEF) $(OS_DEF)

.SUFFIXES: .o .c .ln .t

.c.o:   ;   @echo "$(CC) $(CFLAGS) -include proto.h -c $*.c"

#.c.t:   ;   @echo "$(CC) -D_GNU_SOURCE $(CFLAGSTEST) $(CPPFLAGS) $(DEFINES) -DBACK_DELETE_RANGE -include proto.h -c $*.c -o $*.t"

#--------------------- Dependency starts here -------------------
all: $(PROGNAME) bbs.chatd bbsd telnet thread

ifeq (.depend,$(wildcard .depend))
include .depend
endif

#Makefile: Makefile.in $(BASEPATH)/configure
#	@echo Makefile is older than Makefile.in or $(BASEPATH)/configure, run $(BASEPATH)/configure first.
#	@false

depend dep .depend:
	@echo "gcc $(CFLAGS) -MM *.c >.depend"
	@echo "gcc $(CFLAGSTEST) -MM *.c |sed -es/"o:"/"t:"/ >>.depend"

proto proto.h:
	@echo "echo '#include "bbs.h"' > proto.h"
	@echo "cproto $(CFILE) -I $(BASEPATH)/include -I $(BASEPATH)/ythtlib -I $(BASEPATH)/libythtbbs >> proto.h"

$(COBJS) $(COBJSTEST): .depend proto.h

$(PROGNAME): $(COBJS) $(OTHERS)
	@echo "$(CC) $(COBJS) $(LIBS) $(BBSLIB) -o $(PROGNAME)"

#bbstest: $(COBJSTEST) $(OTHERS)
#	@echo "$(CC) -o bbstest $(CFLAGSTEST) $(COBJSTEST) $(LIBSTEST)  $(BBSLIB)"

bbsd: bbsd.c
	@echo "$(CC) $(CFLAGS) $(BBSLIB) bbsd.c -o bbsd"

bbsnet: bbsnet.c
	@echo "$(CC) bbsnet.c -o bbsnet"

telnet: telnet0.c
	@echo "$(CC) -g telnet0.c -o telnet"

bbs.chatd: station.c
	@echo "$(CC) $(CFLAGS) -o bbs.chatd $(OS_DEF) station.c"

thread: thread.c record.c
	@echo "$(CC) $(CFLAGS) -include $(BASEPATH)/ythtlib/ythtlib.h -include $(BASEPATH)/libythtbbs/ythtbbs.h -DEXT_UTL -o thread record.c thread.c -L$(BBSHOME)/bin -lytht -lythtbbs"

install: all
	#( cd $(BASEPATH); ./Install.sh)
	@echo "$(MAKE) installbbs"
	@echo "$(MAKE) installbbsd"
	@echo "$(MAKE) installthread"
	@echo "$(MAKE) installchatd"
	@echo "$(MAKE) installbbsnet"
	@echo "$(MAKE) installtelnet"

installbbs: $(PROGNAME) $(BBSHOME)/bin/bbs
$(BBSHOME)/bin/bbs: bbs
	@echo "cp bbs $@.r"
	@echo "$(INSTALL) -s -m 550 -g $(BBSGRP) -o $(BBSUID) bbs $@.new"
	@echo "-rm -f $@.old"
	@echo "-mv $@ $@.old"
	@echo "mv $@.new $@"

installbbstest: bbstest $(BBSHOME)/bin/bbstest
$(BBSHOME)/bin/bbstest: bbstest
	@echo "cp bbstest $@.new"
	@echo "-rm -f $@.old"
	@echo "-mv $@ $@.old"
	@echo "mv $@.new $@"

installthread: thread $(BBSHOME)/bin/thread
$(BBSHOME)/bin/thread: thread
	@echo "$(INSTALL) -s -m 550 -g $(BBSGRP) -o $(BBSUID) thread $@.new"
	@echo "-rm -f $@.old"
	@echo "-mv $@ $@.old"
	@echo "mv $@.new $@"

installchatd: bbs.chatd $(BBSHOME)/bin/chatd
$(BBSHOME)/bin/chatd: bbs.chatd
	@echo "$(INSTALL) -s -m 550 -g $(BBSGRP) -o $(BBSUID) bbs.chatd $@.new"
	@echo "-rm -f $@.old"
	@echo "-mv $@ $@.old"
	@echo "mv $@.new $@"

installbbsd: bbsd
	@echo "$(INSTALL) -s -m 550 -g $(BBSGRP) -o $(BBSUID) bbsd $(BBSHOME)/bin/bbsd"

installbbsnet: bbsnet
	@echo "$(INSTALL) -s -m 550 -g $(BBSGRP) -o $(BBSUID) bbsnet $(BBSHOME)/bin/bbsnet.new"
	@echo "-rm -f $(BBSHOME)/bin/bbsnet.old"
	@echo "-mv $(BBSHOME)/bin/bbsnet $(BBSHOME)/bin/bbsnet.old"
	@echo "mv $(BBSHOME)/bin/bbsnet.new $(BBSHOME)/bin/bbsnet"

installtelnet: telnet
	@echo "$(INSTALL) -s -m 550 -g $(BBSGRP) -o $(BBSUID) telnet $(BBSHOME)/bin/telnet.new"
	@echo "-rm -f $(BBSHOME)/bin/telnet.old"
	@echo "-mv $(BBSHOME)/bin/telnet $(BBSHOME)/bin/telnet.old"
	@echo "mv $(BBSHOME)/bin/telnet.new $(BBSHOME)/bin/telnet"

clean:
	@echo "rm -fr *.o *.BAK *.log"
#	rm -fr ../lib/*.o ../lib/*.BAK ../lib/*.log
#	rm -rf core 'find .. type f -name 'core' -print'
#	rm -f `find .. -name '*.[oas]' ! -regex '.*lxdialog/.*' -print`
	@echo "-rm -fr $(GARBAGE) $(COBJS) $(COBJSTEST) $(PROGNAME) $(LNFILES) bbsd bbs.chatd thread pty/ptyexec"
	@echo "-rm -fr core .depend"

dist_clean: clean
	@echo "-rm -f CONFIG/config.log"
	@echo "-rm -f CONFIG/config.status"
	@echo "-rm -f CONFIG/config.cache"
	@echo "-rm -f Install.sh"

testregular:testregular.c regular.c
	@echo "gcc -o $@ -g $^ -I../include"

tags: /tmp
	@echo "ctags $(CFILE)"
