# Container

### Docker

#### 1. **Pull Images**

image ：[Docker Hub](https://hub.docker.com/r/lishuangshuang3/dnbc4tools)

```shell
docker pull lishuangshuang3/dnbc4tools
```

#### 2. **Creat database**

```shell
### filter gtf
docker run -P  -v $Result_LOCAL:/result lishuangshuang3/dnbc4tools \
DNBC4tools mkref --action mkgtf --ingtf /result/raw.gtf --outgtf /result/gene.gtf \
			 --attribute gene_type:protein_coding \
                         gene_type:lncRNA \
                         gene_type:IG_C_gene \
                         gene_type:IG_D_gene \
                         gene_type:IG_J_gene \
                         gene_type:IG_LV_gene \
                         gene_type:IG_V_gene \
                         gene_type:IG_V_pseudogene \
                         gene_type:IG_J_pseudogene \
                         gene_type:IG_C_pseudogene \
                         gene_type:TR_C_gene \
                         gene_type:TR_D_gene \
                         gene_type:TR_J_gene \
                         gene_type:TR_V_gene \
                         gene_type:TR_V_pseudogene \
                         gene_type:TR_J_pseudogene

### Create STAR database
docker run -P  -v $Result_LOCAL:/result lishuangshuang3/dnbc4tools \
DNBC4tools mkref --action mkref --ingtf /result/gene.gtf \
		      --fasta /result/genome.fa --star_dir /result --thread $threads

# $Result_LOCAL: directory for result.
```

#### 3. **Run docker**

```shell
### DNBC4tools run
docker run -P -v $Database_LOCAL:/database -v $Rawdata_LOCAL:/data -v $Result_LOCAL:/result lishuangshuang3/dnbc4tools \
DNBC4tools run --name $name \
--cDNAfastq1 /data/cDNA_1_R1.fq.gz,/data/cDNA_2_R1.fq.gz --cDNAfastq2 /data/cDNA_1_R2.fq.gz,/data/cDNA_2_R2.fq.gz \
--oligofastq1 /data/oligo_1_R1.fq.gz,/data/oligo_2_R1.fq.gz --oligofastq2 /data/oligo_1_R2.fq.gz,/data/oligo_2_R2.fq.gz \
--starIndexDir /database --gtf /database/gene.gtf --species $species

# $Database_LOCAL: directory on your local machine that has the database files. 
# $Rawdata_LOCAL: directory on your local machine that has the sequence data.
# $Result_LOCAL: directory for result.
# You can use --user $(id -u):$(id -g) to obtain the owner and group
```


<br />

### singularity

#### 1.**build sif**

```shell
singularity build dnbc4tools.sif docker://lishuangshuang3/dnbc4tools
```

#### 2.**Creat database**

```shell
### filter gtf
export SINGULARITY_BIND=$Result_LOCAL
singularity exec dnbc4tools.sif \
DNBC4tools mkref --action mkgtf --ingtf $Result_LOCAL/raw.gtf --outgtf $Result_LOCAL/gene.gtf \
			 --attribute gene_type:protein_coding \
                         gene_type:lncRNA \
                         gene_type:IG_C_gene \
                         gene_type:IG_D_gene \
                         gene_type:IG_J_gene \
                         gene_type:IG_LV_gene \
                         gene_type:IG_V_gene \
                         gene_type:IG_V_pseudogene \
                         gene_type:IG_J_pseudogene \
                         gene_type:IG_C_pseudogene \
                         gene_type:TR_C_gene \
                         gene_type:TR_D_gene \
                         gene_type:TR_J_gene \
                         gene_type:TR_V_gene \
                         gene_type:TR_V_pseudogene \
                         gene_type:TR_J_pseudogene

### Create STAR database
export SINGULARITY_BIND=$Result_LOCAL
singularity exec dnbc4tools.sif \
DNBC4tools mkref --action mkref --ingtf $Result_LOCAL/gene.gtf \
		      --fasta $Result_LOCAL/genome.fa --star_dir $Result_LOCAL --thread $threads
# $Result_LOCAL: directory for result.
# You can bind multiple directories by Using the environment variable:
# export SINGULARITY_BINDPATH=$cDNA_data,$oligo_data,$result,$database
```

#### 3. **Run singularity**

```shell
### DNBC4tools run
export SINGULARITY_BIND=$cDNA_data,$oligo_data,$result,$database
singularity exec dnbc4tools.sif \
DNBC4tools run --name $name \
--cDNAfastq1 $cDNA_data/cDNA_1_R1.fq.gz,$cDNA_data/cDNA_2_R1.fq.gz \
--cDNAfastq2 $cDNA_data/cDNA_1_R2.fq.gz,$cDNA_data/cDNA_2_R2.fq.gz \
--oligofastq1 $oligo_data/oligo_1_R1.fq.gz,$oligo_data/oligo_2_R1.fq.gz \
--oligofastq2 $oligo_data/oligo_1_R2.fq.gz,$oligo_data/oligo_2_R2.fq.gz \
--starIndexDir $database --gtf $database/gene.gtf --species $species

# You can bind multiple directories by Using the environment variable:
# export SINGULARITY_BINDPATH=$cDNA_data,$oligo_data,$result,$database
```

