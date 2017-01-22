Mercat: Python code for Parallel k-mer counting
================================================
  
Usage: python mercat.py -i path-to-input-file -k kmer-length [-n no-of-cores] [-c kmer-min-count]
> Example: To compute all 3-mers:
            python mercat.py -i test.fa -k 3 -n 8 -c 10
            
- Supported Formats: `faa`, `fasta` and `fastQ`
- If core count is not specified, mercat will use all available physical cores on the machine.
- Results are stored in input-file-name.csv and input-file-name_summary.csv  
   (test.csv and test_summary.csv in the above example)  
- test.csv contains kmer count for kmers in individual sequences  
- test_summary.csv contains kmer count for all unique kmers across all sequences in the sample test.fa

New Items:
1. take a multiple file input then take output csv do MDS in matlibplot. 
 - MDS with each sample equaling one point
 - MDS each kmer from each sample as many points with different colors per sample 
2. calculate these metrics: from a .faa file
- Add pI calculation
http://isoelectric.ovh.org/

-Hydro, MW and pH
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3283891/


TRIMMOMATIC
Allows user to clean up fastq file prior to using mercat
java -jar trimmomatic-0.33.jar SE -phred33 In.fastq Out.fastq ILLUMINACLIP:TruSeq2-SE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:30 MINLEN:50
User defines: 
SLIDINGWINDOW:4:30
Scan the read with a 4-base wide sliding window, cutting when the average quality per base drops below 30 (SLIDINGWINDOW:4:30)
User can define the :4:X not the window size aka 4. 

trimmomatic SE -phred33 test.fq Out.fastq ILLUMINACLIP:TruSeq2-SE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:30 MINLEN:50

PRODIGAL
To format nucleotide.fa/.fasta to .faa
add prodigal program for converting .fa, .fasta, fastq nucleotides to .faa amino acid file
shell command 
prodigal -i input -o {output.gff} -a {output.orf_pro.faa} -f gff -p meta -d {output.orf_nuc} 2> {log}

input = nucleotide.fa or nucleotide.fasta or nucleotide.fna

outputs 
-a = output_orf_proteins.faa file amino acid fasta, 
-f a gff location file for protein coding orfs
-p meta default assumes metagenome or metatranscriptome (microbial community)
-d nucleotide orf called .ffn tells you the nucleotide sequence used for each protein coding orf. 

We will use the .faa for amino acid kmer counting if the person requests it!

Need to calculate metrics from .faa or protein coding file
2. calculate these metrics: from a .faa file
- Add pI calculation
http://isoelectric.ovh.org/

-Hydro, MW and pI
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3283891/


CHUNKER
python 3/anaconda3.5
Chunker.py file.faa Output_folder/ -c 100M -d ">"
Chunker.py file.fasta Output_folder/ -c 100M -d ">"

This would split the file by ">" every 100 mb of file evenly. It could also do it by lines and size. the -l option. 

Dependencies
------------
Mercat needs the following python libraries:   
> pip install pandas joblib --user
> pip3 install humanize --user
Add humanize 


Current Limitations
--------------------
- Mercat only works with 1 input file at a time. Could we process more files at once in the future?
- multiple file summary


Plots
----

Nucleotides for a single sample:
1.From Summary file: Stacked Bar chart for Top 10 kmers -- % of them Freq on Y axis, kmer (count?)on X
2.From Summary file: Cluster graph - kmer on Y axis, GC% and AT% on Xaxis - top 10 kmers - count G,C,A,T

Protein for a single sample:
From summary file:
1. same as 1. 
2. scatter plots for PI, MW, Hydro (Top 10 kmers)


DISCUSS
--------
For NUC:
 - we have AT,GC calc in both k-mer summary and gene seq reports

For protein: 
 - do we need AT,GC % calculation - currently we have it only in seq report csv - not in kmer summary
 - we have Pi,MW,Hydro for k-mer summary, but not in seq csv

Different options and their combinations and error checks

Sorted summary file in dec order of kmer occurence counts 
Revisit scatter plots - x vs y axis

