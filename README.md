# pyDNMFk-apptainer
Apptainer image to run [pyDNMFk](https://github.com/lanl/pyDNMFk) on EMSL's tahoma

Example of Slurm script
```
#!/bin/bash
#SBATCH -N 1
#SBATCH -t 00:29:00
#SBATCH -A allocation_name
#SBATCH --ntasks-per-node 36
#SBATCH -o singularity_library.output.%j
#SBATCH -e ./singularity_library.err.%j
#SBATCH -J singularity_library
#SBATCH --export ALL
source /etc/profile.d/modules.sh
export https_proxy=http://proxy.emsl.pnl.gov:3128
module purge
module load openmpi/4.1.4
export MYT=16
srun -u -l --mpi=pmi2 -N $SLURM_NNODES -n $MYT singularity exec  oras://ghcr.io/edoapra/pyDNMFk-apptainer/apptainer:latest \
python /pyDNMFk/main.py \
 --p_r=$MYT --p_c=1 --process='pyDNMFk'  --fpath='/pyDNMFk/data/' --ftype='mat' --fname='swim' \
 --init='nnsvd' --itr=5000 --norm='kl' --method='mu' --results_path='results/' \
 --perturbations=20 --noise_var=0.015 --start_k=2 --end_k=5 --sill_thr=.9 --sampling='uniform'	   < /dev/null
```
