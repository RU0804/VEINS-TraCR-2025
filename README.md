# VEINS-TraCR-2025
For Graduate research assistant project.



# Getting Started
Installing VEINS 5.3.1 on Ubuntu 22.04.
Reference: Official tutorial[https://veins.car2x.org/tutorial/#step4]; Github installing guide(CN)[https://github.com/Yrongovo/Veins5.2-Ubuntu18.04-Installation-Guide]



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
export SUMO_HOME=$HOME/sumo-1.22.0-install
export PATH=$SUMO_HOME/bin:$PATH
```

Then, everytime you launch omnetpp, just go:
```
cd ~/Downloads/omnetpp-6.0.3/bin
./omnetpp
```









