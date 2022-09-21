## Single-cell-MSIsensor


## generate barcode files from metadata file (this step has already been done for test samples)

$ while read sample; do grep {sample}/tumor_barcodes.txt; grep {sample}/normal_barcodes.txt; cat cell_type_annotation/${sample}/tumor_barcodes.txt cell_type_annotation/${sample}/normal_barcodes.txt > cell_type_annotation/${sample}/tumor_normal_barcodes.txt; done < test_samples.txt

## split bams into tumor.bam and normal.bam then calculate msi score

msi_calculation.sh

```
!/bin/bash

source activate /diskmnt/Projects/Users/ysong/program/anaconda3/envs/R

sample=$1

## split bam

mkdir -p ./split_BAMs/${sample}

cd split_BAMs/${sample}

echo "Splitting bam ..."

sinto filterbarcodes -b /diskmnt/Projects/MetNet_analysis_2/Colorectal/cellranger/cellranger-v6/${sample}/outs/possorted_genome_bam.bam -c /diskmnt/Projects/Users/ysong/project/MSISensor_testing/mCRC/cell_type_annotation/${sample}_c5/tumor_normal_barcodes.txt -p 50 >> ${sample}.sinto.err

