# mVarScan

This project is for CSE 185. It implements a subset of `mpileup2snp` and finds SNPs within a given aligned genome in the form of a mpileup file. See [VarScan](https://varscan.sourceforge.net/using-varscan.html) for more details

[REQUIREMENTS](#requirements) | [INSTALLATION](#installation) | [BASIC USAGE](#usage) | [OPTIONAL](#optional) | [File formats](#format) | [Miro board](#miro)

<a name="requirements"></a>
## REQUIREMENTS:
- Ensure that you have Python 3.10 and above
- scipy.stats

These packages can be installed with `pip`:
```
pip install scipy.stats
```
Note: If you do not have root access, you can run the command above with additional options to install locally:
```
pip install --user scipy.stats
```

<a name="installation"></a>
## INSTALLATION:
```
git clone https://github.com/andrewbigelow/mVarScan.git
cd mVarScan
```

<a name="usage"></a>
## BASIC USAGE:
The basic usage of `mVarScan` is:
```
python path/to/file/CSE-185-mVarScan/main.py [options] [mpileup]
```

<a name="optional"></a>
## OPTIONAL:
    -o --out FILENAME (file to output contents to)
    -t --tab (1 for yes) (output using TAB formating, default: 0)
    -m --min-var-frequency FREQUENCY (minimum frequency to call a non-reference mutation, default: 0.2)
    -h --min-freq-for-hom FREQUENCY (minimum frequency to call a non-reference homozygous mutation, default: 0.8)
    -p --pvalue FLOAT (p-value threshold to output SNP, default: 0.99)
    -r2 --min-reads2 INT (minimum supporting reads at a position to call variants, default: 2)
    -c --min-coverage INT (Minimum read depth at a position to make a call. Default 8)
    -q --min-avg-qual INT (minimum average base quality at a position to count a read, default: 15)


<a name="format"></a>
## File Formats:
### Input Files
#### `mpileup`
A mpileup file is a tab-delimited text file with no header, traditionally generated by `samtools mpileup`.  It contains 6 columns:
```
chromosome    position    reference_base    coverage    read_bases    read_qualities    [optional extra columns]
```
 - col 1: chromosome, in the format `chr[name]` where `[name]` is the chromosome number
 - col 2: The 1-based coordinate of the position on the chromosome
 - col 3: The reference base at the position specified in column 2
 - col 4: The number of reads covering the position.
 - col 5: The bases from the reads that map to the position
 - col 6: A string of ASCII characters representing the quality scores of the reads aligned to the position

Example:
```
chr6	128405804	T	22	......................	DE:EFFImEJIJJIJ>JJIJHF
```

### Output Files
#### `tab`
A tab file is a tab-delimited text file that is a modified VCF format. It includes similar columns, but differs in what it displays. Below is the header used:
```
#CHROM    tPOS    REF    ALT    SAMPLE    [other samples]
```
 - col 1: chromosome, in the format `chr[name]` where `[name]` is the chromosome number
 - col 2: The 1-based coordinate of the position on the chromosome
 - col 3: The reference base at the position specified in column 2
 - col 4: The alternate base(s) observed which differ from the reference, suggesting a potential variant.
 - col 5: SAMPLE: Encoded information about the sample's variant status and additional metrics. This field is further subdivided into multiple subfields separated by colons (:):
     - Homozygous status: Indicates whether the variant is homozygous for the alternate allele (1/1) or heterozygous (0/1).
     - Variant count: The number of reads supporting the variant allele.
     - Total coverage: The total number of reads covering this position.
     - Average quality: The average base quality score of reads covering this position.
     - Variant frequency: The frequency of the variant allele relative to the total coverage.
     - P-value: A statistical measure that indicates the confidence in the variant call (the lower, the more confident).

Example:
```
chr6	128414945	c	T	1/1:44,44:38.63636363636363:1.0:7.619481455868034e-26
```

#### `regular output`
The information about the snps above are printed in clearly labeled sections in the terminal. As seen below:
```
Chromosome:position | Sample # | homozygous_status | ref_base -> variant_base | frequency | p-value | reads, coverage | average base quality |
```
Example:
```
chr6:128414945 | Sample 1 | 1/1 | c -> T | frequency 1.00 | p-value 7.619481455868034e-26 | reads 44,44 | avg base quality 38.63636363636363|
```

<a name="notes"></a>
### NOTES:
- The `--min-avg-qual` option sets the minimum average Phred quality score for bases to be considered in variant calling. Phred quality scores are a common metric in sequencing data quality control, indicating the probability of a base call being incorrect. For more information about Phred scores, refer to [this link](https://drive5.com/usearch/manual10/quality_score.html).

## Contributors:
This repository was generated by Andrew Bigelow, Aditya Parmar.
