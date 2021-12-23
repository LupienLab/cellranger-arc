# cellranger-arc
Reference: https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/what-is-cell-ranger-arc

**STEP 1: Login to cluster:**
```
ssh username@cluster.org
```


**STEP2: Download genome references and example data**
```
wget https://cf.10xgenomics.com/supp/cell-arc/refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz
wget https://cf.10xgenomics.com/supp/cell-arc/refdata-cellranger-arc-mm10-2020-A-2.0.0.tar.gz

tar -xvf refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz
tar -xvf refdata-cellranger-arc-mm10-2020-A-2.0.0.tar.gz

wget https://cf.10xgenomics.com/supp/cell-arc/cellranger-arc-tiny-bcl-atac-1.0.0.tar.gz
wget https://cf.10xgenomics.com/supp/cell-arc/cellranger-arc-tiny-bcl-atac-simple-1.0.0.csv

tar -xvf cellranger-arc-tiny-bcl-atac-1.0.0.tar.gz

wget https://cf.10xgenomics.com/supp/cell-arc/cellranger-arc-tiny-bcl-gex-1.0.0.tar.gz
wget https://cf.10xgenomics.com/supp/cell-arc/cellranger-arc-tiny-bcl-gex-simple-1.0.0.csv


tar -xvf cellranger-arc-tiny-bcl-gex-1.0.0.tar.gz

rm refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz cellranger-arc-tiny-bcl-atac-1.0.0.tar.gz cellranger-arc-tiny-bcl-gex-1.0.0.tar.gz

```

**STEP 3: Load cellranger-arc module with following command:**
```
module load cellranger-arc/2.0.0
module load bcl2fastq2

```

**STEP4: Run mkfastq with following command if required, refer [here](https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/using/mkfastq) to create csv file:**
```
cellranger-arc mkfastq --id=tiny-bcl-atac \
                     --run=/path/to/cellranger-arc-tiny-bcl-atac-1.0.0 \
                     --csv=/path/to/cellranger-arc-tiny-bcl-atac-simple-1.0.0.csv
e.g.
cellranger-arc mkfastq --id=tiny-bcl-atac --run=cellranger-arc-tiny-bcl-atac-1.0.0/ --csv=cellranger-arc-tiny-bcl-atac-simple-1.0.0.csv --output-dir=results/
```

**STEP5: Create a libraries CSV file as shown [here](https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/using/count):**

Construct a 3-column libraries CSV file that specifies the location of the ATAC and GEX FASTQ files associated with the sample.
For our example, the file would look as follows:
```
fastqs,sample,library_type
/home/jdoe/runs/HNGEXSQXXX/outs/fastq_path,example,Gene Expression
/home/jdoe/runs/HNATACSQXX/outs/fastq_path,example,Chromatin Accessibility
```

**STEP6: Run count with following command:**
```
cellranger-arc count --id=sample345 \
                       --reference=/opt/refdata-cellranger-arc-GRCh38-2020-A-2.0.0 \
                       --libraries=/home/jdoe/runs/libraries.csv \
                       --localcores=16 \
                       --localmem=64
```
