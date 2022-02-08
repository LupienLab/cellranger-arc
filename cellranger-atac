# cellranger scATAC
References:
https://support.10xgenomics.com/single-cell-atac/software/pipelines/latest/what-is-cell-ranger-atac


**STEP 1: Login to cluster:**
```
ssh username@cluster.org
```


**STEP2: Download genome references and example data**
```
wget https://cf.10xgenomics.com/supp/cell-atac/refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz
wget https://cf.10xgenomics.com/supp/cell-atac/cellranger-atac-tiny-bcl-1.0.0.tar.gz

tar -xvf refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz
tar -xvf cellranger-atac-tiny-bcl-1.0.0.tar.gz


```

**STEP 3: Load cellranger-arc module with following command:**
```
module load cellranger-atac/2.0.0
module load bcl2fastq2

```

**STEP4: Run mkfastq with following command if required, refer [here](https://support.10xgenomics.com/single-cell-atac/software/pipelines/latest/using/mkfastq) to create csv file:**
```
cellranger-atac mkfastq --id=tiny-bcl \
                     --run=cellranger-atac-tiny-bcl-1.0.0 \
                     --csv=cellranger-atac-tiny-bcl-simple-1.0.0.csv
e.g.
cellranger mkfastq --id=tiny-bcl --run=cellranger-atac-tiny-bcl-1.0.0 --csv=cellranger-atac-tiny-bcl-simple-1.0.0.csv --localcores=$SLURM_CPUS_PER_TASK  --localmem=6

sbatch -p himem -J scRNA --export=ALL -c 8 --mem 60G -t 1-0 --wrap "cellranger mkfastq --id=tiny-bcl --run=cellranger-atac-tiny-bcl-1.0.0 --csv=cellranger-atac-tiny-bcl-simple-1.0.0.csv --localcores=$SLURM_CPUS_PER_TASK  --localmem=6"

```

**STEP5: Run count with following command:**
```
cellranger-atac count --id=sample345 \
                        --reference=/opt/refdata-cellranger-arc-GRCh38-2020-A-2.0.0 \
                        --fastqs=/home/jdoe/runs/HAWT7ADXX/outs/fastq_path \
                        --sample=mysample \
                        --localcores=8 \
                        --localmem=64
                        
e.g.

sbatch -p himem -J peaks --export=ALL -c 6 --mem 30G -t 1-0 --wrap "cellranger-atac count --id=sample345 --reference=refdata-cellranger-arc-GRCh38-2020-A-2.0.0 --fastqs=/cluster/projects/lupiengroup/People/ankita/demo/scATAC/cellranger-atac-tiny-bcl-simple-1.0.0.csv --localcores=$SLURM_CPUS_PER_TASK   --localmem=30"

```

