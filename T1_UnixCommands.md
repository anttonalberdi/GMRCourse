# Tutorial 1 - Basic Unix Commands
## Connecting to a remote server
We will run the exercises in the UPV/EHU's computation unit **Arina**.

http://www.ehu.eus/sgi/recursos/cluster-arina

We have 6 user accounts:

>ikasle02, ikasle03, ikasle04, ikasle06, ikasle07, ikasle08

One server:

>txinparta.lgp.ehu.es

One password per user account:

> XXXXXXXXX

In order to connect to the server we need a ssh agent:

> - Windows: Putty (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
> - Linux: Terminal
> - Mac: Terminal/iTerm

And we need to stay connected to the UPV/EHU's network

> - Eduroam
> - VPN

Open the terminal and type:

```
ssh ikasleXX@txinparta.lgp.ehu.es
```

Type the password
```
ikasleXX@txinparta.lgp.ehu.es's password:
```
Voilà, you are connected to the remote server
```
ikasleXX@nd0 ~]$:
```
## Basic Unix Commands
We will first check what files and directories are in our home directory:
```
ls
```
To list the files and directories inside a child directory:
```
ls REPOSITORIO
```
If we want to move to that child directory and list the items:
```
cd REPOSITORIO
ls
```
The `ls` command lists the files and directories, but does not provide any further information. In order to know more about the items in a given directory, we need to add arguments:
```
ls -l
```
Or even better:
```
ls -lh
```
Typing 'pwd' (Print Working Directory), we can now the absolute path of the directory we are now:
```
pwd
```
And to goind back to a parent directory we can either use a relative path command:
```
cd ../
cd REPOSITORIO
```
Or we can use absolute path command:
```
cd /home/ikasleXX/
cd /home/ikasleXX/REPOSITORIO
```
If we want to visualize a text file, we have different options. Check these out:
```
cat readme.txt
head readme.txt
tail readme.txt
less readme.txt
more readme.txt
```
If we want to edit the contents, we can use the text editor `vi`:
```
vi readme.txt
Esc
:
x
```
We can rename a file using `mv`:
```
mv readme.txt alreadyread.txt
ls -lh
```
Or we can move the file somewhere else also using `mv`:
```
mv alreadyread.txt /home/ikasleXX/
ls -lh
cd ../
ls -lh
```
We could also move and rename it:
```
mv alreadyread.txt /home/ikasleXX/renamedfile.txt
ls -lh
cd ../
ls -lh
```
We can do absolutely the same but copying instead of moving/renaming using the command `cp`:
```
cp alreadyread.txt /home/ikasleXX/renamedfile.txt
ls -lh
cd ../
ls -lh
```
We can always come back to our working directory using cd+absolute path:
```
cd /home/ikasleXX
```
Or if the path is too long and we want to create a smarter code, we can use variables:
```
pwd
workingdir=/home/ikasleXX
cd ${workingdir}
```
We can also create directories using the command `mkdir`:
```
cd ${workingdir}
mkdir newdirectory
ls -lh
```
And remove files and directories using the command `rm`:
```
rm alreadyread.txt
rm -r newdirectory
ls -lh
```

## Preparing the home directories for the excercises
We will create two directories, one for each of the two excercises we will perform:
```
cd ${workingdir}
mkdir metabarcoding
mkdir metagenomics
ls -lh
```
### Metabarcoding
For metabarcoding, we will create the following directories:
```
cd ${workingdir}/metabarcoding
mkdir 1-Rawdata
mkdir 2-QualityFiltered
mkdir 3-Sorted
mkdir 4-Filtered
mkdir 5-OTUs
mkdir 6-Blast
```
Alternativelly, we could also create the directories from the parent directory:
```
cd ${workingdir}
mkdir metabarcoding/1-Rawdata
mkdir metabarcoding/2-QualityFiltered
mkdir metabarcoding/3-Sorted
mkdir metabarcoding/4-Filtered
mkdir metabarcoding/5-OTUs
mkdir metabarcoding/6-Blast
```
Or even using absolute paths:
```
mkdir /home/ikasleXX/metabarcoding/1-Rawdata
mkdir /home/ikasleXX/metabarcoding/2-QualityFiltered
mkdir /home/ikasleXX/metabarcoding/3-Sorted
mkdir /home/ikasleXX/metabarcoding/4-Filtered
mkdir /home/ikasleXX/metabarcoding/5-OTUs
mkdir /home/ikasleXX/metabarcoding/6-Blast
```
Then we will transfer the raw data files to the `1-Rawdata` directory:
```
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool1_1.fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool1_2.fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool2_1.fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool2_2.fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool3_1.fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool3_2.fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
```
Or we could do the same operation using wild cards, which is much easier and effective:
```
cp /home/ikasle01/REPOSITORIO/metabarcoding/* /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/*.gz /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool* /home/ikasleXX/metabarcoding/1-Rawdata
cp /home/ikasle01/REPOSITORIO/metabarcoding/Pool[1-3]_[1-2].fastq.gz /home/ikasleXX/metabarcoding/1-Rawdata
```

### Metagenomics
For metagenomics, we will create the following directories:
```
cd ${workingdir}/metagenomics
mkdir 1-Rawdata
mkdir 2-QualityFiltered
mkdir 3-DuplicatesRemoved
mkdir 4-HostRemoved
mkdir 5-HumanRemoved
mkdir 6-Assembly
```
Now we can transfer the raw data files to the `1-Rawdata` directory:
```
cp /home/ikasle01/REPOSITORIO/metagenomics/* /home/ikasleXX/metagenomics/1-Rawdata
```

