---
layout: post
title:  "Creating bigPsl track for UCSC genome browser"
---

Starting point is to have a [psl](https://genome.ucsc.edu/FAQ/FAQformat.html#format2) file. 

Its also needed to have in your path [pslToBigPsl](http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/pslToBigPsl), 
[bedToBigBed](http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/bedToBigBed), [bigPsl.as](https://genome.ucsc.edu/goldenPath/help/examples/bigPsl.as) and 
chrom.sizes file.

The Makefile to automatize it is:

{% highlight bash %}

SHELL=/bin/bash
.ONESHELL:

PSL=$(wildcard *.psl)
TXT:=$(addsuffix .txt, $(basename $(PSL)))
BB:=$(addsuffix .bb,$(basename $(TXT)))

.PHONY: all clear


all: $(BB)


%.txt: %.psl
        pslToBigPsl $< stdout | sort -k1,1 -k2,2n > $@

%.bb: %.txt
        wget -c https://genome.ucsc.edu/goldenPath/help/examples/bigPsl.as && \
        bedToBigBed -as=bigPsl.as -type=bed12+13 -tab $< chrom.sizes $@

clear:
        rm *.txt && \
        rm *.bb


{% endhighlight %}

A trackDb.ra snippet:


{% highlight bash %}

track FOOBAR
parent foo
shortLabel FOOBAR
longLabel FOOBAR
group genes
priority 4
visibility dense
color 51,153,255
altcolor 0,102,0
searchTable FOOBAR
searchType bigBed
searchMethod prefix
maxWindowToDraw 10000000
colorByStrand 50,50,150 150,50,50
type bigBed 6 +
bigDataUrl /gbdb/foo/bar
spectrum on

{% endhighlight %}

___

## References

[UCSC - bigPsl](https://genome.ucsc.edu/goldenPath/help/bigPsl.html)


