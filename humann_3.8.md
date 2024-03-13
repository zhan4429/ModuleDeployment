The database server for humann sometimes do not work. So we have to download the database manually

## utility_mapping
```
cd /cluster/tufts/biocontainers/datasets/humann
mkdir utility_mapping
cd utility_mapping
wget --no-check-certificate http://huttenhower.sph.harvard.edu/humann_data/full_mapping_v201901b.tar.gz
tar -xf full_mapping_v201901b.tar.gz 
```

## chocophlan
```
cd /cluster/tufts/biocontainers/datasets/humann
mkdir chocophlan
cd chocophlan
wget --no-check-certificate http://huttenhower.sph.harvard.edu/humann_data/chocophlan/full_chocophlan.v201901_v31.tar.gz 
tar -xf full_chocophlan.v201901_v31.tar.gz

humann_config --update database_folders nucleotide /cluster/tufts/biocontainers/datasets/humann/chocophlan
```


## uniref
```
cd /cluster/tufts/biocontainers/datasets/humann
mkdir uniref
cd uniref
wget --no-check-certificate https://huttenhower.sph.harvard.edu/humann_data/uniprot/uniref_annotated/uniref90_annotated_v201901b_full.tar.gz
tar -xf uniref90_annotated_v201901b_full.tar.gz
rm uniref90_annotated_v201901b_full.tar.gz
humann_config --update database_folders protein /cluster/tufts/biocontainers/datasets/humann/uniref
```

## Update permission
```
chmod -R 775 /cluster/tufts/biocontainers/datasets/humann
```


## MetaPhlAn database
When I run humann 3.8, I encountered the following error:

> ERROR: The MetaPhlAn taxonomic profile provided was not generated with the database version v3 or vOct22 . Please update your version of MetaPhlAn to at least v3.0 or 
> if you are using MetaPhlAn v4 please use the database vOct22.

The solution from this post seems to fix the error:

https://zhuanlan.zhihu.com/p/685102224

### Manually download MetaPhlAn database


```
module load humann/3.8
mkdir /cluster/tufts/biocontainers/datasets/humann/metaphlan
cd /cluster/tufts/biocontainers/datasets/humann/metaphlan
singularity exec /cluster/tufts/biocontainers/images/humann_3.8.sif metaphlan --install --index mpa_vOct22_CHOCOPhlAnSGB_202212  --bowtie2db .

```
## Definition file
```
bootstrap: docker
from: diddlydoodles/humann:3.8

%environment
######################################################################
export LC_ALL=C


%post
######################################################################
APP=humann3
APPVER=3.8


humann_config --update database_folders nucleotide /cluster/tufts/biocontainers/datasets/humann/chocophlan
humann_config --update database_folders protein /cluster/tufts/biocontainers/datasets/humann/uniref
humann_config --update database_folders utility_mapping /cluster/tufts/biocontainers/datasets/humann/utility_mapping


######################################################################
```



## Run the command
```
humann --input examples/demo.fasta --output demo_out --metaphlan-options  "--index mpa_vOct22_CHOCOPhlAnSGB_202212 --bowtie2db /cluster/tufts/biocontainers/datasets/humann/metaphlan"
```

## Binding the metaplan database to the container
If I bind the metaplan database into the container, I will not need to add the `--metaphlan-options` in the command line
```
#
# prepend-path and set SINGULARITY_BIND
#
prepend-path PATH            /cluster/tufts/biocontainers/tools/humann/3.8/bin
prepend-path --delim=, SINGULARITY_BIND /cluster/tufts
prepend-path --delim=, SINGULARITY_BIND  /cluster/tufts/biocontainers/datasets/humann/metaphlan:/usr/local/lib/python3.8/dist-packages/metaphlan/metap
hlan_databases
#
# set environment variable
#

```

Then I can simply run the command like this:

```
humann --input examples/demo.fasta --output demo_out 
```