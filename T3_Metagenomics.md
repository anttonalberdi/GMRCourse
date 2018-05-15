# Metagenomics excercise
First, let's set the working and software directories
```
workdir="/home/ikasleXX/metagenomics"
softwaredir="/home/ikasle01/REPOSITORIO/software"
```
We can declare a list of samples to loop across them:
```
declare -a samples=("AI1" "AI2" "AI3" "VH1" "VH2" "VH3")
```
We first need to apply a quality filter
```
for sample in "${samples[@]}"
do
AdapterRemoval --file1 ${workdir}/1-Rawdata/${sample}.fastq --basename ${workdir}/2-QualityFiltered/${sample} --minquality 30 --trimns --maxns 5 --qualitymax 60 --threads 4
mv ${workdir}/2-QualityFiltered/${sample}.truncated ${workdir}/2-QualityFiltered/${sample}.fastq
done
rm ${workdir}/2-QualityFiltered/*settings
rm ${workdir}/2-QualityFiltered/*discarded
```
We can now remove duplicated clonal reads
```
for sample in "${samples[@]}"
do
cat ${workdir}/2-QualityFiltered/${sample}.fastq | seqkit rmdup -s -o ${workdir}/3-DuplicatesRemoved/${sample}.fastq
done
```
Now is time to remove host DNA
