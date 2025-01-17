CXXFLAGS=-g -O3 -Wall -Werror -Wno-error=unknown-pragmas -std=c++11 -pthread -I./include -I./vendor
LDLIBS=-lz

all: indexlr

clean:
	rm -f indexlr

# Check the C++ source code for errors.
lint: clang-format clang-tidy

# Check the C++ source code for errors with clang-tidy.
clang-tidy:
	clang-tidy -warnings-as-errors='*' include/*.h *.cc -- -std=c++11 -x c++ -I./include -I./vendor

# Check the C++ source code for white-space errors with clang-format.
clang-format:
	for i in *.cc include/*.h; do clang-format -style=file $$i >$$i.fixed; done
	for i in *.cc include/*.h; do diff -su $$i $$i.fixed && rm -f $$i.fixed; done
	if ls *.fixed; then exit 1; fi

# Fix white-space errors with clang-format.
fix:
	for i in *.cc; do clang-format -i -style=file $$i; done

check: check-index-fasta check-index-pos-fasta check-index-pos-strand-fasta check-index-fastq

check-index-fasta: all
	./indexlr -t16 -k100 -w5 data/mt.fa | diff -q - data/mt.physlr.tsv.good

check-index-pos-fasta: all
	./indexlr -t16 -k100 -w5 --pos data/mt.fa | diff -q - data/mt.pos.physlr.tsv.good

check-index-pos-strand-fasta: all
	./indexlr -t16 -k100 -w5 --pos --strand data/mt.fa | diff -q - data/mt.pos.strand.physlr.tsv.good

check-index-fastq: all
	./indexlr -t16 -k100 -w5 data/tiny.fq | diff -q - data/tiny.physlr.tsv.good
