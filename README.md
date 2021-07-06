# Using python and R applications and containers on Clusters
 6 june 2021
 
 
 
 # Objectives
 1. How to install python/r applications on clusters
 
 2. How to create and use singularity clusters
 
 
 # Intro
 
 10 most popular programming languages
 
 1. C 
 2. Java
 3.python 
 7. R




       
        1. load R with the software stack

        module load gcc r
        R

        (in batch mode) : Rscript

2. install packages. 
 
        a. As usuall in R: install.packages("ggplot")

        b. install from github:
       install devtools 
       from github:  intall_github("bala/blabla")


3. Example create a slurm script to run the job



Reserve a node

Sinteractive -m 2G -c 1

cat BATCH SCRIPT chisq.R
Batch script


# Share R packages

/work/FAC/...
chmod 3770 path/to/folder  # make it accessible to anyone
export R_LIBS='path' # it will install all packages in that location. IMPORTANT

# use multi-core parallelism

export OMP_NUM_THREADS=x


# PYTHON

      0. load module
      module load gcc python
      
      install python apps better in virtual environments!!!!

      1. system wide app (non recommended on laptop)

      pip install package name
      or
      python3 setup.py install

      --user # to install in the user folder

      packages installed in

      BEST WAY TO INSTALL PACKAGES. IN Virtual environment

      create virtual environment
      python3 -m venv $PATH_TO_VENV

      # best way to install packages
      $PATH_TO_VENV/bin/python setup.py install

      module load gcc python
      pip3.8 install numpy==1.20.1 --user

      if installin new version of library pip remove other version of same packages



     ex. networkx lib

     pip list | grep -i networkx

     in python
     import networkx 

     source $path_to_venv/bin/python

Example:
     0. load python
          module load gcc python
     1. Create virtual environment

     mkdir new_evn
     python3 -m venv /work/FAC/FBM/DEE/isanders/popgen_to_var/IM/new_evn

     2. Activate virtual env

     source torch /work/FAC/FBM/DEE/isanders/popgen_to_var/IM/new_evn activate

     3. install library in venv

     pip install torch

     4. Run script with files
     python PyTorch.py

     5. deactivate 
     deactivate



#Usefull commands

pip install numpy==1.17.5

install list of packages in a file

pip install -r requirements.txt

check packages installed
pip list

updates setup tools in virtual

pip update -U setuptools



# Conda
package manager to install python packages


1. load appropiate module
module load gcc miniconda3

2. redifine location create ~/.condarc with c

first time: conda init bash


conda search 
conda import


# Singularity containers

compatible with dockers


     1. load singularity
    module load singularity

    2. build container with info from bleedingedge_R.def
    singularity build --fakeroot ~/Bleeding_edge_R.sif Bleeding_edge_R.def

bleedingedge_R.def file:
     
     From: ubuntu:20.04

     %post
     apt update
     apt install -y locales gnupg-agent
    sed -i '/^#.* en_.*.UTF-8 /s/^#//' /etc/locale.gen
    sed -i '/^#.* fr_.*.UTF-8 /s/^#//' /etc/locale.gen
    locale-gen

    # Specific to R installation (https://cloud.r-project.org/bin/linux/ubuntu)
    apt install -y --no-install-recommends software-properties-common dirmngr
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
    apt install -y --no-install-recommends r-base

    3. export path

    export SINGULARITY_BINDPATH="/users"

    4. run container

    singularity run ~/Bleeding_edge_R.sif Rscript ~/DCSR-Examples/python_r/chisq.R


# Slides will come...




 
