# Pytorch_ROCm_whl
Pytorch compiled with ROCm.

---

## How to compile
  Highly recommend to use [Anaconda3](https://www.anaconda.com/).

### Install [ROCm](https://rocmdocs.amd.com/en/latest/Installation_Guide/Installation-Guide.html#ubuntu), Then install required packages:
  ```
  sudo apt install rock-dkms rocm-dev rocm-libs miopen-hip hipsparse rccl
  sudo apt install  libopenblas-dev cmake libnuma-dev autoconf build-essential ca-certificates curl libgoogle-glog-dev libhiredis-dev libiomp-dev libleveldb-dev liblmdb-dev libopencv-dev libpthread-stubs0-dev libsnappy-dev  libprotobuf-dev protobuf-compiler
  pip install enum34 numpy pyyaml setuptools typing cffi future hypothesis
  ```
  
### Get Pytorch source code from [ROCmSoftwarePlatform](https://github.com/ROCmSoftwarePlatform/pytorch).
  ```
  git clone --recursive https://github.com/ROCmSoftwarePlatform/pytorch.git
  ```

### Initialize the environment.
  ```
  cd pytorch
  /opt/rocm/bin/rocm_agent_enumerator
  ```
  
  It will show your ROCm devices, select the target, **Note: gfx906 is my device for demo**
  
  ```
  export PYTORCH_ROCM_ARCH=gfx906
  ```
  
  patch pytorch code for hipify.
  
  ```
  python tools/amd_build/build_amd.py
  ```
  
### Start compiling, this will last for a long time.
  ```
  export USE_NINJA=1
  USE_CUDA=0 USE_ROCM=1 USE_LMDB=1 USE_OPENCV=1 MAX_JOBS=2 python setup.py bdist_wheel
  ```
  
### Install whl.
  Pytorch whl file compiled will be put in `dist/` folder, install it whit pip.
  ```
  pip install dist/*.whl
  ```
  
### Test Pytorch as your wish.
  a simple test for gpu available.
  ```
  python -c "import torch;print('Torch gpu is available: ', torch.cuda.is_available())"
  ```
  
## References

1. [ROCm DocumentationLogo](https://rocmdocs.amd.com/en/latest/)
2. [【全网首发】AMD显卡上完美原生运行PyTorch攻略，无需容器(Docker)](https://zhuanlan.zhihu.com/p/67940936)
