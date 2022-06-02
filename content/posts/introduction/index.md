---
title: "HIHISIV - storytelling"
date: 2020-06-08
description: storyteling hihisiv
menu:
  sidebar:
    name: HIHISIV
    identifier: introduction
    weight: 10
tags: ["Basic", "Multi-lingual"]
categories: ["Basic"]
---

# Storytelling about HIHISIV

The idea about this project appeared during my Ph.D. -- when I collected and worked with transcriptome data from microarray platforms.
This page is more storytelling -- I will describe the processes that I conducted and organized to generate the database HIHISIV.
In the end, I intend to point the difficulties and challenges faced as well as the importance of collaboration.

For academic information and technical description, please, check the preprint [BioRxiv](link) and the code on [Github]().


## Data organization

The first step of this story is the data survey in the main transcriptome repositories -- GEO. Using the key-words `HIV` and `SIV` we found XX. The result was wrote in the Google sheet as show in the Figure X and posteriorly refined.

![first steps in the shared sheet.](/assets/images/googleSheet_hihisiv.png)

\clearpage

The datasets were downloaded for a directory (CEL files).
In each project, a sheet was created (in .csv file) phenodata.

All material was organized as described in this structure below:

```
hihisiv/

/hihisiv/      # hihisiv on the git
|--- a_code/
|    |--- microarray_analysis/
|    |--- rna-seq_analysis/
|         |--- get_sra/
|    |--- tables_organizing/
|--- b_database/
|    |--- tables_hihisiv/
          |--- gene_gene/
          |--- gene_species/
          |--- platform_transcript_id/
          |--- saved_google_sheet/
          |--- traits/
|    |--- Dockerfile
|    |--- init.sql
|--- c_web_app/
|    |--- hihisiv_jekyll/
|    |--- hihisiv_webapp/
|--- README.md             # description of the project
|--- docker-compose.yml
|--- metadata.html


/hihisiv_other_parts/
|--- d_documents/
|    |--- articles/
|    |--- manuscript/
|--- e_figures/
|--- f_datasets/
|    |--- microarray/
|    |--- rna-seq/
|--- g_data_results/
|    |--- microarray_results
|    |--- rna-seq_results
|--- storytelling.md

```

\newpage

### R scripts

*Microarray_analysis*

| script_name             | input                               | output                  |        library  |
|-----------------------|---------------------------------------|------------------------ |-----------------|
| parameters.R   | information about the experiment | - | |
| module_processing.R   | - | - | |
| dependencies.R |  |  | |
| raw_activity.R  | raw data (CEL files) | normalized matrix |affy, impute|
| e-set_activity.R | normalized matrix | eSet object | - |
| limma_activity.R | eSet object and phenodata matrix | differentially expressed genes matrix | limma, ggplot2, RColorBrewer|

*rna-seq_analysis*

| script_name             | input                               | output                  | library |
|-----------------------|---------------------------------------|--------------------------|------------|
| get_sra.sh   | archive with ids | SRA data | aria2c from aria2 |
| sra_to_fastq.sh | SRA data |  fastq files | fastq-dump from sratoolkit (v. 2.11.3) |
| quality_control.sh | fastq | .html reports | fastqc multiqc |
| alignment_rsem.sh | fastq paired |  | bowtie2; perl; rsem |
| limma_voom.R | | | |
| rsem_matrix.sbatch | *genes.results | geneMat.txt |bowtie2; rsem |

| tables and list             | input                               |
|-----------------------|---------------------------------------|
| GSE119234_hihisiv.txt   | list of the samples with url. Used by script. |
| GSE119234_metadata.csv   | information about samples (phenodata). |



## Docker containers

### Database docker

* Build database docker container (remove old containers and images if needed):

`cd B_Database/`
`docker build -t hihisiv-postgres .`


* Create empty directory on host to store database files, for example:

`mkdir /home/user/hihisiv_db`


### Web-app

* Build web application docker container (remove old containers and images if needed):

`cd c_web_app/hihisiv_webapp`

`docker build -t hihisiv-webapp .`


### Web-static

`bundle exec jekyll serve`


```
/home/
|--- Background # about HIV and SIV 
|--- Experiments list # normalized dataset to download
|--- Database # postgres, streamlit, database in fact
|--- Documentation
|--- Data availability ????
|--- External sources

```


### All-in-one, docker-compose

Requirements: docker and docker-composed

Installation steps:

* Edit web application container environment variables to enter database initialization file directory (`b_database/tables_hihisiv`) and database files directory:

`vi .env`

* Set `VIRTUAL_HOST` on `docker-compose.yml` to host name of the host that will run the web application.

* Start containers with:

`docker-compose up -d`

* Open on the browser:

`localhost`

* Stop docker:

`docker-compose down`



## Documents, manuscript

Contem artigos

* Manuscrito no Overleaf

## Figures

Figuras geradas no google slides:

* logo HIHISIV
* dataflow analysis
* framework

Modelo conceitual gerado no XXX



## Frustations, Difficults, challenges and conclusions

HIHISIV old version



![Graph.](e_figures/old_conceptual_model_hihisiv.png)

![Graph.](e_figures/old_mainPage_hihisiv.png)
![Graph.](e_figures/old_framework_hihisiv.jpg)


## Commands

*Postgres - main commands*

    * psql hihisiv
    * \d schema
    * \d virus insert
    * psql -f arq.sql hihisiv
    * copy experiment form "arq.csv" delimiter ',' csv
    * select * from design;
    * delete from XX; todo o conteúdo da tabela é deletado.
    * dropdb hihisiv
    * createdb hihisiv
    * barra q quit the database
    * drop table XX;
    * drop table XXX cascade;  

*Comandos em docker*

    * docker images
    * docker ps
    * docker stop <container_id>  
    * docker rmi --force <container_id>

<!--

FAIR
