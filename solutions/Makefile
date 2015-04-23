EXEC = pandoc
FLAGS = --mathjax -s --template="/Users/will/.pandoc/templates/cpsc365-template.tex"
SRCS = $(wildcard *.md)
OBJS = $(patsubst %.md,%.md.pdf,$(SRCS))


all: $(OBJS)

%.md.pdf: %.md
	${EXEC} ${FLAGS} -o $@ $^

%: %.md
	${EXEC} ${FLAGS} -o $@.md.pdf $^ && open -a Preview $@.md.pdf

clean:
	rm -f *.o *.dSYM *.md.pdf .DS_Stores
