# Installation of tensorflow with GPU option

Install cudnn from deb files, if not done so already
```
sudo apt-get install libcupti-dev
sudo apt-get install python-dev
```

If necessary, symbolically link 8.0 binaries to 9.0 *PROBABLY NOT NEEDED FOR SOURCE BUILD*

```
ln -s /usr/local/cuda/lib64/libcublas.so.9.0 /usr/local/cuda/lib64/libcublas.so.8.0
ln -s /usr/local/cuda/lib64/libcusolver.so.9.0 /usr/local/cuda/lib64/libcusolver.so.8.0
ln -s /usr/local/cuda/lib64/libcudart.so.9.0 /usr/local/cuda/lib64/libcudart.so.8.0
ln -s /usr/local/cuda/lib64/libcudnn.so /usr/local/cuda/lib64/libcudnn.so.6
ln -s /usr/local/cuda/lib64/libcufft.so.9.0 /usr/local/cuda/lib64/libcufft.so.8.0
ln -s /usr/local/cuda/lib64/libcurand.so.9.0 /usr/local/cuda/lib64/libcurand.so.8.0
```

Build tensorflow from source. This ensures both the integration of the hardware specific accelaration options and compilation against the latest CUDA libraries.

```
git init
git clone https://github.com/tensorflow/tensorflow 
cd tensorflow
git checkout
```
The tensorflow source build requires the [bazel](https://docs.bazel.build/versions/master/install.html "bazel install instructions")  build and test tool (plus several pre-requisites)
```
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python-wheel
sudo apt-get install python-numpy python-dev

wget https://github.com/bazelbuild/bazel/releases/download/0.8.1/bazel-0.8.1-installer-linux-x86_64.sh
chmod +x bazel-0.8.1-installer-linux-x86_64.sh 
./bazel-0.8.1-installer-linux-x86_64.sh --user
# Add $HOME/bin to PATH in ~./bashrc
```
Ready to build. The build results in a `.whl` file which needs to be install with pip

```
cd ../tensorflow
./configure
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
pip --proxy http://10.168.209.73:8012 install /tmp/tensorflow_pkg/tensorflow-1.4.0-cp27-cp27mu-linux_x86_64.whl
```

Change to another directory (out of ../tensorflow) and create and test a simple tensorflow example to check the set-up.

```
# tf.py
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

`python tf.py` will yield:

```2018-01-05 11:45:14.214623: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:895] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2018-01-05 11:45:14.215232: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1105] Found device 0 with properties: 
name: Quadro M2200 major: 5 minor: 2 memoryClockRate(GHz): 1.036
pciBusID: 0000:01:00.0
totalMemory: 3.94GiB freeMemory: 1.51GiB
2018-01-05 11:45:14.215261: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1195] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Quadro M2200, pci bus id: 0000:01:00.0, compute capability: 5.2)
```
