Easy installations and virtual environments with conda
========================================================
author: Maxime Borry
date: 24/01/2020
width: 1440

Follow the presentation
=========================================================
## [maxibor.github.io/conda-presentation](https://maxibor.github.io/conda-presentation)

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

### When can't "install" programms on a computer, it's because you don't have write access to these directories

What's in my path ?
========================================================
incremental: true
For example, let's have a look at `/usr/bin` !


```bash
borry@mpi-sdag1:~$ ls -1 /usr/bin
...
head
headerdoc2html
heap
heap32
hexdump
hidutil
hiutil
host
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
"Conda is an open source, **cross-platform**, **language-agnostic** **package and environment management system** distributed by Anaconda."

- Free
- **No admin rights required**
- Tons of bioinformatic packages available
- Easy to install

Install conda
========================================================
incremental: true
### Two distributions:
- Anaconda (fully featured, heavy)
- **Miniconda** (bare minimum)

### Installation for Mac

```bash
borry@maxime:~$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh -O ~/miniconda.sh
borry@maxime:~$ bash ~/miniconda.sh
```
### Installation for Linux

```bash
borry@mpi-sdag1:~$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
borry@maxime:~$ bash ~/miniconda.sh
```

Using conda
========================================================
incremental: true
## Install a package
##### When you install a package, conda automatically handles the installation of all its dependancies

### Jupyter notebook

```bash
(base)borry@mpi-sdag1:~$ conda install jupyter
```
## Install a package from a specific channel
##### BWA

```bash
(base)borry@mpi-sdag1:~$ conda install -c bioconda bwa
```
### [Bioconda](https://bioconda.github.io/recipes.html): Channel for bioinformatics packages
##### [Bioconda: sustainable and comprehensive software distribution for the life sciences](https://www.nature.com/articles/s41592-018-0046-7)
### [conda-forge](https://conda-forge.org/feedstocks/): community-led (huge) collection of recipes

Everything with conda is an environment
========================================================
incremental: true
## An environement is an isolated sandbox that allows a fine control on program's versions and dependancies

##### By default, you're in the **base** environment

```bash
(base)borry@mpi-sdag1:~$ conda env list
# conda environments:
#
base          *  /projects1/clusterhomes/borry/miniconda3
```

##### But can create new environments...

```bash
(base)borry@mpi-sdag1:~$ conda create -n myEnvName
```
Everything with conda is an environment(2)
========================================================
incremental: true

##### Change your current environemnt (activate)

```bash
(base)borry@mpi-sdag1:~$ conda activate myEnvName
```

##### **Note that your prompt now includes the name of the active environment**

##### Install a package in this environment, for example, RStudio

```bash
(myEnvName)borry@mpi-sdag1:~$ conda install -c r rstudio
```

##### And go back to the **base** environment

```bash
(myEnvName)borry@mpi-sdag1:~$ conda deactivate
```

##### **The packages installed in one environement are not accessible from outside !**

Let's create an environment for MetaPhlan2 and Krona
========================================================
incremental: true

[Krona plots](http://marbl.github.io/Krona/examples/rdp.krona.html)


```bash
(base)borry@mpi-sdag1:~$ conda create -n metaphlan
(base)borry@mpi-sdag1:~$ conda activate metaphlan
(metaphlan)borry@mpi-sdag1:~$ conda install -c bioconda metaphlan2
# Now you can use metaphlan !
(metaphlan)conda install -c bioconda krona
```

Now let's share our environement !

```bash
(metaphlan)borry@mpi-sdag1:~$ conda env export > metaphlan_env.yml
```

And recreate it on another machine from the environment file

```bash
(base)borry@maxime:~$ conda env create -f metaphlan_env.yml
```

Other useful conda commands
========================================================
incremental: true
##### List installed packages and versions in an environment

```bash
(metaphlan) borry@mpi-sdag1:~$ conda list
```

##### Uninstall a package (here Krona)

```bash
(metaphlan)borry@mpi-sdag1:~$ conda remove krona
```

##### Delete an environment

```bash
(base)borry@mpi-sdag1:~$ conda env remove -n metaphlan
```

### Conda documentation: [conda.io/docs](https://conda.io/docs)

Faster conda with Mamba
==========================

[Mamba](https://github.com/mamba-org/mamba) is conda drop-in replacement, with a faster dependancy solver.

```bash
conda install -c conda-forge mamba
```




Bonus: building conda recipes
========================================================

- [conda-build documentation](https://docs.conda.io/projects/conda-build/en/latest/user-guide/index.html)
- [conda recipes skeleton for R packages on CRAN](https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-r-pkgs.html)
- [conda recipes skeleton for Python packages on Pypi with **Grayskull**](https://github.com/marcelotrevisani/grayskull)
