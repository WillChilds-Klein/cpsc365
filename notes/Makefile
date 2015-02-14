EXEC = pandoc
FLAGS = --mathjax
SRCS = $(wildcard *.md)
OBJS = $(patsubst %.md,%.pdf,$(SRCS))


all: $(OBJS)

%.pdf: %.md
	${EXEC} ${FLAGS} -o $@ $^

clean:
	rm -f *.o *.dSYM *notes*15.pdf .DS_Stores
