# Troubleshooting
some hints when creating a new venv/repo in server

```bash
    git config --global http.proxy http://proxy.cse.cuhk.edu.hk:8000/
    git clone xxxx...
    virtualenv --no-site-packages --python=python3.6 pytorch1.0
    pip install --proxy=http://proxy.cse.cuhk.edu.hk:8000/ package-name
```

-may need to set cuda path in env

#### set cocoapi

  ```bash
  git clone https://github.com/cocodataset/cocoapi.git
```

when face
```python
import pycocotools._mask as _mask
```

`ValueError: numpy.ufunc size changed, may indicate binary incompatibility. Expected 216 from C header, got 192 from PyObject`
upgrade numpy 1.15.4 to 1.16.0

when face 
```
pycocotools/_mask.c:4:20: fatal error: Python.h: No such file or directory
```
install the specific version of python-dev, such as 
```
sudo apt-get install python3.6-dev
```

#### install other requirements
```  
pip install --proxy=http://proxy.cse.cuhk.edu.hk:8000/ -r requirement.txt
pip --proxy=http://proxy.cse.cuhk.edu.hk:8000/ install scikit-image

```

#### Miscellaneous
check fold size `du -sh`

check store quota in server ` quota -s`

if gcc<4.9, maskrcnn-benchmark will have 'segment dump' problem. If you do not have root, but want to upgrade gcc version, check:https://gist.github.com/abhishekcs10/21db5064641b3cc563d193bda4fd788a

If UnicodeDecodeError accured when collect env, a pytorch bug need to be fixed before run the code. Check:https://github.com/facebookresearch/maskrcnn-benchmark/issues/265

slurm
`squeue -u USERNAME`
http://bicmr.pku.edu.cn/~wenzw/pages/slurm.html

wget with proxy
`wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz -e use_proxy=yes -e https_proxy=your proxy`

#### how to install deepin-wine wechat on ubuntu:
https://forum.ubuntu.org.cn/viewtopic.php?f=73&p=3217021&sid=6194a64cefc1f4c5ac43dcd8729ca3c8
change 18~19 ----->  18~22rc0
see in https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin-wine/

### distributed training
when face :RuntimeError: CUDA error: no kernel image is available for execution on the device (ROIAlign_forward_cuda..........
It may because the gpu arch currently is different from that when build setup
Check https://github.com/facebookresearch/detectron2/blob/master/INSTALL.md#common-installation-issues
To solve this issue, rebuild and add 'os.environ["TORCH_CUDA_ARCH_LIST"] = "5.2;6.1;7.0"# match the gpu arch!!! GTX TITANX -->5.2, GTX1080Ti,nvidia titian xp-->6.1; Nvidia titian v-->7.0' in setup.py

### build opencv
1. wget the .zip file
2. git clone opencv-contrib 
3. unzip, mkdir build
4. cd build
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=your local path \
      -D INSTALL_C_EXAMPLES=ON \
      -D INSTALL_PYTHON_EXAMPLES=ON \
      -D WITH_TBB=ON \
      -D WITH_V4L=ON \
      -D WITH_OPENGL=ON \
      -D OPENCV_EXTRA_MODULES_PATH=[your opencv contrib path]../../opencv_contrib/modules \
      -D BUILD_EXAMPLES=ON ..

```
5.make -j8
！！
fatal error: opencv2/core/utils/tls.hpp: No such file or directory
 #include <opencv2/core/utils/tls.hpp>
 https://github.com/opencv/opencv_contrib/issues/1301
 https://www.cnblogs.com/arxive/p/11778731.html
 
 
 ```
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_lbgm.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_256.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_128.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_064.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm_hd.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm_bi.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_120.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_64.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_48.i \
 wget https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_80.i
 ```
 
 
 
 
 
 can't find these file
//#include "features2d/test/test_detectors_regression.impl.hpp"
//#include "features2d/test/test_descriptors_regression.impl.hpp"

then modify the relative path to absolute path
 e.g.
#include "/data/raid/project/software/opencv4.1.2/modules/features2d/test/test_detectors_regression.impl.hpp"
#include "/data/raid/project/software/opencv4.1.2/modules/features2d/test/test_descriptors_regression.impl.hpp"

6.make install
### Install Horovod

1. install openmpi

    a. download https://www.open-mpi.org/software/ompi/v4.0/
    
    b. 
```
gunzip -c openmpi-4.0.3.tar.gz | tar xf -
cd openmpi-4.0.3
./configure --prefix=your local path
<...lots of output...>
make all install
```
c. Install NCCL 2 
```
git clone https://github.com/NVIDIA/nccl
make src.build CUDA_HOME=<path to cuda install>
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/XXX/nccl-master/build/lib
source bashrc
```
d.Install Horovod
```
    HOROVOD_NCCL_HOME=/home/ynzhou/cgc/nccl/build HOROVOD_GPU_ALLREDUCE=NCCL HOROVOD_GPU_BROADCAST=NCCL HOROVOD_CUDA_HOME=/usr/local/cuda-10.1 pip install --no-cache-dir horovod
```
