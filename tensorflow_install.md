# Install cudnn from deb files, if not done so already
sudo apt-get install libcupti-dev
sudo apt-get install python-dev
# Symbolically link 8.0 binaries to 9.0 {PROBABLY NOT NEEDED FOR SOURCE BUILD}
ln -s /usr/local/cuda/lib64/libcublas.so.9.0 /usr/local/cuda/lib64/libcublas.so.8.0
ln -s /usr/local/cuda/lib64/libcusolver.so.9.0 /usr/local/cuda/lib64/libcusolver.so.8.0
ln -s /usr/local/cuda/lib64/libcudart.so.9.0 /usr/local/cuda/lib64/libcudart.so.8.0
ln -s /usr/local/cuda/lib64/libcudnn.so /usr/local/cuda/lib64/libcudnn.so.6
ln -s /usr/local/cuda/lib64/libcufft.so.9.0 /usr/local/cuda/lib64/libcufft.so.8.0
ln -s /usr/local/cuda/lib64/libcurand.so.9.0 /usr/local/cuda/lib64/libcurand.so.8.0
sudo apt-get install git
git init
git clone https://github.com/tensorflow/tensorflow 
cd tensorflow
git checkout
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip
# Download bazel installer (see tensorflow source build instructions)
chmod +x bazel-0.8.1-installer-linux-x86_64.sh 
./bazel-0.8.1-installer-linux-x86_64.sh --user
# Add $HOME/bin to PATH
vi ~/.bashrc
source ~/.bashrc
sudo apt-get install python-wheel
cd ../tensorflow
./configure
sudo pip install six numpy
sudo apt-get install python-numpy python-dev
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
pip --proxy http://10.168.209.73:8012 install /tmp/tensorflow_pkg/tensorflow-1.4.0-cp27-cp27mu-linux_x86_64.whl
# cd out of the tnesorflow install directory
# create the simple tf test file and run
