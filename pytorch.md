## How to create a pytorch conda environment and run it on Jupyter Open OnDemand
### Load required modules
```
module load anaconda/2021.11
module load cuda/12.2
module load conda-env-mod
```
### Create conda environment
```
conda-env-mod create -n torch231 --jupyter 
module load use.own
module load conda-env/torch231-py3.9.7
```

### Install torch by pip
```
pip3 install torch torchvision torchaudio
```

### Test whether torch can detect GPU
```
import torch
torch.cuda.is_available()
torch.cuda.device_count()
torch.cuda.current_device()
torch.cuda.device(0)
torch.cuda.get_device_name(0)
```