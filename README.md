# PathMNIST
Solving PathMNIST with ResNet

### Create env and download dataset 
```python
# Create conda env
conda create -y -n pathmnist python=3.9 && conda activate pathmnist

# Install tensorflow, cuda and cudnn
conda install -y -c conda-forge cudatoolkit=11.8.0
python3 -m pip install nvidia-cudnn-cu11==8.6.0.163 tensorflow==2.13.0
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

# For some ubuntu versions additional steps are required ...
conda install -y -c nvidia cuda-nvcc=11.3.58
printf 'export XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib/\n' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice
cp $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/

# Download pathmnist dataset
wget -O pathmnist.npz https://zenodo.org/record/6496656/files/pathmnist.npz?download=1
```

### Run
```bash
# train
python train.py

# view training
tensorboard --logdir=logs
```