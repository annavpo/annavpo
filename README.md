# CODON MIGRATION

## login

ssh codon-login  # use your credentials

#set up your .bashrc and .bash_profile under /homes/YOURUSERNAME

#Where to store data, where to work in codon

store important permanent data under /nfs/research/irene/YOURUSERNAME

##run jobs, store intermediate outputs that you dont need for long term here : /hps/nobackup/irene/ma/YOURUSERNAME

'''
cd /hps/nobackup/irene/ma
mkdir YOURUSERNAME
'''
##install software, conda env etc here : /hps/software/users/ma/YOURUSERNAME

cd /hps/software/users/ma/
mkdir YOURUSERNAME


##How to move data from yoda to codon 

navigate in codon:

cd /nfs/research/irene/YOURUSERNAME

RUN DATA MOVING JOB


bsub -n 8 -q datamover "/hps/software/copytools/msrsync/msrsync -p 8 /nfs/yoda/leia/research/ma/YOURUSERNAME  /nfs/research/irene/

#the above command will use 8 cores, the datamover queue and it will move the whole directory YOURUSERNAME from yoda to codon. 



##install anaconda in codon

Follow the instructions here : 
To install conda download the installer script from https://docs.conda.io/en/latest/miniconda.html and follow the instructions
https://conda.io/projects/conda/en/latest/user-guide/install/linux.html

install Anaconda under : /hps/software/users/ma/YOURUSERNAME
Once anaconda is installed in codon go to yoda 

#export environments from yoda

conda info --envs # prints the environments you have on yoda- select which ones you use and you want to move to codon


##EXAMPLE: export the base environment
mkdir yml_files
conda activate base
bsub -M 1200 -n 4 conda env export > yml_files/base.yml
conda deactivate

#Repeat the above 3 steps to export all the environments in yml files.

#move the yml files to codon 
bsub -n 8 -q datamover "/hps/software/copytools/msrsync/msrsync -p 8 /nfs/yoda/leia/research/ma/YOURUSERNAME/yml_files /hps/software/users/ma/YOURUSERNAME

#build environments from yml files
cd /hps/software/users/ma/YOURUSERNAME/
conda env create -f yml_files/base.yml

#check your environment
conda activate base
conda list

#repeat for the other environments
