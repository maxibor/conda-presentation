Finger in the nose installations and virtual environments with conda
========================================================
author: Maxime Borry
date: 17/10/2018
width: 1440

Follow the presentation
=========================================================
## [huit.re/conda](https://huit.re/conda)

What does it mean to install a program ?
========================================================
incremental: true
### -> Copy/move the executable of the program in the `PATH`

### What is the `PATH` ?


```bash
borry@mpi-sdag1:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/
games:/usr/local/games:/opt/dell/srvadmin/bin
```

- `/usr/local/sbin`
- `/usr/local/bin`
- `/usr/sbin`
- `/usr/bin`
- ...

### You can't "install" programms on SDAG because you don't have write access to these directories

What's in my path ?
========================================================
incremental: true
For example, let's have a look at `/usr/bin` !


```bash
borry@mpi-sdag1:~$ ls /usr/bin
...
erb                             savelog
erb1.9.1                        sbatch
erb2.0                          sbcast
evince                          scancel
evince-previewer                scconf-bootinfo
evince-thumbnailer              scconf-tool
ex                              scontrol
...
```

Great, but what about Conda ?
========================================================
incremental: true

### I have a confession: this is not my real path

```bash
borry@mpi-sdag1:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/
games:/usr/local/games:/opt/dell/srvadmin/bin
```

### Here is my real path

```bash
borry@mpi-sdag1:~$ echo $PATH
/projects1/clusterhomes/borry/miniconda3/bin:/usr/local/
sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/
games:/usr/local/games:/opt/dell/srvadmin/bin
```

### The location where executables will be installed by Conda
```
/projects1/clusterhomes/borry/miniconda3/bin
```
Conda
========================================================
incremental: true
"Conda is an open source, **cross-platform**, **language-agnostic** **package and environment management system** distributed by Continuum Analytics."

- Free
- **No admin rights required**
- Tons of bioinformatic packages available
- Easy to install

Install conda
========================================================
incremental: true
### Two distributions:
- Anaconda (fully fledged, heavy)
- **Miniconda** (bare minimum)

### Installation for Mac
```
wget https://repo.continuum.io/miniconda/Miniconda3-3.7.0-Linux-x86_64.sh -O ~/miniconda.sh
bash ~/miniconda.sh
```
### Installation for Linux
```
https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
bash ~/miniconda.sh
```

Using conda
========================================================
incremental: true
## Install a package
##### When you install a package, conda automatically handles the installation of all its dependancies

### Jupyter/IPython notebook
```
conda install jupyter
```
## Install a package from a specific channel
##### BWA
```
conda install -c bioconda bwa
```
### [Bioconda](https://bioconda.github.io/recipes.html): Channel for bioinformatics packages

Everything with conda is an environment
========================================================
incremental: true
## An environement is an isolated sandbox that allows a fine control on program's versions and dependancies

##### By default, you're in the **base** environment
```
borry@mpi-sdag1:~$ conda env list
# conda environments:
#
base          *  /projects1/clusterhomes/borry/miniconda3
```

##### But can create new environments...
```
conda create -n myEnvName
```
Everything with conda is an environment(2)
========================================================
incremental: true

##### Change your current environemnt (activate)
```
borry@mpi-sdag1:~$ source activate myEnvName
```

##### **Note that your prompt now includes the name of the active environment**

##### Install a package in this environment, for example, RStudio
```
(myEnvName) borry@mpi-sdag1:~$ conda install -c r rstudio
```

##### And go back to the **base** environment
```
(myEnvName) borry@mpi-sdag1:~$ source deactivate
```

##### **The packages installed in one environement are not accessible from outside !**

Let's create an environment for MetaPhlan2 and Krona
========================================================
incremental: true

[Krona plots](http://marbl.github.io/Krona/examples/rdp.krona.html)

```
borry@mpi-sdag1:~$ conda create -n metaphlan
borry@mpi-sdag1:~$ source activate metaphlan
(metaphlan) borry@mpi-sdag1:~$conda install -c bioconda metaphlan2
# Now you can use metaphlan !
(metaphlan) conda install -c bioconda krona
```

Now let's share our environement !
```
(metaphlan) borry@mpi-sdag1:~$ conda env export > metaphlan_env.yml
```

And recreate it on another machine from the environment file
```
borry@maxime:~$ conda env create -f metaphlan_env.yml
```

Other useful conda commands
========================================================
incremental: true
##### List installed packages and versions in an environment
```
(metaphlan) borry@mpi-sdag1:~$ conda list
```

##### Uninstall a package (here Krona)
```
(metaphlan) borry@mpi-sdag1:~$ conda remove krona
```

##### Delete an environment
```
borry@mpi-sdag1:~$ conda env remove -n metaphlan
```

### Conda documentation: [conda.io/docs](https://conda.io/docs/)

