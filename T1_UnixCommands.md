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

## Preparing 
