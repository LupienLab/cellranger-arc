# cellranger scRNA
References:
https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/what-is-cell-ranger

https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/count


**STEP 1: Login to cluster:**
```
ssh username@cluster.org
```


**STEP2: Download genome references and example data**
```
wget https://cf.10xgenomics.com/supp/cell-exp/refdata-gex-GRCh38-2020-A.tar.gz
wget https://cf.10xgenomics.com/samples/cell-exp/6.1.0/500_PBMC_3p_LT_Chromium_X/500_PBMC_3p_LT_Chromium_X_fastqs.tar

tar -xvf refdata-gex-GRCh38-2020-A.tar.gz
tar -xvf 500_PBMC_3p_LT_Chromium_X_fastqs.tar


```

**STEP 3: Load cellranger-arc module with following command:**
```
module load cellranger/6.1.2
module load bcl2fastq2

```

**STEP4: Run mkfastq with following command if required, refer [here](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/mkfastq) to create csv file:**
```
cellranger mkfastq --id=tiny-bcl \
                     --run=/path/to/cellranger-tiny-bcl-1.2.0 \
                     --csv=/path/to/ellranger-tiny-bcl-simple-1.2.0.csv
e.g.
cellranger mkfastq --id=tiny-bcl --run=cellranger-tiny-bcl-1.2.0 --csv=cellranger-tiny-bcl-simple-1.2.0.csv --localcores=$SLURM_CPUS_PER_TASK   --localmem=6

sbatch -p himem -J scRNA --export=ALL -c 8 --mem 60G -t 1-0 --wrap "cellranger mkfastq --id=tiny-bcl --run=cellranger-tiny-bcl-1.2.0 --csv=cellranger-tiny-bcl-simple-1.2.0.csv --localcores=$SLURM_CPUS_PER_TASK   --localmem=60"

```

**STEP5: Run count with following command:**
```
cellranger count --id=sample345 \
                       --transcriptome=/opt/refdata-gex-GRCh38-2020-A \
                       --fastqs=/home/jdoe/500_PBMC_3p_LT_Chromium_X_fastqs \
                       --localcores=8 \
                       --expect-cells=500 \
                       --localmem=60
e.g.
cellranger count --id=sample345 --transcriptome=/cluster/projects/lupiengroup/People/ankita/demo/scRNA/refdata-gex-GRCh38-2020-A --fastqs=/cluster/projects/lupiengroup/People/ankita/demo/scRNA/500_PBMC_3p_LT_Chromium_X_fastqs --expect-cells=500 --localcores=8 --localmem=60

sbatch -p himem -J scRNA --export=ALL -c 8 --mem 60G -t 1-0 --wrap "cellranger count --id=sample345 --transcriptome=/cluster/projects/lupiengroup/People/ankita/demo/scRNA/refdata-gex-GRCh38-2020-A --fastqs=/cluster/projects/lupiengroup/People/ankita/demo/scRNA/500_PBMC_3p_LT_Chromium_X_fastqs --expect-cells=500 --localcores=8 --localmem=60"


```

