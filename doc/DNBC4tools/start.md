## **DNBC4tools**

### **Software Description**
- This software packages the process of DNBC4tools to pypi [https://pypi.org/project/DNBC4tools/](https://pypi.org/project/DNBC4tools/).
- You can directly use pip to install and update.

### **Software Usage**
#### Software Catalog
 After conda installs the software environment, the command line "DNBC4tools" will be generated in the environment directory "bin"
<br /> example: **/miniconda3/bin/env/DNBC4tools/bin/DNBC4tools**
<br />

### Software Command Parameters
#### **DNBC4tools run**

**Usage**

- **Required parameter**：
  - **--name** sample name.
  - **--cDNAfastq1** The R1 sequence of the sample cDNA library, multiple files are separated by commas, and the sequence is the same as that of cDNA R2.
  - **--cDNAfastq2** The R2 sequence of the sample cDNA library, multiple files are separated by commas, and the sequence is the same as that of cDNA R1.
  - **--oligofastq1** The R1 sequence of the sample oligo library, multiple files are separated by commas, and the sequence is the same as that of oligo R2.
  - **--oligofastq2** The R2 sequence of the sample oligo library, multiple files are separated by commas, and the sequence is the same as that of oligo R1.
  - **--starIndexDir** The STAR index path of the species' genome.
  - **--gtf**: Species annotation file, which needs to match the genome file.
- **Optional parameter**：
    - **--species** The species name corresponding to the sample, the default is NA, it is recommended to select, the species name will be displayed in the report, and if the species is human and mouse, the cell population will be annotated in the analysis.
    -  **--outdir** The result is a directory on which a directory of sample names is generated. Defaults to the current path.
    - **--thread** The number of processes used for software analysis, the default is 4.
    -  **--cDNAconfig** The structure and whitelist of cell barcode file of the cDNA library, default is the config location of the package.
    - **--oligoconfig** The structure and whitelist of cell barcode file of the oligo library, default is the config location of the package.
    - **--oligotype** The whitelist file of the oligo library, which defaults to the config location of the package.
    - **--calling_method** Methods for identifying empty droplets, including barcoderanks and emptydrops, default is emptydrops. 
    - **--expectcells** The expected number of beads to capture, this parameter applies to emptydrops, defaults is 3000.
    - **--forcecells** Cut off the specified number of beads for subsequent analysis, defalut is 0, no cut off.
    - **--mtgenes** Set mitochondrial genes(mtgene list file path) or auto, auto is find genes starting with MT or mt. mtgenes's file like [this](../gene.list)
    - **--process** Select the steps to be analyzed, including data, count, analysis, and report. If this parameter is selected, some of the steps are in error and the step can be rerun.
- **FLAG parameter**：
    - **--no_introns** By default, the reads of exon and intron are calculated at the same time. If this parameter is selected, the intron reads will not be calculated. It is recommended not to select.
    - **--mixseq** cDNA and oligo library are sequenced on the same chip, and this parameter is added when the sequencing mode is cDNA sequencing mode. In the presence of this parameter, the --oligoconfig parameter is invalid, and DNBelabC4_scRNA_oligomix_readStructure.json is used.
    - **--no_bam** By default, anno_decon_sort.bam will be moved to the result directory output. If this parameter is selected, this operation will not be performed.
    - **--dry** The code of the step is printed directly without analysis.



#### **DNBC4tools data** 

Analyze data quality control and data comparison annotation to generate final.bam and raw_matrix, the corresponding directory is *01.data*.

#### **DNBC4tools count** 

Analyze and filter empty droplets and size magnetic beads matching, saturation curve and generate filter_matrix, the corresponding directory is *02.count*.

#### **DNBC4tools analysis** 

Use filter_matrix for data filtering and cluster annotation. Provide more parameter modifications. If you need to adjust the analysis results, you can use analysis alone to analyze and then use run "--process report" to generate a report. The corresponding directory is *03.analysis*.

#### **DNBC4tools report** 

Generate result files and result reports, the corresponding directories are *04.report* and output.

#### **DNBC4tools multi** 

To generate multiple shells for multiple samples and run them manually, all samples need to use the same reference gene and annotation files.
**--list** Sample list of this parameter. [refer](../list.md)

#### **DNBC4tools clean**

 If it is determined that reanalysis is not required, use the command to delete large temp files, use "DNBC4tools clean" to delete all reported samples in this directory by default, or use "--name" to selectively delete samples. The result will also generate a result file to merge the metrics_summary.xls of the sample.

#### **DNBC4tools mkref** 

Generate star index file based on genome and annotation file gtf. [refer](../database.md)

<br />


### **Example of Use**
- **run** 
<br /> We recommend that in addition to adding required parameters, "--species" and "--thread" are also necessary. Larger threads can reduce the time of process analysis.
```
### Run the pipeline in local
/mgi/miniconda3/envs/DNBC4tools/bin/DNBC4tools run --cDNAfastq1 /test/data/test_cDNA_R1.fastq.gz \
--cDNAfastq2 /test/data/test_cDNA_R2.fastq.gz \
--oligofastq1 /test/data/test_oligo1_1.fq.gz,/test/data/test_oligo2_1.fq.gz \
--oligofastq2 /test/data/test_oligo1_2.fq.gz,/test/data/test_oligo2_2.fq.gz \
--starIndexDir /database/Mouse/mm10/ --gtf /database/Mouse/mm10/genes.gtf \
--name test --species Mouse --thread 10
```
- **multi**
<br /> All samples need to use the same reference gene and annotation files
```
/mgi/miniconda3/envs/DNBC4tools/bin/DNBC4tools multi --list sample.list \
--starIndexDir /database/Mouse/mm10/ --gtf /database/Mouse/mm10/genes.gtf \
--thread 10
```
- **clean**
```
### Delete temp files for all analysis samples in this directory
/mgi/miniconda3/envs/DNBC4tools/bin/DNBC4tools clean

### Delete the temp files of sample A
/mgi/miniconda3/envs/DNBC4tools/bin/DNBC4tools clean --name A
```
<br />


## FAQ
Frequently Asked Questions [here](./faq.md)
