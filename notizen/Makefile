OUTFILE = ../Vertiefung_Rechnernetze

all:
	sed -e 's: ,,: \\glqq{}:g' $(OUTFILE).md > temp.md
	# png ist in Markdown gut, aber eps ist in LaTeX besser. Beide Bilddateien noetig
	sed -i -e 's:\.png):\.eps):g' temp.md
	# in diesem Fall sind die Bilder im Notizen-Unterordner. Die LaTeX-Dateien aber auch!
	sed -i -e 's:](notizen/:](:g' temp.md
	pandoc temp.md -o temp.tex
	# Das ist notwendig, um in LaTeX hinterher gut auf Bilder \ref zu koennen und
	# man keine Markdown Bildangaben nimmt
	#sed -i -e 's:{\\label{img\:\([^}]*\)}:\[\1\]{\\label{img\:\1}:g' temp.tex
	latex -shell-escape _template.tex
	latex -shell-escape _template.tex
	dvips _template
	ps2pdf _template.ps $(OUTFILE).pdf

clean:
	rm -rf temp.md
	rm -rf temp.tex
	rm -rf _template.aux
	rm -rf _template.log
	rm -rf _template.nav
	rm -rf _template.toc
	rm -rf _template.snm
	rm -rf _template.pyg
	rm -rf _template.out
	rm -rf _template.vrb
	rm -rf _template.ps
	rm -rf _template.dvi
