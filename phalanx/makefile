####### default places (can be overridden by "make prefix=/usr install" etc.)

prefix = /usr/local
exec_prefix = $(prefix)
bindir = $(exec_prefix)/bin
libdir = $(exec_prefix)/lib
pluginsdir = $(prefix)/share/games/plugins/xboard

####### choose the line that makes the binary faster on your machine
# CFLAGS = -O3 -Wall -fomit-frame-pointer -funroll-loops
CFLAGS = -O3 -Wall -mtune=native --std=gnu89 -D_GNU_SOURCE
# CFLAGS = -fprofile-generate -mtune=native -O3 -Wall --std=gnu89 -D_GNU_SOURCE
# CFLAGS = -fprofile-use -mtune=native -O3 -Wall --std=gnu89 -D_GNU_SOURCE
# CFLAGS = -O3 -Wall -fomit-frame-pointer --std=gnu89 -D_GNU_SOURCE
# -std=c99
#-Werror removed

####### debug/tuning options for developers
# CFLAGS = -O -Wall -g3 -static
# CFLAGS = -O -Wall -pg

#######
### DEFINES
### -DSHOW_FORCED_MOVES
### -DPBOOK_FILE=\"pbook.phalanx\"
### -DSBOOK_FILE=\"sbook.phalanx\"
### -DLEARN_FILE=\"learn.phalanx\"
### -DPBOOK_DIR=\"${libdir}\"
### -DSBOOK_DIR=\"${libdir}\"
### -DLEARN_DIR=\"/var/local/lib\"
### -DQCAPSONLY

DEFINES = -DGNUFUN
LDFLAGS =

OBJ = .o/phalanx.o .o/bcreate.o .o/search.o .o/io.o .o/data.o \
	   .o/evaluate.o .o/genmoves.o .o/moving.o .o/hash.o .o/static.o \
	   .o/levels.o .o/book.o .o/killers.o .o/endgame.o .o/learn.o .o/easy.o

phalanx: .o $(OBJ)
	$(CC) $(CFLAGS) $(DEFINES) $(LDFLAGS) $(OBJ) -o phalanx

.o/%.o: makefile phalanx.h %.c
	$(CC) $(CFLAGS) $(DEFINES) -c $*.c -o .o/$*.o

.o:
	mkdir .o

clean:
	rm -rf .o phalanx *.lrn learn.phalanx

backup:
	tar -czvf Archive/phalanx.tgz \
	makefile *.c *.h pbook.phalanx sbook.phalanx test.fin phalanx.eng

install: phalanx
	install -d 0755 $(DESTDIR)$(bindir)
	install -m 0755 phalanx $(DESTDIR)$(bindir)
	install -d 0755 $(libdir)
	install -m 0644 pbook.phalanx $(DESTDIR)$(libdir)
	install -m 0644 sbook.phalanx $(DESTDIR)$(libdir)
	install -d 0755 $(DESTDIR)$(pluginsdir)
	install -m 0644 phalanx.eng $(DESTDIR)$(pluginsdir)

uninstall:
	rm -f $(DESTDIR)$(bindir)/phalanx
	rm -f $(DESTDIR)$(libdir)/pbook.phalanx
	rm -f $(DESTDIR)$(libdir)/sbook.phalanx
	rm -f $(DESTDIR)$(pluginsdir)/phalanx.eng
