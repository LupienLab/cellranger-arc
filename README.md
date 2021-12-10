# cellranger-arc
##https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/what-is-cell-ranger-arc

STEP 1: Login to h4h


STEP 2: Load cellranger-arc module

module load cellranger-arc/2.0.0


STEP3: Download genome references

wget https://cf.10xgenomics.com/supp/cell-arc/refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz
wget https://cf.10xgenomics.com/supp/cell-arc/refdata-cellranger-arc-mm10-2020-A-2.0.0.tar.gz


STEP4: Run mkfastq:

cellranger-arc mkfastq --id=tiny-bcl-atac \
                     --run=/path/to/cellranger-arc-tiny-bcl-atac-1.0.0 \
                     --csv=/path/to/cellranger-arc-tiny-bcl-atac-simple-1.0.0.csv


STEP4: Run count:

cellranger-arc count --id=sample345 \
                       --reference=/opt/refdata-cellranger-arc-GRCh38-2020-A-2.0.0 \
                       --libraries=/home/jdoe/runs/libraries.csv \
                       --localcores=16 \
                       --localmem=64
