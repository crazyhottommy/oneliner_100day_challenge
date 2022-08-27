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
`cat -n` print the line number

* Day 4

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


