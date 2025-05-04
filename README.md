# VEINS-TraCR-2025
For Graduate research assistant project.



# Getting Started
Installing VEINS 5.3.1 on Ubuntu 22.04.
Reference: 
Official tutorial[https://veins.car2x.org/tutorial/#step4]; Github installing guide(CN)[https://github.com/Yrongovo/Veins5.2-Ubuntu18.04-Installation-Guide]  

OMNeT++ installation guide: {Installation Guide}[https://github.com/RU0804/VEINS-TraCR-2025/blob/main/InstallGuide.pdf]



Always run these before launching OMNeT++ IDE if you have not set PATH in /.bashrc:
```
cd ~/Downloads/omnetpp-6.0.3
source setenv
cd bin
./omnetpp
```

tbd

To save some time launching, remember to add env PATH:
Go to 
```
gedit ~/.bashrc
```
And add 
```
export PATH=$PATH:$HOME/omnetpp-6.0.3/bin
```

Then, everytime you launch omnetpp, just go:
```
cd ~/Downloads/omnetpp-6.0.3/bin
./omnetpp
```

### 🔄 Re-Activating the `.venv` Environment After Restart

If you restart your system or open a new terminal, the `.venv` Python environment used by OMNeT++ will not be active by default. You must manually activate it before working with OMNeT++.

#### ✅ Step 1: Manually Activate `.venv`

Run the following commands:

```bash
cd ~/omnetpp-6.0.3
source .venv/bin/activate
```



## 🚗 Installing SUMO 1.22.0 from Source (VEINS-Compatible)

This section documents how to install **SUMO 1.22.0** from source with minimal dependencies to ensure full compatibility with **VEINS 5.3.1** and **OMNeT++ 6.0.3**.

### ✅ Step 1: Remove Existing SUMO (if installed via apt)
Do this only if you have already installed SUMO with ```sudo apt-get install sumo sumo-tools sumo-doc```. Otherwise, skip to Step 2.
```bash
sudo apt remove --purge sumo sumo-tools sumo-doc libgdal-dev libfmt-dev
sudo apt autoremove
```


### 📦 Step 2: Install Required Dependencies (Minimal Setup)
```bash
sudo apt update
sudo apt install -y \
  cmake g++ libxerces-c-dev libfox-1.6-dev swig libtool \
  libgl2ps-dev python3-dev python3-pip \
  python3-numpy python3-pyproj python3-matplotlib libspatialindex-dev
```

### 📥 Step 3: Download and Prepare SUMO Source Code
```bash
cd ~
wget https://github.com/eclipse-sumo/sumo/archive/refs/tags/v1_22_0.tar.gz
tar -xzf v1_22_0.tar.gz
mv sumo-1_22_0 sumo-1.22.0
cd sumo-1.22.0
```


### 🛠 Step 4: Configure the Build (Disable Incompatible Modules)
```bash
mkdir build
cd build
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/sumo-1.22.0-install \
  -DCMAKE_DISABLE_FIND_PACKAGE_fmt=ON \
  -DUSE_GDAL=OFF \
  -DUSE_PROTOBUF=OFF \
  -DUSE_FFMPEG=OFF \
  -DUSE_GL2PS=OFF \
  -DUSE_OSM=OFF \
  -DUSE_TRACI=ON \
  -DUSE_NETGENERATE=ON \
  -DBUILD_SHARED_LIBS=OFF
```

### 🔨 Step 5: Build and Install
```bash
make -j$(nproc)
make install
```

### 🧪 Step 6: Set Environment Variables
Edit your ~/.bashrc file:
```bash
gedit ~/.bashrc
```
Add these lines to the bottom:
```bash
export SUMO_HOME=$HOME/sumo-1.22.0-install
export PATH=$SUMO_HOME/bin:$PATH
```
Apply change
```bash
source ~/.bashrc
```

### ✅ Step 7: Verify Installation
```bash
sumo --version
```
Expected output
```nginx
SUMO version 1.22.0
```









