# oneliner_100day_challenge


* Day 1 Get the sequences length distribution from a fastq file using awk:

```
zcat example.fastq.gz

@SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=72
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACCAAGTTACCCTTAACAACTTAAGGGTTTTCAAATAGA
+SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=72
IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9ICIIIIIIIIIIIIIIIIIIIIDIIIIIII>IIIIII/
@SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=72
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGAAGCAGAAGTCGATGATAATACGCGTCGTTTTATCAT
+SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=72
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBIIIIIIIIIIIIIIIIIIIIIIIGII>IIIII-I)8I
@SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=36
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
+SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=36
IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9IC
@SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=36
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA
+SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=36
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBI
@071112_SLXA-EAS1_s_7:5:1:817:345
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
+071112_SLXA-EAS1_s_7:5:1:817:345
IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9IC
@071112_SLXA-EAS1_s_7:5:1:801:338
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA
+071112_SLXA-EAS1_s_7:5:1:801:338
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBI

zless example.fastq.gz | awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}'  
```

initiate a awk arrary named `lengths`, save all the record length to it and increment the frequency by `array[]++`
A fastq record contains 4 lines.
when line number can be divided by 4 and leave 2: `NR%4 == 2` to get the sequence line.
`length($0)` gives the length of the full line denoted by `$0`.

when the `awk` finishes reading all lines: `END`
print out the lengths and its frequency



* Day 2 Reverse complement a sequence:

```
echo 'ATTGCTATGCTNNNT' | rev | tr 'ACTG' 'TGAC'

ANNNAGCATAGCAAT
```

`rev`: reverse the sequence 
`tr`: translate the `ACTG` to its complement `TGAC`


* Day 3 split a multi-fasta to multiple single fasta file

```
cat multi_fasta.fa
>seq1
ATCTGCGATCCCC
>seq2
GGGTCGTCTTCAAAAAT
>seq3
TCGATTTTGATTTTAAAAA

cat multi_fasta.fa | awk '{
        if (substr($0, 1, 1)==">") {filename=(substr($0,2) ".fasta")}
        print $0 >> filename
        close(filename)
}'

```

or use csplit

```
csplit -z multi_fasta.fa

```

* Day 4 turn a fastq to a fasta file:

```
cat example.fastq| paste - - - - | perl -F"\t" -ane 'print ">$F[0]_$F[3]$F[1]\n";'
>@SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=72_IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9ICIIIIIIIIIIIIIIIIIIIIDIIIIIII>IIIIII/
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACCAAGTTACCCTTAACAACTTAAGGGTTTTCAAATAGA
>@SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=72_IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBIIIIIIIIIIIIIIIIIIIIIIIGII>IIIII-I)8I
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGAAGCAGAAGTCGATGATAATACGCGTCGTTTTATCAT
>@SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=36_IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9IC
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
>@SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=36_IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBI
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA
>@071112_SLXA-EAS1_s_7:5:1:817:345_IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9IC
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
>@071112_SLXA-EAS1_s_7:5:1:801:338_IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBI
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA


less example.fastq | paste - - - -  |  awk 'BEGIN { FS = "\t" };{printf(">%s_%s\n%s\n",$1,$4,$2);}'
>@SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=72_IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9ICIIIIIIIIIIIIIIIIIIIIDIIIIIII>IIIIII/
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACCAAGTTACCCTTAACAACTTAAGGGTTTTCAAATAGA
>@SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=72_IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBIIIIIIIIIIIIIIIIIIIIIIIGII>IIIII-I)8I
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGAAGCAGAAGTCGATGATAATACGCGTCGTTTTATCAT
>@SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=36_IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9IC
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
>@SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=36_IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBI
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA
>@071112_SLXA-EAS1_s_7:5:1:817:345_IIIIIIIIIIIIIIIIIIIIIIIIIIIIII9IG9IC
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
>@071112_SLXA-EAS1_s_7:5:1:801:338_IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII6IBI
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA


cat example.fastq | sed -n '1~4s/^@/>/p;2~4p'
>SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=72
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACCAAGTTACCCTTAACAACTTAAGGGTTTTCAAATAGA
>SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=72
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGAAGCAGAAGTCGATGATAATACGCGTCGTTTTATCAT
>SRR001666.1 071112_SLXA-EAS1_s_7:5:1:817:345 length=36
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
>SRR001666.2 071112_SLXA-EAS1_s_7:5:1:801:338 length=36
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA
>071112_SLXA-EAS1_s_7:5:1:817:345
GGGTGATGGCCGCTGCCGATGGCGTCAAATCCCACC
>071112_SLXA-EAS1_s_7:5:1:801:338
GTTCAGGGATACGACGTTTGTATTTTAAGAATCTGA
```

split fastq to a set of smaller fastqs with a fixed number of reads (SEQNUM)

```

SEQNUM=10; split -l $((4*$SEQNUM)) --filter='gzip > $FILE.gz' --additional-suffix=.fq -d <(zcat aaa.fq.gz) 
```

* Day 5 Reproducible subsampling of a FASTQ file. 

`0.01` is the % of reads to output.

```
cat file.fq | paste - - - - | awk 'BEGIN{srand(1234)}{if(rand() < 0.01) print $0}' | tr '\t' '\n' > out.fq

cat file.fq | paste - - - - | shuf -n  1000 | tr '\t' '\n' > out.fq

seqkit sample -p 0.01 file.fq

```
* Day 6 Get the header and the column number:

```
cat data.tsv
ID	head1	head2	head3	head4
1	25.5	1364.0	22.5	13.2
2	10.1	215.56	1.15	22.2

cat data.tsv | head -1 | tr "\t" "\n" | cat -n
     1	ID
     2	head1
     3	head2
     4	head3
     5	head4
```

`head -1` : print the first line
`tr` translate tab to newline
`cat -n` print the line number. You can use `nl` too

```

 cat data.tsv | nl
     1	ID	head1	head2	head3	head4
     2	1	25.5	1364.0	22.5	13.2
     3	2	10.1	215.56	1.15	22.2


cat data.tsv | head -1 | tr "\t" "\n" | nl
     1	ID
     2	head1
     3	head2
     4	head3
     5	head4

# use sed, you can change to any line number

cat data.tsv | sed -n '1s/\t/\n/gp'
ID
head1
head2
head3
head4

```

use `csvkit`:

```
csvcut -nt -l data.tsv
  1: ID
  2: head1
  3: head2
  4: head3
  5: head4
```
* Day 7 Show hidden characters

```
cat -A data.tsv
ID^Ihead1^Ihead2^Ihead3^Ihead4$
1^I25.5^I1364.0^I22.5^I13.2$
2^I10.1^I215.56^I1.15^I22.2$
```

`^I` means tab, `$` means the end of the line.
This is useful when you get a file and see if you do see a tab between columns or you may have 2 tabs between the columns
Note on mac you will need to use gnu utilities and use `gcat -A`

```
sed -n l data.tsv
ID\thead1\thead2\thead3\thead4$
1\t25.5\t1364.0\t22.5\t13.2$
2\t10.1\t215.56\t1.15\t22.2$
```
now, tab is denoted as `\t` and `$` means the end of the line.


* Day 8 print out unique rows based on the first and second column

```
awk '!a[$1,$2]++' input_file
```

sort based on unique (first and second)column

```
sort -u -k1,2 input_file
```

In `R`

```
df %>% dplyr::distinct(column1, column2, .keep_all =TRUE)
```


* Day 9 split a bed file by chromosome

```

cat nexterarapidcapture_exome_targetedregions_v1.2.bed | sort -k1,1 -k2,2n | sed 's/^chr//' | awk '{close(f);f=$1}{print > f".bed"}'

#or
awk '{print $0 >> $1".bed"}' example.bed
```

`sed` to remove the `chr` and `awk` split the files to `1.bed`, `2.bed` etc.


split large file by id/label/column. you can change $1 to $2 etc depends on which column you want to use

```
awk '{print >> $1; close($1)}' input_file

```


```
cat example.bed
chr1    12  14  sample1
chr1    10  15  sample2
chr2    10  20  sample1
chr2    22  33  sample2

awk '{print >> $1".bed"; close($1".bed")}' example.bed
```
it gives you `chr1.bed`, `chr2.bed`

```
awk '{print >> $4".bed"; close($4".bed")}' example.bed

```
it gives you `sample1.bed`, `sample2.bed`

* Day 10 sort VCF with header

```
 cat my.vcf | awk '$0~"^#" { print $0; next } { print $0 | "sort -k1,1V -k2,2n" }'

```


* Day 11 rename file with bash string manipulation 

```
for file in *gz
    do zcat $file > ${file/bed.gz/bed}
done 
```

* Day 12 exit a dead `ssh` session

press:
```
~.
```

* Day 13 copy large files with rsync

copy the from_dir directory to the to_dir directory

```
rsync -av from_dir  to_dir
```

copy every file inside the frm_dir to to_dir. Note the trailing slash

```
rsync -av from_dir/ to_dir
```

re-copy the files avoiding completed ones

```
rsync -avhP from_dir to_dir
```

* Day 14  make directory using the current date

```
mkdir $(date +%F)
```

see A Quick Guide to Organizing Computational Biology Projects

* Day 15  get all the folders' size in the current folder

```
du -h --max-depth=1
``` 

the total size of current directory

```
du -sh .
```

disk usage

```
df -h
```

check GNU [`ncdu`](https://dev.yorhel.nl/ncdu). 

open `top -M` with human readable size in Mb, Gb. install [htop](https://htop.dev/) for better visualization.


* Day 16 pretty output

```
less -S
fold -w 60
cat file.tsv| column -t | less -S
csvtk pretty names.csv #from csvtk
csvlook names.csv #from csvkit
```

* Day 17 keep header filter

always print the first line, and filter by column 10, 11 and 18
```
awk ' NR ==1 || ($10 > 1 && $11 > 0 && $18 > 0.001)' input_file
```

In R, header is always maintained in the dataframe.
```
df %>% filter(column10 >1, column11 >0, column18 > 0.001)
```

* Day 18 delete lines by sed

delete the blank lines 
```
sed /^$/d'

```
delete the last line

```
sed $d
```

sed '1d' to remove the header. 

```
ls *csv | parallel 'cut -f, -d 2 | sed '1d' > {/.}.list'
```


print the second line of a LARGE file and quit: 

```
sed -n '2{p;q}'
```

* Day 19 create a tx2gene mapping file from ensemble gtf retaining the version number of genes and transcripts.

```
awk -F "\t" '$3 == "transcript" { print $9 }' myensembl.gtf| tr -s ";" " "   | cut -d " " -f2,4|  sed 's/\"//g' | awk '{print $1"."$2}' > genes.txt

awk -F "\t" '$3 == "transcript" { print $9 }' myensembl.gtf| tr -s ";" " "   | cut -d " " -f6,8|  sed 's/\"//g' | awk '{print $1"."$2}' > transcripts.txt

paste transcripts.txt genes.txt > tx2genes.txt

```

* Day 20 print every 2nd line of every 4 lines in a fastq file, get the barcode frequency table

```
zcat  mysample_L001_I1_001.fastq.gz | sed -n '2~4p' | sort | uniq -c | sort -k1,1 -nr > barcode_freq.txt
```


* Day 21

ever using sed to replace paths in your string, use , as a separator! 

```
sed 's,/my/path/,,'
```

* Day 22

sanity check if the cell barcode is v2 or v3


```
comm -12 <(zcat 3M-february-2018.txt.gz |sort) <(zcat Sample1/outs/filtered_feature_bc_matrix/barcodes.tsv.gz | sed 's/-1//' | sort) | wc -l
```

`M-february-2018.txt.gz` is the 10x genomics whitelist file.

* Day 23 select lines from a file based on columns in another file

```
awk -F"\t" 'NR==FNR{a[$1$2$3]++;next};a[$1$2$3] > 0' file2 file1 
```
NR==FNR : NR is the current input line number and FNR the current file's line number. The two will be equal only while the 1st file is being read.

a[$1$2$3]++; next : if this is the 1st file, save the 1st three fields in the `a` array. Then, skip to the next line so that this is only applied on the 1st file.

a[$1$2]>0 : the else block will only be executed if this is the second file so we check whether fields 1, 2 and 3of this file have already been seen (a[$1$2$3]>0) and if they have been, we print the line. In awk, the default action is to print the line so if a[$1$2$3]>0 is true, the line will be printed.

see https://github.com/crazyhottommy/scripts-general-use/blob/master/Shell/Awk_anotates_vcf_with_bed.ipynb


* Day 24  find bam in current folder (search recursively) and copy it to a new directory using 5 CPUs

```
find . -name "*bam" | xargs -P5 -I{} rsync -av {} dest_dir
```

* Day 25

`ls -X ` will group files by extension.

* Day 26 loop through all chromosomes

```
for i in {1..22} X Y 
do
  echo $i
done
```

```
for i in {01..22}
do 
    echo $i
done
```

01
02
03
04
05
06
07
08
09
10
11
12
13
14
15
16
17
18
19
20
21
22

* Day 27

`paste` is used to concatenate corresponding lines from files: paste file1 file2 file3 .... If one of the "file" arguments is "-", then lines are read from standard input. If there are 2 "-" arguments, then paste takes 2 lines from stdin. And so on.

```
cat test.txt  
0    ATTTTATTNGAAATAGTAGTGGG
0    CTCCCAAAATACTAAAATTATAA
1    TTTTAGTTATTTANGAGGTTGAG
1    CNTAATCTTAACTCACTACAACC
2    TTATAATTTTAGTATTTTGGGAG
2    CATATTAACCAAACTAATCTTAA
3    GGTTAATATGGTGAAATTTAAT
3    ACCTCAACCTCNTAAATAACTAA

cat test.txt| paste - -                               
0    ATTTTATTNGAAATAGTAGTGGG    0    CTCCCAAAATACTAAAATTATAA
1    TTTTAGTTATTTANGAGGTTGAG    1    CNTAATCTTAACTCACTACAACC
2    TTATAATTTTAGTATTTTGGGAG    2    CATATTAACCAAACTAATCTTAA
3    GGTTAATATGGTGAAATTTAAT     3    ACCTCAACCTCNTAAATAACTAA
```

or use awk

```
cat test.txt| awk 'ORS=NR%2?"\t":"\n"'          

0    ATTTTATTNGAAATAGTAGTGGG    0    CTCCCAAAATACTAAAATTATAA
1    TTTTAGTTATTTANGAGGTTGAG    1    CNTAATCTTAACTCACTACAACC
2    TTATAATTTTAGTATTTTGGGAG    2    CATATTAACCAAACTAATCTTAA
3    GGTTAATATGGTGAAATTTAAT     3    ACCTCAACCTCNTAAATAACTAA
```

ORS: output record seperator in awk. `var=condition?condition_if_true:condition_if_false` is the ternary operator.




