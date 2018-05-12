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
The `ls`command lists the files and directories, but does not provide any further information. In order to know more about the items in a given directory, we need to add arguments:
```
ls -l
```
Or even better:
```
ls -lh
```
