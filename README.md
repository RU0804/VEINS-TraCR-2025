# VEINS-TraCR-2025
For Graduate research assistant project.



# 1. Getting Started
## 1.1 Installing: VEINS 5.3.1 + SUMO 1.22.0 + OMNeT++ 6.0.3
Installing VEINS 5.3.1 on Ubuntu 22.04.
Reference: 
Official tutorial[https://veins.car2x.org/tutorial/#step4]; Github installing guide(CN)[https://github.com/Yrongovo/Veins5.2-Ubuntu18.04-Installation-Guide]  

OMNeT++ installation guide: {Installation Guide}[https://github.com/RU0804/VEINS-TraCR-2025/blob/main/InstallGuide.pdf]
### 1.1.a OMNeT++ 6.0.3
This guide explains how to **Install OMNeT++ 6.0.3 from the pre‑compiled binary `.tgz`, and set up the optional Python virtual environment** recommended by the OMNeT++ team.  
All commands are intended for **Bash** and have been tested on Ubuntu 22.04.

**📦 Perpare the package**


Download OMNeT++ zip file to the target location. For example, `~/omnetpp-6.0.3-linux-x86_64.tgz`

Refresh the database of available packages. Install them in one go:
```bash
sudo apt update
sudo apt install build-essential clang lld gdb bison flex perl \\
     python3 python3-pip libpython3-dev \\
     qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools \\
     libqt5opengl5-dev libxml2-dev zlib1g-dev doxygen graphviz \\
     libwebkit2gtk-4.1-0 xdg-utils libdw-dev
sudo apt install libopenscenegraph-dev
```

**📦 Extract the OMNeT++ Archive**
```bash
cd ~
tar -xzf ~/omnetpp-6.0.3-linux-x86_64.tgz
```

**🐍 Create & Activate a Python Virtual Environment**
```bash
cd ~/omnetpp-6.0.3
python3 -m venv .venv
source .venv/bin/activate        # prompt shows (.venv)
python -m pip install -r python/requirements.txt
```
`.venv` keeps OMNeT++ IDE’s Python modules (`numpy`, `scipy`, `pandas`, `matplotlib`, `posix_ipc`, …) isolated.  

**⚙️ Configure**
```bash
cd ~/omnetpp-6.0.3
source setenv
./configure
```

**🚀 Build and Test**
```bash
make -j$(nproc)
cd ~/omnetpp-6.0.3/bin
./omnetpp
```
`make -j$(nproc)` might take 30 mintues or more.


Always run these before launching OMNeT++ IDE if you have not set PATH in /.bashrc:
```
cd ~/Downloads/omnetpp-6.0.3
source setenv
cd bin
./omnetpp
```
Or,  

Add this to ~/.bashrc so every new terminal has OMNeT++ available:
```bash
source ~/omnetpp-6.0.3/setenv
alias omnetpp-dev="cd ~/omnetpp-6.0.3 && source .venv/bin/activate"
source ~/.bashrc
```


**🔄 Re-Activating the `.venv` Environment After Restart**

If you restart your system or open a new terminal, the `.venv` Python environment used by OMNeT++ will not be active by default. You must manually activate it before working with OMNeT++.

**✅ Manually Activate `.venv`**

Run the following commands:

```bash
cd ~/omnetpp-6.0.3
source .venv/bin/activate
```

Then launch OMNeT++.


### 1.1.b SUMO 1.22.0 from Source (VEINS-Compatible)

This section documents how to install **SUMO 1.22.0** from source with minimal dependencies to ensure full compatibility with **VEINS 5.3.1** and **OMNeT++ 6.0.3**.

**✅ Remove Existing SUMO (if installed via apt)**
Do this only if you have already installed SUMO with ```sudo apt-get install sumo sumo-tools sumo-doc```. Otherwise, skip to Step 2.
```bash
sudo apt remove --purge sumo sumo-tools sumo-doc libgdal-dev libfmt-dev
sudo apt autoremove
```


**📦 Install Required Dependencies (Minimal Setup)**
```bash
sudo apt update
sudo apt install -y \
  cmake g++ libxerces-c-dev libfox-1.6-dev swig libtool \
  libgl2ps-dev python3-dev python3-pip \
  python3-numpy python3-pyproj python3-matplotlib libspatialindex-dev
```

**📥 Download and Prepare SUMO Source Code**
```bash
cd ~
wget https://github.com/eclipse-sumo/sumo/archive/refs/tags/v1_22_0.tar.gz
tar -xzf v1_22_0.tar.gz
mv sumo-1_22_0 sumo-1.22.0
cd sumo-1.22.0
```


**🛠 Configure the Build (Disable Incompatible Modules)**
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

**🔨 Build and Install**
```bash
make -j$(nproc)
make install
```

**🧪 Set Environment Variables**
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

**✅ Verify Installation**
```bash
sumo --version
```
Expected output
```nginx
SUMO version 1.22.0
```



### 1.1.c VEINS 5.3.1
Download and Extract VEINS. Make a new folder `my`, create `lte` and `ve`, put VEINS into `ve`. Then locate to `examples` folder.
Example:
```bash
cd ~/omnetpp-6.0.3/samples/my/ve/veins-5.3.1/veins-veins-5.3.1/examples
sumo-gui -c erlangen.sumo.cfg
```

Then
```bash
cd ~/omnetpp-6.0.3/samples/my/ve/veins-5.3.1/veins-veins-5.3.1/
./configure
make -j$(nproc)
python sumo-launchd.py -vv -c sumo-gui
```
When you see `Listening on port 9999`, switch back to OMNeT++ IDE.

Go to `File`->`Import`->`General`->`Existing Projects into Workspace`->select root directory, for example `/home/joey/omnetpp-6.0.3/samples/my/ve/veins-5.3.1/veins-veins-5.3.1/exapmles/veins`
Select and click Next.  
Right click `omnetpp.ini` in veins folder and select `Run As` -> `1 OMNeT++ Simulation`






