# virtualenv-server
记录在server创建venv环境过程,方便以后使用

'''bash
    git config --global http.proxy http://proxy.cse.cuhk.edu.hk:8000/
    git clone xxxx...
    virtualenv --no-site-packages --python=python3.6 pytorch1.0
    pip install --proxy=http://proxy.cse.cuhk.edu.hk:8000/ package-name
'''


** may need to set cuda path in env

#### set cocoapi
  git clone https://github.com/cocodataset/cocoapi.git

*import pycocotools._mask as _mask 出现 ValueError: numpy.ufunc size changed, may indicate binary incompatibility. Expected 216 from C header, got 192 from PyObject----> upgrade numpy 1.15.4 to 1.16.0

#### install other requirements
  pip install --proxy=http://proxy.cse.cuhk.edu.hk:8000/ -r requirement.txt
