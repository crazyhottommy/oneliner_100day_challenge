# oneliner_100day_challenge


* Day 1

Get the sequences length distribution from a fastq file using awk:

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



* Day 2

Reverse complement a sequence:

```
echo 'ATTGCTATGCTNNNT' | rev | tr 'ACTG' 'TGAC'

ANNNAGCATAGCAAT
```

`rev`: reverse the sequence 
`tr`: translate the `ACTG` to its complement `TGAC`


* Day 3

split a multi-fasta to multiple single fasta file

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

* Day 4

turn a fastq to a fasta file:

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

* Day 5

Reproducible subsampling of a FASTQ file. 0.01 is the % of reads to output.

```
cat file.fq | paste - - - - | awk 'BEGIN{srand(1234)}{if(rand() < 0.01) print $0}' | tr '\t' '\n' > out.fq

```
* Day 6

Get the header and the column number:

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
* Day 7

Show hidden character with `cat`:

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


* Day 8

print out unique rows based on the first and second column

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




