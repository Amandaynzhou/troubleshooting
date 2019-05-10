# virtualenv-server
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

if gcc<4.9, maskrcnn-benchmark will have 'segment dump' problem. If you do not have root, but want to upgrade gcc version, check http://luiarthur.github.io/gccinstall

If UnicodeDecodeError accured when collect env, a pytorch bug need to be fixed before run the code. Check:https://github.com/facebookresearch/maskrcnn-benchmark/issues/265

slurm
`squeue -u USERNAME`
http://bicmr.pku.edu.cn/~wenzw/pages/slurm.html

wget with proxy
`wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz -e use_proxy=yes -e https_proxy=your proxy`
