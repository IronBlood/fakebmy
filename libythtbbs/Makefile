BASEPATH = ..
include $(BASEPATH)/Makefile.Base
CFILE	:=boardrc.c misc.c record.c user.c article.c modetype.c docutil.c msg.c politics.c goodgbid.c binaryattach.c permissions.c bbseva.c attach.c regform.c announce.c board.c sectree.c sectreeop.c libiden.c logging.c
COBJS	:=$(CFILE:.c=.o)
.c.o:	; @echo "$(CC) -c $*.c -D_GNU_SOURCE $(CFLAGS) $(OS_DEF) -g -L/usr/lib/mysql -lmysqlclient -I/usr/include/libxml2"
all: libythtbbs.a

ifeq (.depend,$(wildcard .depend))
include .depend
endif

depend dep .depend:
	@echo "gcc $(CFLAGS) -M *.c >.depend"

$(COBJS): .depend

libythtbbs.a:$(COBJS)
	@echo "ar r $@ $?"
install:libythtbbs.a
	@echo "mkdir -p $(BBSHOME)/bin"
	@echo "cp libythtbbs.a $(BBSHOME)/bin"
clean:
	@echo "rm -fr $(COBJS) libythtbbs.a seclist.local"
seclist.local:
	if [ -f ../site/seclist.txt ];then echo "ln -sf ../site/seclist.txt seclist.local";else echo "ln -sf seclist.txt seclist.local";fi
sectree.c: gensectree seclist.local sectree.h
	@echo "./gensectree> sectree.c"
gensectree: gensectree.o
	@echo "$(CC) $(CFLAGS) -o $@ $^ -L$(BBSHOME)/bin -lytht"
