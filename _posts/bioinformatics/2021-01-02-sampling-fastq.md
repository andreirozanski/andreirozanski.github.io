---
layout: post
title:  "Sampling fastq files with seqtk"
tag: Bioinformatics
---

Using [seqtk](https://github.com/lh3/seqtk) to sample fastq files. 

One example of usage of seqtk sample for 5 mi of reads:

{% highlight bash %}

seqtk sample -s100 foo_R1.fastq.gz 5000000 |gzip - > foo_sample_5mi_s100_R1.fastq.gz &
seqtk sample -s100 foo_R2.fastq.gz 5000000 |gzip - > foo_sample_5mi_s100_R2.fastq.gz &

{% endhighlight %}


in that case, it is important to fix a seed for both files in order to keep proper pairing. 


___

## References

[seqtk](https://github.com/lh3/seqtk)


