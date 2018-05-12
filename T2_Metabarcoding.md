# Metabarcoding excercise
First, let's set the working directory
```
workdir="/home/ikasleXX/REPOSITORIO/metabarcoding"
```

We will start by visualizing the data files:
```
cd ${workdir}/1-Rawdata
head Pool1_1.fastq.gz
```

The file is compressed, so we cannot see what is inside. Let's uncompress the file:
```
cd ${workdir}/1-Rawdata
gunzip Pool1_1.fastq.gz
gunzip Pool1_2.fastq.gz
```
Or alternatively, we can use the improved software `pigz`, which allows parallelizing:
```
cd ${workdir}/1-Rawdata
pigz -dc Pool2_1.fastq.gz
pigz -dc Pool2_2.fastq.gz
```
We can always use wild cards:
```
cd ${workdir}/1-Rawdata
pigz -dc Pool3_[1-2].fastq.gz
```
