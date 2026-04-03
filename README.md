# IBDKinship v1.0

Contact Author: Hua Chen (chen_hua@fudan.edu.cn)

Copyright (c) 2026 Institute of Forensic Science, Fudan University

The tool is free for non-commercial usage. Please use at your own discretion. For commercial use please contact us.

## Run IBDKinship Program

IBDKinship program is compiled using Java 17+ (compatible with any platform that has a Java Runtime Environment). IBDKinship program has below parameters (all commands start with `java -jar IBDKinship.jar`):
- --infer  
Run Kinship Inference mode (computes lg-likelihood ratios and final predictions).
- --testing  
Run Kinship LR Testing mode (computes Likelihood Ratios for specified pedigrees).
- --files-root `<INPUT PATH>`  
Root directory for input IBD files (default: data).
- --chr-path `<PREFIX>`  
Chromosome file prefix (default: chr).
- --sample-num `<INT>`  
Number of samples to process (default: 10).
- --output-root `<OUTPUT PATH>`  
Output directory (default: result/inference or result/testing depending on mode).

Inference Mode (`--infer`) specific parameters:  
- --sus-peds `<LIST>`  
Comma-separated list of suspect pedigrees to test (default: pach,fsibling,hsibling,avuncular,grand,1cousin,1cor,2cousin,2cor,3cousin).
- --true-peds `<LIST>`  
Comma-separated list of true pedigrees in the data (default: same as sus-peds + unrelated).
- --max-workers `<INT>`  
Maximum number of parallel threads (default: CPU cores - 1).
- --no-parallel  
Disable parallel execution.
- --append  
Append results to existing output files instead of overwriting.

Testing Mode (`--testing`) specific parameters:
- --pedigree `<LIST>`  
Comma-separated list of pedigrees to test (e.g. grand,fsibling,1cousin).

Two example command of running IBDKinship:
```
java -jar IBDKinship.jar --infer
java -jar IBDKinship.jar --testing
```

The command to get the help of the program:
```
java -jar IBDKinship.jar -h
java -jar IBDKinship.jar --infer -h
java -jar IBDKinship.jar --testing -h
```

## Input Files
To run the IBDKinship program, you need to provide IBD segment files only. One IBD segment file is required for each chromosome (`1`–`22`) for the target pedigree. More than one individual can be analyzed in a single run by setting the `--sample-num` parameter. If any chromosome file is missing, the program will report an error.  
- File naming: The IBD segment files must follow this naming pattern: `{files-root}/{chr-path}{1-22}-{pedigree}.txt`.  
```
data/chr1-fsibling.txt  
data/chr2-fsibling.txt 
...  
data/chr22-fsibling.txt
```

- Supported pedigree names: The `{pedigree}` part of the filename must match a pedigree name supported by the program.

| Pedigree code | Relationship |
|:-------------:|:------------:|
| `pach` | Parent-child |
| `fsibling` | Full siblings |
| `hsibling` | Half siblings |
| `avuncular` | Avuncular |
| `grand` | Grandparent-grandchild |
| `1cousin` | First cousins |
| `1cor` | First cousins once removed |
| `2cousin` | Second cousins |
| `2cor` | Second cousins once removed |
| `3cousin` | Third cousins |
| `unrelated` | Unrelated |

- File format: Each file must contain two columns per line, separated by spaces or tabs.
```
First column: Sample ID (1-based).
Second column: IBD segment length in centiMorgans (cM).  
1   12.345
1   8.672
2   15.123
...
```

## Output files

The program writes output files in text format. The output order follows the sample IDs from `1` to `--sample-num`.

- Testing mode  
`{pedigree}.txt` — one `log10(LR)` value per sample, with one value on each line.

- Inference mode  
`{truePed}_{susPed}.txt` — one `log10(LR)` value per sample for the hypothesized relationship.  
`{truePed}_final_inference.txt` — final inference results with three columns: SampleID, PredictedPedigree and lg^LR.
