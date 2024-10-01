# cuda_setup_on_Ubuntu

## Install Nvidia Driver
- `sudo apt update` and `sudo apt upgrade`
- Remove previous Nvidia installations: `sudo apt autoremove nvidia* --purge` and `sudo /usr/local/cuda-X.Y/bin/cuda-uninstaller` and `rm -rf /usr/local/cuda-X.Y` -> https://docs.nvidia.com/cuda/cuda-installation-guide-linux/#handle-conflicting-installation-methods
- Check Ubuntu devices: `ubuntu-drivers devices` pick the one you like, I picked the one with recommended at the end
- Install NVIDIA drivers: `sudo apt install nvidia-driver-560`
- Reboot and check: `reboot` and `nvidia-smi`

## Install CUDA driver
- Pick the CUDA version from here: https://developer.nvidia.com/cuda-toolkit-archive
- Choose installer type: runfile and follow the instructions
  - `wget xx.run`
  - install CUDA toolkit: `sudo sh xx.run` (you will see "existed package manager found" -> continue -> unchecked driver -> install)
  - Set Path by adding `export PATH=$PATH:/usr/local/cuda-x.y/bin` and `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-x.y/lib64` (watch for the output of previous step)
  - remove runfile: `rm xx.run`

## Install cuDNN
- Check the cuDNN version here: https://developer.nvidia.com/cudnn-archive
- Install with instructions on the website

## Test CUDA on Pytorch
- Create a python virtual environment and activate it: `python -m venv env` and `source env/bin/activate`
- Install pytorch: `pip3 install torch torchvision torchaudio`
- Open Python and execute a test
  ```python
  import torch
  print(torch.cuda.is_available()) # should be True

  t = torch.rand(10, 10).cuda()
  print(t.device) # should be CUDA
  ```

## References:
- Official guide: https://docs.nvidia.com/cuda/cuda-installation-guide-linux/
- https://gist.github.com/denguir/b21aa66ae7fb1089655dd9de8351a202 
- sidenotes:
1. add ppa to apt (search gpu driver ppa) (https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa?field.series_filter=noble) and install using sudo apt install nvidia-driver-550
2. go to cuda download: https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=24.04&target_type=runfile_local
3. select 24.04 runfile (local) and run commands
4. check by using nvcc --version
Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-12.6/

Please make sure that
 -   PATH includes /usr/local/cuda-12.6/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-12.6/lib64, or, add /usr/local/cuda-12.6/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-12.6/bin
***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least 560.00 is required for CUDA 12.6 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run --silent --driver
