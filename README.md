# tRNA Gene Annotation using tRNAscan-SE
This pipeline describes the installation and usage of tRNAscan-SE v2.0.12 to annotate tRNA genes in wheat genome assemblies. Suitable for HPC systems like Atlas or CCAST.

## Installation (on HPC)
```bash
# Step 1: Download and extract
wget https://github.com/UCSC-LoweLab/tRNAscan-SE/archive/refs/tags/v2.0.12.tar.gz -O trnascan-se-2.0.12.tar.gz
tar -xzf trnascan-se-2.0.12.tar.gz
cd tRNAscan-SE-2.0.12

# Step 2: Configure installation
./configure --prefix=/home/user.name/trna_envs

# Step 3: Compile and install
make
make install

# Step 4: Add to PATH
nano ~/.bashrc
# Add this line:
export PATH=$PATH:/home/user.name/trna_envs/tRNAscan-SE-2.0.12/bin
source ~/.bashrc

# Usage Example
tRNAscan-SE -o /directory/where/this/saved/trna_result/glenn_trnascan_output.txt /directory/where/this/saved/wheat.fasta
```

## SLURM Job Script

```bash
#!/bin/bash
#SBATCH -N 1
#SBATCH -n 48
#SBATCH -p atlas
#SBATCH --mem=376GB
#SBATCH -J trna_wheat
#SBATCH -A genolabswheatphg

cd /home/user.name/trna_envs

tRNAscan-SE -o /directory/where/this/saved/trna_result/wheat_trnascan_output.txt /directory/where/this/saved/wheat.fasta
```

## Post-processing

```bash
awk '$NF != "pseudo" {print $1}' sumai_trnascan_output.txt | sort | uniq -c
```

Maintainer:
Ruby Mijan