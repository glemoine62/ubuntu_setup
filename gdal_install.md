# building GDAL from source

## Pre-requisites

Check availability of [latest] PNG, TIFF and LCMS libraries. Install if not available.
JPEG2000 build requires cmake

```
sudo apt-get install libtiff5 libtiff5-dev
sudo apt-get install liblcms2-2 liblcms2-dev 
sudo apt-get install cmake
```

## Build JPEG2000 from source

JPEG2000 is required for Sentinel-2. Best if built from source.
Note that we have to install in `/usr/local`

```
wget https://github.com/uclouvain/openjpeg/archive/v2.2.0/openjpeg-2.2.0.tar.gz
tar xfz openjpeg-2.2.0.tar.gz 
cd openjpeg-2.2.0/
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..
make -j8
make install
```

## Build GDAL from source

```
wget http://download.osgeo.org/gdal/2.2.3/gdal-2.2.3.tar.gz
tar xfz gdal-2.2.3.tar.gz
cd gdal-2.2.3/
./configure --prefix=/usr/local --with-python
make -j8
make install
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
# Check that gdal binaries work
gdalinfo --version
# Check that GDAL python bindings work
python -c 'from osgeo import gdal'
```

Better to add LD_LIBRARY_PATH export to `.bashrc`

