## Single-cell-MSIsensor
sample=$1

## split bam

mkdir -p ./split_BAMs/${sample}

cd split_BAMs/${sample}

echo "Splitting bam ..."

sinto filterbarcodes -b /diskmnt/Projects/MetNet_analysis_2/Colorectal/cellranger/cellranger-v6/${sample}/outs/possorted_genome_bam.bam -c /diskmnt/Projects/Users/ysong/project/MSISensor_testing/mCRC/cell_type_annotation/${sample}_c5/tumor_normal_barcodes.txt -p 50 >> ${sample}.sinto.err

