# This Makefile is for the Bash/documentation directory -*- text -*-.
#
CP	    = cp
RM	    = rm -f

DOC_SUPPORT = ../lib/doc-support/
TEXINDEX    = $(DOC_SUPPORT)texindex
TEXINDSRC   = $(DOC_SUPPORT)texindex.c
TEX	    = tex
QUIETPS	    = #set this to -q to shut up dvips
DVIPS	    = dvips -D 300 $(QUIETPS) -o $@	# tricky
TEXINPUTS   = ./../lib/readline/doc
MAKEINFO    = makeinfo

HSUSER      = ./../lib/readline/doc/hsuser.texinfo
RLUSER      = ./../lib/readline/doc/rluser.texinfo

.SUFFIXES:	.1 .3 .ms .ps .txt .dvi

.1.ps:
	$(RM) $@
	groff -man $*.1 > $@
.1.txt:
	$(RM) $@
	nroff -man $*.1 > $@

.ms.ps:
	$(RM) $@
	groff -ms $*.ms > $@
.ms.txt:
	$(RM) $@
	nroff -ms $*.ms > $@

.3.ps:
	$(RM) $@
	groff -man $*.3 > $@
.3.txt:
	$(RM) $@
	nroff -man $*.3 > $@

all: ps info dvi text

ps: bash.ps builtins.ps readline.ps article.ps
dvi: features.dvi features.ps 
info: features.info
text: bash.txt builtins.txt readline.txt article.txt

features.dvi: features.texi $(HSUSER) $(RLUSER)
	TEXINPUTS=.:$(TEXINPUTS):$$TEXINPUTS $(TEX) features.texi
	$(TEXINDEX) features.??
	TEXINPUTS=.:$(TEXINPUTS):$$TEXINPUTS $(TEX) features.texi

features.ps: features.dvi
	$(RM) $@
	$(DVIPS) $*.dvi

features.info: features.texi $(HSUSER) $(RLUSER)
	$(MAKEINFO) --no-split -I$(TEXINPUTS) features.texi

bash.dvi: $(TEXINDEX) bash.texinfo $(HSUSER) $(RLUSER)
	TEXINPUTS=.:$(TEXINPUTS):$$TEXINPUTS $(TEX) bash.texinfo
	$(TEXINDEX) bash.??
	TEXINPUTS=.:$(TEXINPUTS):$$TEXINPUTS $(TEX) bash.texinfo

bashman.ps: bash.dvi
	rm -f $@
	$(DVIPS) bash.dvi

bash.txt: bash.1
bash.ps: bash.1
builtins.ps: builtins.1 bash.1
builtins.txt: builtins.1 bash.1
readline.txt: readline.3
readline.ps: readline.3
article.ps: article.ms

$(TEXINDEX): $(TEXINDSRC)
	(cd $(DOC_SUPPORT); $(MAKE) $(MFLAGS) texindex)

hsuser.texinfo: ../lib/readline/doc/hsuser.texinfo
	ln -s ../lib/readline/doc/hsuser.texinfo .

rluser.texinfo: ../lib/readline/doc/rluser.texinfo
	ln -s ../lib/readline/doc/rluser.texinfo .

clean:
	rm -f *.aux *.cp *.fn *.ky *.log *.pg *.toc *.tp *.vr *.cps *.pgs \
	*.fns *.kys *.tps *.vrs *.o core texindex rluser.texinfo hsuser.texinfo

distclean:
	rm -f *.aux *.cp *.fn *.ky *.log *.pg *.toc *.tp *.vr *.cps *.pgs \
	*.dvi *.info *.info-* *.fns *.kys *.tps *.vrs *.o core texindex \
	rluser.texinfo hsuser.texinfo

realclean:	clean

install: all
	$(CP) bash.1 $(mandir)
	sed 's:so bash.1:so man1/bash.1:' < builtins.1 > $(mandir)/bash_builtins.1
	$(CP) readline.3 $(man3dir)
	$(CP) features.info $(infodir)/bash.info
