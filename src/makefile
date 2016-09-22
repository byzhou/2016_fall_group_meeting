flags 		= -shell-escape
build 		= build
UNAME_S 	:= $(shell uname -s )

file_name	:= latex-base-presentation

ifeq ($(UNAME_S),Darwin)
	target 	:= mac_view
else
	target 	:= linux_view
endif

default:all 
all: mac
	#pdflatex $(flags) hw-security-spectroscopy-dac2015.tex ;
bib:
	bibtex $(file_name);

view: $(target)

mac_view:
	open -a Preview build/$(file_name).pdf &
linux_view:
	@if which mupdf > /dev/null; then\
		mupdf build/$(file_name).pdf &\
	else\
		evince build/$(file_name).pdf &\
	fi

evince:
	evince build/$(file_name).pdf &
mac:
	pdflatex $(file_name)
	#bibtex $(file_name)
	cp $(file_name).aux build/
	~/colorlatexmk \
		-pdf \
		-latexoption="--shell-escape -halt-on-error -file-line-error" \
		-bibtex \
		-time \
		-outdir=$(build) \
		$(file_name).tex ;\
	pgrep mupdf | xargs -n 1 -I PID kill -SIGHUP PID 2>&1 > /dev/null
ghost:
	gs \
		-q \
		-dBATCH \
		-dNOPAUSE \
		-sDEVICE=pdfwrite \
		-sOutputFile=$(file_name).pdf  \
		-dPDFSETTINGS=/prepress \
		-dEmbedAllFonts=true \
		-dSubsetFonts=true \
		-dCompatibilityLevel=1.6 $(build)/$(file_name).pdf;\

formal: mac ghost
	
clean :
	rm -rf *.aux *.log *.nav *.snm *.toc *.out *.blg *.bbl *.bcf *.dvi *.dvi *.pdf *.xml *.fdb_latexmk *.fls *.auxlock
