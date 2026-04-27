---
url: https://gerstung-lab.github.io/PCAWG-11/
source: gerstung-lab.github.io
date_saved: 2026-04-23
tags: 生物信息学, 医学药学
content_type: article
word_count: 8000
status: 已处理
---

# Supplementary code: The evolutionary history of 2,658 cancers

> source: [gerstung-lab.github.io](https://gerstung-lab.github.io/PCAWG-11/)

## 核心要点

1. Supplementary code: The evolutionary history of 2,658 cancers
2. Moritz Gerstung and Santiago Gonzalez, on behalf of the PCAWG-11 Evolution and Heterogeneity Working Group
3. ## Loading required package: knitr
4. Prior to this script, MutationTimeR was run on 2x 2,778 VCF files for subs and indels each.
5. This was done using the script VCF-annotate.
## 内容预览

Supplementary code: The evolutionary history of 2,658 cancers

Moritz Gerstung and Santiago Gonzalez, on behalf of the PCAWG-11 Evolution and Heterogeneity Working Group

2020-01-07

## Loading required package: knitr

1 Prelim

1.1 Preprocessing

Prior to this script, MutationTimeR was run on 2x 2,778 VCF files for subs and indels each. This was done using the script VCF-annotate.R, attached at the end of this vignette.

Input files:

VCF files: Folder ../final/final_consensus_12oct_passonly

Allele-specific copy number: Folder ../final/consensus.20170119.somatic.cna.annotated

Subclone sizes and cell frequencies: ../final/structure_weme_released_consensus_merged.txt, ../final/wcc_consensus_values_9_12.tsv

Purity: ../final/consensus.20170218.purity.ploidy.txt

Output:

cat(paste0(system("for d in `find ../final/annotated_014 -maxdepth 1 -type d`
do echo $d; ls $d | head -6;
done", ignore.stderr=TRUE, intern=TRUE), collapse="
"))
## ../final/annotated_014
## clusters
## cn
## graylist
## indel
## other
## snv_mnv
## ../final/annotated_014/snv_mnv
## 0009b464-b376-4fbc-8a56-da538269a02f.consensus.20160830.somatic.snv_mnv.complete_annotation.vcf.bgz
## 0009b464-b376-4fbc-8a56-da538269a02f.consensus.20160830.somatic.snv_mnv.complete_annotation.vcf.RData
## 003819bc-c415-4e76-887c-931d60ed39e7.consensus.20160830.somatic.snv_mnv.complete_annotation.vcf.bgz
## 003819bc-c415-4e76-887c-931d60ed39e7.consensus.20160830.somatic.snv_mnv.complete_annotation.vcf.RData
## 0040b1b6-b07a-4b...
## Loading required package: knitr

1 Prelim

1.1 Preprocessing

Prior to this script, MutationTimeR was run on 2x 2,778 VCF files for subs and indels each. This was done using the script VCF-annotate.R, attached at the end of this vignette.

Input files:

VCF files: Folder ../final/final_consensus_12oct_passonly

Allele-specific copy number: Folder ../final/consensus.20170119.somatic.cna.annotated

Subclone sizes...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
