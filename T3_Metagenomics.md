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
AdapterRemoval --file1 ${workdir}/1-Rawdata/${sample}.fastq --basename ${workdir}/2-QualityFiltered/AI1 --minquality 30 --trimns --maxns 5 --qualitymax 60 --threads 4
done
```
