## Pacbio-Brick by Brick!
A handy list of tools, blogs, forums, resources and much more dedicated to Pacbio data analysis.

> This is under construction; at the end, everything will make sense. Keep Calm..

#### How does Pacbio sequencing works?

Link to Youtube video animation

<a href="http://www.youtube.com/watch?feature=player_embedded&v=y3OOUhQGFeE" target="_blank"><img src="http://img.youtube.com/vi/y3OOUhQGFeE/0.jpg" alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

#### Which quality score encoding does PacBio use?

PacBio does use `PHRED 33`, but it turns out the question may be irrelevant for the newer PacBio Sequel Sequencer, because it reports all base qualities as `PHRED 0 (ASCII !)`. The RS-II reports `PHRED 33` quality scores.

The Sequel provides data in the form of subreads, which are the circular consensus sequences (CCS) from a single zero-mode waveguide (ZMW). Essentially, a single circularized DNA molecule enters a ZMW well, and is sequenced as many times as possible. (Sometimes multiple molecules get into a single well, but that's not the normal case.) The number of times this single molecule can be sequenced depends on several variables, including molecule length. The consensus sequence for this single molecule is then reported out as a subread.

As I started playing with the quality scores in my data, I realized the quality score for every base is !, which equates to a PHRED quality of `0`. I contacted someone at PacBio who informed me that they learned the base quality values were not any more useful than the bases themselves for generating a high-quality circular consensus sequence. Thus, rather than spend the computational effort to calculate the scores, they skipped it, and reported ! for every base. They also report an overall read quality of `0.8`, but this is simply a placeholder. They are working to make this information more readily available.

> Source: [StackExchange](https://bioinformatics.stackexchange.com/questions/885/which-quality-score-encoding-does-pacbio-use)

----

#### Explanations of headers of canu assembled fasta files

A typical header would look like

```
>tig######## len=<integer> reads=<integer> covStat=<float> gappedBases=<yes|no> class=<contig|bubble|unassm> suggestRepeat=<yes|no> suggestCircular=<yes|no>
```

Field | Description
--- | --- | ---
len | Length of the sequence, in bp
1 | 2 

 

 Number of reads used to form the contig.

`covStat` The log of the ratio of the contig being unique versus being two-copy, based on the read arrival rate.  Positive values indicate more likely to be unique, while negative values indicate more likely to be repetitive.  See `Footnote 24 <http://science.sciencemag.org/content/287/5461/2196.full#ref-24>`_ in `Myers et al., A Whole-Genome Assembly of Drosophila <http://science.sciencemag.org/content/287/5461/2196.full>`_.

`gappedBases`   If yes, the sequence includes all gaps in the multialignment.

`class`   Type of sequence.  Unassembled sequences are primarily low-coverage sequences spanned by a single read.

`suggestRepeat`   If yes, sequence was detected as a repeat based on graph topology or read overlaps to other sequences.

`suggestCircular`   If yes, sequence is likely circular.  Not implemented.

----


#### What is covStat in the header on canu assembled fasta file?

It's the log of the ratio of the contig being unique vs it being two-copy, based on the number of reads used to form it. http://wgs-assembler.sourceforge.net/wiki/index.php/Celera_Assembler_Theory#Coverage-based_repeat_discrimination

Positive means more likely to be unique, negative means more likely to be repetitive. It is flagging tig102 as having too many reads to be unique, based on the rest of the contigs. Given how large the contig is, this could be caused by a pile up of reads in a few repeats, or it could be indicating that this is a contaminant sequenced at higher coverage than the primary target

> Source: [Canu Github](https://github.com/marbl/canu/issues/101)

----

#### There are many fasta files in the canu output directory. What are those?

**contigs** will span repeats, as long as the repeat is unambiguous.

**unitigs** are derived from contigs. Wherever a contig end intersects the middle of another contig, the contig is split.

**bubbles** are deprecated and will be removed in the next release. Treat them as contigs for now.

**unassembled** contains mostly reads that failed to assemble into a contig. There will be some assembled sequences, but these will be short and nearly the same as the longest read in them.

Though out of date, and will probably move when it is updated, the relevant section in the docs is http://canu.readthedocs.io/en/latest/quick-start.html#find-the-output

> Source: [Canu github](https://github.com/marbl/canu/issues/286#issuecomment-258876724)
