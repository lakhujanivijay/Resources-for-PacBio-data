## Pacbio-Brick by Brick!
A handy list of tools, blogs, forums, resources and much more dedicated to Pacbio data analysis.

### Which quality score encoding does PacBio use?

PacBio does use PHRED 33, but it turns out the question may be irrelevant for the newer PacBio Sequel Sequencer, because it reports all base qualities as PHRED 0 (ASCII !). The RS-II reports PHRED 33 quality scores.

The Sequel provides data in the form of subreads, which are the circular consensus sequences (CCS) from a single zero-mode waveguide (ZMW). Essentially, a single circularized DNA molecule enters a ZMW well, and is sequenced as many times as possible. (Sometimes multiple molecules get into a single well, but that's not the normal case.) The number of times this single molecule can be sequenced depends on several variables, including molecule length. The consensus sequence for this single molecule is then reported out as a subread.

As I started playing with the quality scores in my data, I realized the quality score for every base is !, which equates to a PHRED quality of 0. I contacted someone at PacBio who informed me that they learned the base quality values were not any more useful than the bases themselves for generating a high-quality circular consensus sequence. Thus, rather than spend the computational effort to calculate the scores, they skipped it, and reported ! for every base. They also report an overall read quality of 0.8, but this is simply a placeholder. They are working to make this information more readily available.

> Source: [StackExchange](https://bioinformatics.stackexchange.com/questions/885/which-quality-score-encoding-does-pacbio-use)
