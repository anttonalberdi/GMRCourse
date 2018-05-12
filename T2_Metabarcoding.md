# Metabarcoding excercise
## Uncompressing and visualizing data files
First, let's set the working directory
```
workdir="/home/ikasleXX/metabarcoding"
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
pigz -dc Pool2_1.fastq.gz > Pool2_1.fastq
pigz -dc Pool2_2.fastq.gz > Pool2_2.fastq
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
## Quality-filtering and merging paired-end reads
We will use AdapterRemoval to quality-filter and merge paired-end reads:
```
AdapterRemoval -h
```
This operation will require larger computational requirements, so we will 1) copy the data files to the computation unit, 2) run the command, and 3) copy the data files back to our directories.
```
#Copy files to scratch
cd ${workdir}/
cp 1-Rawdata/*.fastq /home/ikasleXX/SCRATCH

#Run Adapter Removal
AdapterRemoval --file1 /home/ikasleXX/SCRATCH/Pool1_1.fastq --file2 /home/ikasleXX/SCRATCH/Pool1_2.fastq --basename home/ikasleXX/SCRATCH/Pool1 --minquality 30 --minlength 300 --trimns --maxns 5 --qualitymax 60 --threads 4 --collapse

#Do the same with the other two pools.

#Check the output files
ls -lh home/ikasleXX/SCRATCH/

#Copy files back to our directory
cp home/ikasleXX/SCRATCH/Pool1.collapsed ${workdir}/2-QualityFiltered

#Check the files have been succesfully copied
ls -lh ${workdir}/2-QualityFiltered

#Remove the scratch files
rm home/ikasleXX/SCRATCH/*
```


