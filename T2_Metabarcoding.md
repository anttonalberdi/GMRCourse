# Metabarcoding excercise
## Uncompressing and visualizing data files
First, let's set the working directory
```
workdir="/home/ikasleXX/metabarcoding"
```
And the software directory (for programs not installed in root)
```
softwaredir="/home/ikasle01/REPOSITORIO/software"
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
pigz -d Pool2_1.fastq.gz
pigz -d Pool2_2.fastq.gz
```
We can always use wild cards:
```
cd ${workdir}/1-Rawdata
pigz -d Pool3_[1-2].fastq.gz
```
Now we can see what the files contain:
```
cd ${workdir}/1-Rawdata
head Pool1_1.fastq
```
And we can count the number of reads of each file
```
cd ${workdir}/1-Rawdata
grep -c "@M01168" Pool1_1.fastq
grep -c "@M01168" Pool1_2.fastq
grep -c "@M01168" Pool2_1.fastq
grep -c "@M01168" Pool2_2.fastq
grep -c "@M01168" Pool3_1.fastq
grep -c "@M01168" Pool3_2.fastq
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
cd ${workdir}/1-Rawdata
for i in {1..3};do
fastqc Pool${i}_1.fastq -o fastqc -t 4
fastqc Pool${i}_2.fastq -o fastqc -t 4
done
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
AdapterRemoval --file1 /home/ikasleXX/SCRATCH/Pool1_1.fastq --file2 /home/ikasleXX/SCRATCH/Pool1_2.fastq --basename /home/ikasleXX/SCRATCH/Pool1 --minquality 30 --minlength 300 --trimns --maxns 5 --qualitymax 60 --threads 4 --collapse

#Do the same with the other two pools.

#Check the output files
ls -lh /home/ikasleXX/SCRATCH/

#Copy files back to our directory
for i in {1..3};do
cp /home/ikasleXX/SCRATCH/Pool${i}.collapsed ${workdir}/2-QualityFiltered/Pool${i}.fastq
echo "Pool${i} copied succesfully"
done

#Check the files have been succesfully copied
ls -lh ${workdir}/2-QualityFiltered

#Remove the scratch files
rm /home/ikasleXX/SCRATCH/Pool*
```
## Quality checking the quality-filtered data files
We will output the files to a new directory inside `1-Rawdata`, which needs to be created:
```
cd ${workdir}/2-QualityFiltered
mkdir fastqc
ls -lh
```
Now we can run `fastqc`:
```
cd ${workdir}/2-QualityFiltered
for i in {1..3};do
fastqc Pool${i}.fastq -o fastqc -t 4
done
```
Download the `html` files to your local computer and compare the results with the pre-quality-filtered files.

## Get quality filtering statistics
We can count the number of lines to calculate the percentage of filtered reads.
```
for i in {1..3};do
before=$(cat 1-Rawdata/Pool${i}_1.fastq | wc -l)
after=$(cat 2-QualityFiltered/Pool${i}.fastq | wc -l)
perc=$((100 * after / before))
echo "Pool $i: ${perc}% of the reads were retained after QF"
done
```
## Sorting reads
We will sort the reads using DAMe

https://github.com/lisandracady/DAMe

https://github.com/shyamsg/DAMe

We need two files indicating the employed primers and tags. We will copy them from `REPOSITORIO` and store them in a new directory called `0-Documents`.
```
mkdir ${workdir}/0-Documents
cd ${workdir}/0-Documents
cp /home/ikasle01/REPOSITORIO/metabarcoding/V3-V4_primers.txt .
cp /home/ikasle01/REPOSITORIO/metabarcoding/V3-V4_tags.txt .
ls -lh
```
Let's see what the documents contain
```
cat V3-V4_primers.txt
cat V3-V4_tags.txt
```
Now we can sort the reads. For doing so, we need to run the program from the output directory, i.e. `3-Sorted/poolX`
```
cd ${workdir}
for i in {1..3}; do
mkdir ${workdir}/3-Sorted/pool${i}
cd ${workdir}/3-Sorted/pool${i}
echo "Filtering Pool${i}"
python ${softwaredir}/DAMe/sort.py -fq ${workdir}/2-QualityFiltered/Pool${i}.fastq -p ${workdir}/0-Documents/V3-V4_primers.txt -t ${workdir}/0-Documents/V3-V4_tags.txt
cd ${workdir}
done
```
Let's examine what the `sort.py` script has created.
```
ls -lh ${workdir}/3-Sorted/pool1
```
## Filtering reads
In order to assign sequences to samples, we need a document that established the relations between them.
```
cd ${workdir}/0-Documents
cp /home/ikasle01/REPOSITORIO/metabarcoding/PSInfo.txt ${workdir}/0-Documents
ls -lh ${workdir}/0-Documents
```
Let's see how it looks like
```
head ${workdir}/0-Documents/PSInfo.txt
```
We can now filter the reads from the three pools. We will retain the sequences that appear in at least 2 out of the 3 PCR replicates (argument `-y`), and have at least two copies (argument `-t`) in each of the replicates.
```
cd ${workdir}/3-Sorted
python ${softwaredir}/DAMe/filter.py -psInfo ${workdir}/0-Documents/PSInfo.txt -x 3 -y 2 -p 3 -t 2 -l 350 -o ${workdir}/4-Filtered
```
This step will take a while...

## OTU clustering
Once filtered the reads and assigned to each sample, we will cluster the sequences into OTUs. For doing so, we first need to change the format of the input file.
```
cd ${workdir}
python ${softwaredir}/DAMe/convertToUSearch.py -i ${workdir}/4-Filtered/FilteredReads.fna -lmin 380 -lmax 450 && mv ${workdir}/FilteredReads.forsumaclust.fna ${workdir}/4-Filtered/FilteredReads.forsumaclust.fna
```
Now we can to the actual OTU clustering using SUMACLUST
```
${softwaredir}/sumaclust/sumaclust -t 0.97 ${workdir}/4-Filtered/FilteredReads.forsumaclust.fna > ${workdir}/5-OTUs/FilteredReads.fna && python ${softwaredir}/DAMe/tabulateSumaclust.py -i ${workdir}/5-OTUs/FilteredReads.fna -o ${workdir}/5-OTUs/FilteredReads.tab -blast 
mv ${workdir}/5-OTUs/FilteredReads.tab.blast.txt ${workdir}/5-OTUs/FilteredReads.blast
```
Once we have the reference OTU sequences, we can assign taxonomy by aligning the sequences to reference databases.
```
export BLASTDB=/home/ikasle01/SCRATCH/NT
blastn -num_threads 16 -query ${workdir}/5-OTUs/FilteredReads.blast -db sorted_nt -out ${workdir}/6-Blast/blast.txt -outfmt 11 -max_target_seqs 10 -dust no
```
We can check how the output file looks like:
```
cd ${workdir}
less 6-Blast/blast.txt
```
The document contains a lot of information, but it is not easy to work with it. We can change the format and tabularaize the data:
```
cd ${workdir}
blast_formatter -archive ${workdir}/6-Blast/blast.txt -outfmt '6 qacc sacc sgi pident length mismatch gapopen qstart qend sstart send evalue bitscore' -out ${workdir}/6-Blast/blast.tab
```
Let's check how the new file looks like:
```
cd ${workdir}
less 6-Blast/blast.tab
```
Now, we will do some magic to get taxonomy from the GI numbers. 
## Working with OTU/taxonomy tables
We will first download the OTU and taxonomy tables to our local computers. For doing so, we can use FileZilla, or we could use scp.
```
scp ikasle01@txinparta.lgp.ehu.es:/home/ikasleXX/metabarcoding/5-OTUs/FilteredReads.tab /MER/metabarcoding
scp ikasle01@txinparta.lgp.ehu.es:/home/ikasleXX/metabarcoding/6-Blast/blast.taxonomy.txt /MER/metabarcoding
```
From now on, we will work in our local R. Let's start by setting the working environment and loading the working files:
```
setwd("/MER/metabarcoding")
otu.table <- read.table()
taxonomy.table <- read.table()

