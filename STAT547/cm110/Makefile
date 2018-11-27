# Put the output file of your pipeline here. Make will go down its dependency chain and rebuild anything thats been changed
# If you have multiple outputs, put them all here so Make checks all the files in your pipeline
all: report.html

# "Make clean all" deletes all of the files specified in clean and then rebuilds all of them, i.e. fresh start
clean:
	rm -f words.txt histogram.tsv histogram.png report.html

words.txt:
	Rscript -e 'download.file("https://svnweb.freebsd.org/base/head/share/dict/web2?view=co", destfile = "words.txt", quiet = TRUE)'

histogram.tsv: histogram.R words.txt
# $< means refer to the first dependency
	Rscript $<
	
histogram.png: histogram.tsv
# $@ means refer to the target, i.e. histogram.png
	Rscript -e 'library(ggplot2); qplot(Length, Freq, data=read.delim("$<")); ggsave("$@")'
	rm -f Rplots.pdf

report.html: report.rmd histogram.tsv histogram.png
	Rscript -e 'rmarkdown::render("$<")'

# You can make other targets that build specific outputs. Just run command "make plots"
plots: histogram.png #anotherplot.png