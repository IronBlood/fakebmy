BASEPATH = ..
include $(BASEPATH)/Makefile.Base
CC	= gcc -Wall -g
CFILE	:=uudecode.c strhash.c common.c numbyte.c strop.c fileop.c strlib.c shmop.c uuencode.c named_socket.c crypt.c limitcpu.c timeop.c smth_filter.c mgrep.c
COBJS	:=$(CFILE:.c=.o)
.c.o:	;  @echo "$(CC) $(CFLAGS) -c $*.c"
all: libytht.a

ifeq (.depend,$(wildcard .depend))
include .depend
endif

depend dep .depend:
	@echo "gcc $(CFLAGS) -MM *.c >.depend"

$(COBJS): .depend

libytht.a:$(COBJS)
	@echo "ar r $@ $?"
install:libytht.a
	@echo "mkdir -p $(BBSHOME)/bin"
	@echo "cp libytht.a $(BBSHOME)/bin"
clean:
	@echo "rm -fr $(COBJS) libytht.a"
