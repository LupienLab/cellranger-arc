# cellranger-arc
Reference:

https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/what-is-cell-ranger-arc

https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/using/count

https://www.10xgenomics.com/resources/datasets/pbmc-from-a-healthy-donor-granulocytes-removed-through-cell-sorting-3-k-1-standard-2-0-0

https://www.10xgenomics.com/resources/datasets?menu%5Bproducts.name%5D=Single%20Cell%20Multiome%20ATAC%20%2B%20Gene%20Expression&query=&page=1&configure%5Bfacets%5D%5B0%5D=chemistryVersionAndThroughput&configure%5Bfacets%5D%5B1%5D=pipeline.version&configure%5BhitsPerPage%5D=500

https://hpc.nih.gov/apps/cellranger-arc.html

**STEP 1: Login to cluster:**
```
ssh username@cluster.org
```


**STEP2: Download genome references and example data**
```
## For human:

wget https://cf.10xgenomics.com/supp/cell-arc/refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz

tar -xvf refdata-cellranger-arc-GRCh38-2020-A-2.0.0.tar.gz


## For mouse

wget https://cf.10xgenomics.com/supp/cell-arc/refdata-cellranger-arc-mm10-2020-A-2.0.0.tar.gz

tar -xvf refdata-cellranger-arc-mm10-2020-A-2.0.0.tar.gz


## Example data and csv file

wget https://s3-us-west-2.amazonaws.com/10x.files/samples/cell-arc/2.0.0/pbmc_granulocyte_sorted_3k/pbmc_granulocyte_sorted_3k_fastqs.tar

tar -xvf pbmc_granulocyte_sorted_3k_fastqs.tar

wget https://cf.10xgenomics.com/samples/cell-arc/2.0.0/pbmc_granulocyte_sorted_3k/pbmc_granulocyte_sorted_3k_library.csv


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
cellranger-arc mkfastq --id=tiny-bcl-atac --run=cellranger-arc-tiny-bcl-atac-1.0.0/ --csv=cellranger-arc-tiny-bcl-atac-simple-1.0.0.csv --localcores=$SLURM_CPUS_PER_TASK   --localmem=30
cellranger-arc mkfastq --id=tiny-bcl-gex --run=cellranger-arc-tiny-bcl-gex-1.0.0/ --csv=cellranger-arc-tiny-bcl-gex-simple-1.0.0.csv --localcores=$SLURM_CPUS_PER_TASK   --localmem=30
```

**STEP5: Create a libraries CSV file as shown [here](https://support.10xgenomics.com/single-cell-multiome-atac-gex/software/pipelines/latest/using/count):**

Construct a 3-column libraries CSV file that specifies the location of the ATAC and GEX FASTQ files associated with the sample.
For our example, the file would look as follows (NOTE: Downloaded for the example data as above):
```
fastqs,sample,library_type
/home/jdoe/runs/HNGEXSQXXX/outs/fastq_path,example,Gene Expression
/home/jdoe/runs/HNATACSQXX/outs/fastq_path,example,Chromatin Accessibility

e.g.
fastqs,sample,library_type
tiny-bcl-gex/outs/fastq_path/,test_sample_gex,Gene Expression
tiny-bcl-atac/outs/fastq_path/,test_sample_atac,Chromatin Accessibility
```

**STEP6: Run count with following command:**
```
cellranger-arc count --id=sample345 \
                       --reference=/opt/refdata-cellranger-arc-GRCh38-2020-A-2.0.0 \
                       --libraries=/home/jdoe/runs/libraries.csv \
                       --localcores=16 \
                       --localmem=64
e.g.
cellranger-arc count --id=sample345 --reference=refdata-cellranger-arc-GRCh38-2020-A-2.0.0 --libraries=libraries.csv --localcores=16   --localmem=64


sbatch -p himem -J peaks --export=ALL -c 6 --mem 30G -t 1-0 --wrap "cellranger-arc count --id=sample345 --reference=refdata-cellranger-arc-GRCh38-2020-A-2.0.0 --libraries=libraries.csv --localcores=$SLURM_CPUS_PER_TASK   --localmem=30"
```
