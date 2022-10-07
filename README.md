## Single-cell-MSIsensor


## generate barcode files from metadata file (this step has already been done for test samples)

$ while read sample; do grep {sample}/tumor_barcodes.txt; grep {sample}/normal_barcodes.txt; cat cell_type_annotation/${sample}/tumor_barcodes.txt cell_type_annotation/${sample}/normal_barcodes.txt > cell_type_annotation/${sample}/tumor_normal_barcodes.txt; done < test_samples.txt

## split bams into tumor.bam and normal.bam then calculate msi score

## msi_calculation.sh

```
#!/bin/bash

source activate /diskmnt/Projects/Users/ysong/program/anaconda3/envs/R

sample=$1

## split bam

mkdir -p ./split_BAMs/${sample}

cd split_BAMs/${sample}

echo "Splitting bam ..."

sinto filterbarcodes -b /diskmnt/Projects/MetNet_analysis_2/Colorectal/cellranger/cellranger-v6/${sample}/outs/possorted_genome_bam.bam -c /diskmnt/Projects/Users/Evan.p/msi_test/cell_type_annotation/${sample}/normal_tumor_barcode.txt -p 50 >> ${sample}.sinto.err

echo "Splitting done."

## sort bams

echo "Sorting bams ..."

samtools sort Tumor.bam -o Tumor.sorted.bam
samtools sort Normal.bam -o Normal.sorted.bam

echo "Sorting done."

## index bams

echo "Indexing bams ..."
samtools index Tumor.sorted.bam
samtools index Normal.sorted.bam

echo "Indexing done."

cd ../../
mkdir -p MSI_outputs/${sample}

echo "Calculating msi score ..."

/diskmnt/Projects/Users/ysong/program/msisensor/msisensor msi -d /diskmnt/Projects/Users/ysong/project/MSISensor_testing/mCRC/microsatellites.list -n /diskmnt/Projects/Users/Evan.p/msi_test/split_BAMs/${sample}/Normal.sorted.bam -t /diskmnt/Projects/Users/Evan.p/msi_test/split_BAMs/${sample}/Tumor.sorted.bam -b 25 -l 1 -q 1 -o MSI_outputs/${sample}/${sample}




echo "All done."

less ./MSI_outputs/${sample}/${sample}

/diskmnt/Projects/Users/ysong/program/msisensor/msisensor msi -d /diskmnt/Projects/Users/Evan.p/msi_test/microsatellites38.list -n /diskmnt/Projects/MetNet_analysis_2/Colorectal/WES_alignment/01.ST_20220613/mark_dup_bam/HT307C1-Jm1D1_1.N.bam  -t /diskmnt/Projects/MetNet_analysis_2/Colorectal/WES_alignment/01.ST_20220613/mark_dup_bam/HT307C1-TH1K1A3D1.N.bam  -b 25 -l 1 -q 1 -o MSI_outputs/bulk/HT307C1_recheck
