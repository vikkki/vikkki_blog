---
title: "Ways to download fastq files of a GEO dataset"
description: Derecrly download(?) using prefetch or from EBI
toc: true
comments: false
layout: post
author: Vikkki
categories: [fastq, GEO, downloading, prefetch]
image: images/info.png
badges: true
---

## Download from NCBI
On the page of target dataset, here I'm trying to retrieve data from GEO Series Series GSE173771 [here](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE173771)

![]({{site.baseurl}}/images/sra.png "SRA selector")

Below Supplementary fild section, click the link of "SRA Run Selector" to obtain **accession list**:

![]({{site.baseurl}}/images/acclist.png "make accession list")

### Prefetch

"Prefetch is a part of the SRA toolkit. This program downloads Runs (sequence files in the compressed SRA format) and all additional data necessary to convert the Run from the SRA format to a more commonly used format. Prefetch can be used to correct and finish an incomplete Run download."

For more info on prefetch and sra download, you can also refer to [this page](https://www.ncbi.nlm.nih.gov/sra/docs/sradownload/).

To install SRA toolkit of your system: https://github.com/ncbi/sra-tools/wiki/

After setting up the toolkit, entire target folder:

```shell
cd ./data_path_to_download_into/

# make command list:

cat ../SRR_Acc_List.txt | while read id
do
echo prefetch ${id} -O ./
done > prefetch.command

# take a look
cat prefetch.command

# run downloading
nohup bash prefetch.command &
```


### Format convert
```shell
mkdir fastq

cd fastq
ln -s ~/path/to/1.sra_data/* ./fastq/

cat ../SRR_Acc_List.txt | while read id
do
# fasterq-dump is also part of src-toolkit
echo "fasterq-dump -e 32 --split-files -O ./ --outfile ${id}.fastq ../sra/${id}.sra"    
echo "pigz -p 16 -f ./${id}_1.fastq"
echo "pigz -p 16 -f ./${id}_2.fastq"
done > sra2fq.sh

cat sra2fq.sh
nohup bash sra2fq.sh &
```


## Or, it could be another option to download data (fastq) from EMBL-EBI

### Generate download path
On ENA homepage: 
https://www.ebi.ac.uk/ena/browser/home search for GSE173771:

https://www.ebi.ac.uk/ena/browser/view/PRJNA726999?show=reads

We can download fastq files directly, or with Aspera, use command line to download them.

![]({{site.baseurl}}/images/ebi.png "EBI project page")



Created at 03/21/2022, editted at 5/10/2022 by vikkki 🌲
