TEX = pdflatex -shell-escape
BIB = bibtex

all: ODE

ODE: ODE.tex
	$(TEX) ODE
	$(TEX) ODE
	$(BIB) ODE
	$(BIB) ODE

clean:
	rm -f *.aux
	rm -f *.bbl
	rm -f *.blg
	rm -f *.log
	rm -f *.nav
	rm -f *.out
	rm -f *.snm
	rm -f *.toc
	rm -f *.pagelist
