## -*- text -*- ####################################################
#								   #
# Makefile for the GNU Readline and History Libraries.		   #
#								   #
####################################################################

# This Makefile is hand made from a template file, found in
# ../template.  Each library must provide several Makefile
# targets: `all', `clean', `documentation', `install', and
# `what-tar'.  The `what-tar' target reports the names of the
# files that need to be included in a tarfile to build the full
# code and documentation for this library.

# Please note that the values for INCLUDES, CC, AR, RM, CP,
# RANLIB, and selfdir are passed in from ../Makefile, and do
# not need to be defined here.
srcdir = .
VPATH = .:$(srcdir)

# Here is a rule for making .o files from .c files that doesn't force
# the type of the machine (like -sun3) into the flags.
.c.o:
	$(CC) -c $(CFLAGS) $(INCLUDES) $(LOCAL_DEFINES) $(CPPFLAGS) $<

#
# See the file STANDALONE for the -D defines that readline understands
#
LOCAL_DEFINES = $(LOCAL_INCLUDES)

# Some systems need to define this (like older NeXT systems) in order
# to prevent namespace conflicts.
CFLAGS = # -DSTATIC_MALLOC

# For libraries which include headers from other libraries.
LOCAL_INCLUDES = -I. -I..

# The name of the main library target.
LIBRARY_NAME = libreadline.a

# The C code source files for this library.
CSOURCES = $(srcdir)readline.c $(srcdir)funmap.c $(srcdir)keymaps.c \
	   $(srcdir)vi_mode.c $(srcdir)parens.c $(srcdir)rltty.c \
	   $(srcdir)complete.c $(srcdir)bind.c $(srcdir)isearch.c \
	   $(srcdir)display.c $(srcdir)signals.c $(srcdir)emacs_keymap.c \
	   $(srcdir)vi_keymap.c $(srcdir)history.c $(srcdir)tilde.c \
	   $(srcdir)xmalloc.c

# The header files for this library.
HSOURCES = readline.h rldefs.h chardefs.h keymaps.h history.h \
	   posixstat.h tilde.h

OBJECTS = readline.o vi_mode.o funmap.o keymaps.o parens.o search.o \
	  rltty.o complete.o bind.o isearch.o display.o signals.o \
	  history.o tilde.o xmalloc.o

# The texinfo files which document this library.
DOCSOURCE = doc/rlman.texinfo doc/rltech.texinfo doc/rluser.texinfo
DOCOBJECT = doc/readline.dvi
DOCSUPPORT = doc/Makefile
DOCUMENTATION = $(DOCSOURCE) $(DOCOBJECT) $(DOCSUPPORT)

SUPPORT = Makefile ChangeLog $(DOCSUPPORT) examples/[-a-z.]*

SOURCES  = $(CSOURCES) $(HSOURCES) $(DOCSOURCE)

THINGS_TO_TAR = $(SOURCES) $(SUPPORT)

##########################################################################

all: libreadline.a libhistory.a

libreadline.a: $(OBJECTS)
	$(RM) -f $@
	$(AR) cq $@ $(OBJECTS)
	-[ -n "$(RANLIB)" ] && $(RANLIB) $@

libhistory.a: history.o
	$(RM) -f $@
	$(AR) cq $@ history.o
	-[ -n "$(RANLIB)" ] && $(RANLIB) $@

documentation: force
	-mkdir doc
	(if [ -d doc ]; then cd doc; $(MAKE) $(MFLAGS); fi)
force:

# The rule for 'includes' is written funny so that the if statement
# always returns TRUE unless there really was an error installing the
# include files.
install: installdirs libreadline.a
	${INSTALL_DATA} readline.h keymaps.h chardefs.h history.h \
		$(incdir)/readline
	-${MV} $(libdir)/libreadline.a $(libdir)/libreadline.old
	${INSTALL_DATA} libreadline.a $(bindir)/libreadline.a
	-[ -n "$(RANLIB)" ] && $(RANLIB) -t $(bindir)/libreadline.a

installdirs:
	[ ! -d $(incdir)/readline ] && { \
	  mkdir $(incdir)/readline && chmod chmod 755 $(incdir)/readline; }

uninstall:
	cd $(incdir)/readline && ${RM} -f ${INSTALLED_HEADERS}
	cd $(libdir) && ${RM} -f libreadline.a libreadline.old

clean:
	rm -f $(OBJECTS) *.a
	(if [ -d doc ]; then cd doc; $(MAKE) $(MFLAGS) $@; fi)

readline: readline.h rldefs.h chardefs.h
readline: $(OBJECTS)
	$(CC) $(CFLAGS) $(CPPFLAGS) $(READLINE_DEFINES) \
	  $(LOCAL_INCLUDES) -DTEST -o readline readline.c vi_mode.o funmap.o \
	  keymaps.o -ltermcap

# Dependencies
readline.o: readline.c readline.h rldefs.h rlconf.h chardefs.h
readline.o: keymaps.h history.h
vi_mode.o:  rldefs.h rlconf.h readline.h history.h
funmap.o:   funmap.c readline.h rlconf.h
keymaps.o:  keymaps.c emacs_keymap.c vi_keymap.c keymaps.h chardefs.h rlconf.h
history.o: history.h memalloc.h
isearch.o: memalloc.h readline.h history.h
search.o: memalloc.h readline.h history.h
display.o: readline.h history.h rldefs.h rlconf.h
complete.o: readline.h rldefs.h rlconf.h
rltty.o: rldefs.h rlconf.h readline.h
bind.o: rldefs.h rlconf.h readline.h history.h
signals.o: rldefs.h rlconf.h readline.h history.h
parens.o: readline.h
