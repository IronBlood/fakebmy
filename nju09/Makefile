BASEPATH = ..
include $(BASEPATH)/Makefile.Base
YTMPATH = $(BBSHOME)/wwwtmp/ytml
CGIPATH = /home/apache/cgi-bin/bbs
HTMPATH = /home/apache/htdocs/bbs

ifdef GPROF
LIBS	:= -lghthash -lz -L/usr/lib/mysql -lmysqlclient
CFLAGS	:= $(CFLAGS) $(OS_DEF) -DGPROF -pg -g
else
ifdef GPROF2
LIBS    := -lghthash -lz -L/usr/lib/mysql -lmysqlclient
CFLAGS  := $(CFLAGS) $(OS_DEF) -DGPROF -pg -g
else
LIBS	:= -lghthash -L/usr/lib/mysql -lmysqlclient
CFLAGS	:= $(CFLAGS) $(OS_DEF) -g
endif
endif
PROGNAME = www
CFILE	= bbsmain.c bbstop10.c bbsdoc.c bbscon.c bbsbrdadd.c bbsboa.c bbsall.c bbsanc.c bbs0an.c \
	bbslogout.c bbsleft.c bbslogin.c bbsbadlogins.c bbsqry.c bbsnot.c bbsfind.c \
	bbsfadd.c bbsfdel.c bbsfall.c bbsfriend.c bbsfoot.c bbsform.c bbspwd.c bbsplan.c bbsinfo.c \
	bbsmybrd.c bbssig.c bbspst.c bbsgcon.c bbsgdoc.c bbsmmdoc.c bbsdel.c bbsdelmail.c bbsmailcon.c bbsmail.c \
	bbsdelmsg.c bbssnd.c bbsnotepad.c bbsmsg.c bbssendmsg.c bbsreg.c \
	bbsmailmsg.c bbssndmail.c bbsnewmail.c bbspstmail.c bbsgetmsg.c bbscloak.c \
	bbsmdoc.c bbsnick.c bbstfind.c bbsadl.c bbstcon.c bbstdoc.c \
	bbsdoreg.c bbsmywww.c bbsccc.c bbsufind.c bbsclear.c bbsstat.c bbsedit.c bbsman.c bbsparm.c \
	bbsfwd.c bbsmnote.c bbsdenyall.c bbsdenydel.c bbsdenyadd.c \
	bbstopb10.c bbsbfind.c bbsx.c bbseva.c bbsvote.c \
	bbsshownav.c bbsbkndoc.c bbsbknsel.c bbsbkncon.c \
	bbshome.c bbsindex.c bbssechand.c bbsupload.c bbslform.c regreq.c \
	bbsselstyle.c bbscon1.c bbsattach.c bbskick.c \
	bbsscanreg.c bbsshowfile.c bbst.c bbsdt.c bbslt.c bbsincon.c bbssetscript.c \
	bbscccmail.c bbsfwdmail.c bbstmpl.c bbsrss.c bbsucss.c bbsdefcss.c bbssecfly.c bbssbs.c bbseditmail.c  bbsfindpass.c bbsresetpass.c bbsfindacc.c
	 CFILEBACKUP = bbsdmulan.c bbsdt.c bbsjs.c bbslt.c bbsmovie.c bbsnewleft.c bbssel.c bbst.c bbstty.c bbschat.c \
	      ytml.c ytml_func.c bclass.c bbsusr.c bbsalluser.c

COBJS	:=$(CFILE:.c=.o)

#OTHERS	= usesql.h BBSLIB.c boardrc.c deny_users.c bbsupdatelastpost.c bbsred.c tshirt.h
OTHERS = BBSLIB.c boardrc.c deny_users.c bbsupdatelastpost.c bbsred.c
.c.o:	;  @echo "$(CC) -c $*.c -D_GNU_SOURCE $(CFLAGS) $(CPPFLAGS) $(WWW_CPPFLAGS) -DYTMPATH='\"$(YTMPATH)\"' -DHTMPATH='\"$(HTMPATH)\"' -Wall $(LIBS)"

all: $(PROGNAME)

ifeq (.depend,$(wildcard .depend))
include .depend
endif

depend dep .depend:
	@echo "gcc $(CFLAGS) -M $(CFILE) $(OTHERS) upload.c >.depend"

proto proto.h:
	@echo "cproto -DMAKE_PROTO -D__GNUC__ $(CFILE) $(OTHERS) $(CPPFLAGS) $(WWW_CPPFLAGS) -I$(BASEPATH)/libythtbbs -I$(BASEPATH)/ythtlib -I$(BASEPATH)/include > proto.h"

$(COBJS):  .depend

$(PROGNAME):	$(COBJS) $(OTHERS) BBSLIB.o
ifdef GPROF
	@echo "$(CC)	--static -o $(PROGNAME)	$(COBJS) $(LIBS) BBSLIB.o -L$(BBSHOME)/bin -lytht  -lythtbbs -g -pg"
else
ifdef GPROF2
	@echo "$(CC)   -o $(PROGNAME)  $(COBJS) $(LIBS) BBSLIB.o -L$(BBSHOME)/bin -lytht -lythtbbs -g -pg -l"
else
	@echo "$(CC)	-o $(PROGNAME)	$(COBJS) $(LDFLAGS) $(LIBS) $(WWW_LIBS) BBSLIB.o $(BBSLIB)"
endif
endif
usesql.h: ~/etc/sysconf.ini
	@echo "echo \"#include <mysql/mysql.h>\" > usesql.h"
	@echo "cat ~/etc/sysconf.ini|grep SQL|sed -e 's/^/#define /;s/=//g' >> usesql.h"

install: $(PROGNAME) upload
	@echo "-mv $(CGIPATH)/$(PROGNAME) $(CGIPATH)/$(PROGNAME).old"
	@echo "cp $(PROGNAME) $(CGIPATH)"
	@echo "chmod +s $(CGIPATH)/$(PROGNAME)"
	@echo "cd $(CGIPATH);ln $(PROGNAME) attach -f"
	@echo "-mv $(CGIPATH)/upload $(CGIPATH)/upload.old"
	@echo "cp upload $(CGIPATH)"
	@echo "chmod +s $(CGIPATH)/upload"
	@echo "-cp -R html/* $(HTMPATH)"
	@echo "[ -d $(YTMPATH) ] || mkdir -p $(YTMPATH)"
	@echo "-cp ytml/*.ytml $(YTMPATH)"
	@echo "Finished!"
	@echo "-$(CGIPATH)/$(PROGNAME) &> /dev/null"
	@echo "Updated!"

update: $(PROGNAME) upload
	@echo "-mv $(CGIPATH)/$(PROGNAME) $(CGIPATH)/$(PROGNAME).old"
	@echo "cp $(PROGNAME) $(CGIPATH)"
	@echo "chmod +s $(CGIPATH)/$(PROGNAME)"
	@echo "cd $(CGIPATH);ln $(PROGNAME) attach -f"
	@echo "-mv $(CGIPATH)/upload $(CGIPATH)/upload.old"
	@echo "cp upload $(CGIPATH)"
	@echo "chmod +s $(CGIPATH)/upload"

getuser:getuser.c
	@echo "$(CC) $(CFLAGS) -o getuser $^ -L$(BBSHOME)/bin -lytht "
test_ytml:test_ytml.c ytml.c ytml_func.c
	@echo "$(CC) $(CFLAGS) -o test_ytml $^ -L$(BBSHOME)/bin -lytht"

upload: upload.c
	@echo "$(CC) $(CFLAGS) -o $@ $^ -L$(BBSHOME)/bin -lytht -lythtbbs"

clean:
	@echo "rm -rf $(COBJS) $(PROGRNAME) *.o"
checklist:=$(filter-out $(CFILE) $(OTHERS),$(shell ls *.c))
check:
	@echo "@echo $(checklist)"
