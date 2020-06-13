
# Default values (should be supplied from calling script)
SRC?=src.tex
PREFIX?=outline
FILE?=$(PREFIX).tex
STOREPATH=../content/files/$(FILE)
NUM?=0

# Settings
SETTINGS=settings.tex

export TEXINPUTS:=../src//:${TEXINPUTS}

pdf: guard-SETTINGS guard-SRC guard-PREFIX
	pdflatex --jobname="$(PREFIX)" '\newif\ifweb\webfalse\newcommand{\LECTNUM}{$(NUM)}\input{$(SRC)}'
	pdflatex --jobname="$(PREFIX)" '\newif\ifweb\webfalse\newcommand{\LECTNUM}{$(NUM)}\input{$(SRC)}'
	@cp $(PREFIX).pdf ../outbox

www: guard-SETTINGS guard-SRC guard-PREFIX
	pdflatex --jobname="$(PREFIX)-website" '\newif\ifweb\webtrue\newcommand{\LECTNUM}{$(NUM)}\input{$(SRC)}'
	pdflatex --jobname="$(PREFIX)-website" '\newif\ifweb\webtrue\newcommand{\LECTNUM}{$(NUM)}\input{$(SRC)}'
	@cp $(PREFIX)-website.pdf ../outbox


guard-%:
	@ if [ "${${*}}" = "" ]; then \
		echo "Environment variable $* not set"; \
	exit 1; \
fi

copy: guard-FILE
	@cp -i $(SRC) $(STOREPATH)

store: copy
	@git add $(STOREPATH)
	@sleep 0.5
	@git commit -m "make store of $(FILE)"

load: guard-FILE
	@cp -i $(STOREPATH) $(SRC)

tidy:
	@rm -f *.aux
	@rm -f *.thm
	@rm -f *.log
	@rm -f *.out
	@rm -f *.snm
	@rm -f *.nav
	@rm -f *.pdf
	@rm -f *.blg
	@rm -f *.bbl
	@rm -f *.toc
	@rm -f *.vrb

snapshot: tidy
	@git add ..
	@sleep 0.5
	@read -p "Snapshot" STUFF; \
	git commit -m "Snapshot, working on $$STUFF"

