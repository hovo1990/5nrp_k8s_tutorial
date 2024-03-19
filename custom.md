
mkdir -p ~/.kube/  
cp config ~/.kube/config 




kubectl create -f simple.yaml
kubectl get pods -o wide

kubectl exec -it pod-hgrabski -- /bin/bash


# AI Training

kubectl logs gp3-username
kubectl apply -f pytorch-training.yaml

kubectl delete -f gp3-hgrabski

kubectl exec -it ollama-madhirar -- /bin/bash

# GPU

#!/usr/bin/env bash

#SBATCH --job-name=tfhvd-gpu
#SBATCH --account=use300
#SBATCH --partition=gpu
#SBATCH --nodes=2                
#SBATCH --ntasks-per-node=4
#SBATCH --cpus-per-task=8
# ----------for gpu debug------
# #SBATCH --mem=92G    
# #SBATCH --gpus=1           
# -----------for gpu
#SBATCH --mem=368G          
#SBATCH --gpus=4            #use 1-4 for gpu, 1 for gpu-debug
#SBATCH --time=00:05:00
#SBATCH --output=slurm.gpu2x4.%x.o%j.%N.out

module reset
module load slurm  
module load gcc/10.2.0          #compiler, unix module  
module load openmpi/4.1.3       #mpi module
module load singularitypro/3.9  #container
module list

export OMPI_MCA_btl='self,vader'
export UCX_TLS='shm,rc,ud,dc'
export UCX_NET_DEVICES='mlx5_0:1'
export UCX_MAX_RNDV_RAILS=1

#maybe useful for debugging
#printenv 
#nvidia-smi -q
#nvidia-smi topo -m

mpirun -n 8 singularity exec --bind /expanse,/scratch --nv /cm/shared/apps/containers/singularity/tensorflow/tensorflow-latest.sif python3 ./SI2023_MNIST_wHVD_exercise.py > stdout-gpu2x4.txt
run-hvd-main-




# GPU galilyeo

/cm/shared/apps/sdsc/galyleo/galyleo launch --account gue998 --partition gpu-debug --cpus 10 --memory 93 --gpus 1 --time-limit 00:30:00 --conda-env keras-nlp24 --conda-yml keras-nlp24.yaml --mamba



/cm/shared/apps/sdsc/galyleo/galyleo launch --reservation 5nrp-tutorial --account gue998 --partition compute --nodes 1 --memory 242 --cpus 128 --time-limit 01:00:00 --conda-env keras-nlp24 --conda-yml keras-nlp24-cpu.yaml --mamba