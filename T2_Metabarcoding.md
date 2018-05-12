# Metabarcoding excercise
## Uncompressing and visualizing data files
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
Now we can see what the files contain:
```
cd ${workdir}/1-Rawdata
head Pool1_1.fastq
```
## Quality checking the original data files
We will use FastQC to quality-check the data files. Let's see what the help text says:
```
fastqc -h
```
We will output the files to a new directory inside `1-Rawdata`, which needs to be created:
```
cd ${workdir}/1-Rawdata
mkdir fastqc
ls -lh
```
Now we can run `fastqc`:
```
cd ${workdir}/
fastqc 1-Rawdata/*.fastq -o 1-Rawdata/fastqc -t 4
```
The `.html` output files need to be visualized in an Internet Browser, in our local computer. For transferring files from the remote server to our local computer we can use an FTP agent, e.g. FileZilla:

https://filezilla-project.org/download.php?type=client

Connect to the server and download the files inside the directory:
```
/home/ikasleXX/metabarcoding/1-Rawdata/fastqc
```



