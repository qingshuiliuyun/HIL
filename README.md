##Hardware in the loop tool for PX4 Firmware

This is a tool for running hardware-in-the-loop (HIL) simulations for the px4 autopilot.

###HIL modes:

* State-level HIL: tests the control and guidance systems.
* Sensor-level HIL: tests the navigation system in addition to the control, and guidance systems. This requires a lot of data to be sent to the vehicle and a high baudrate.

## Dependencies

### PyMAVLink

Pymavlink is the python library required for running the runhil.py script, you can install it with easy install or pypi-install

If you are using a debian based system (e.g. Ubuntu), you can install using pypi-install so that it can be installed as debian package:

```
sudo apt-get install python-stdeb
pypi-install pymavlink
```

If you are using Mac OS / other unix systems:

```
sudo easy_install pymavlink
```

### JSBSim

JSBSim is the C++ flight dynamics model. It can be built with cmake/ or autotools.

```
brew install autoconf automake
```

```
git clone https://github.com/PX4/jsbsim.git jsbsim
cd jsbsim
./autogen.sh
make
```

## Usage

### NSH Startup Script

You must use the startup script found in data/rc. The two main lines added to normal startup are:
```
kalman_demo start
control_demo start
```

### Python Script

This python script runhil.py is used for conducting HIL. Both sensor-level and state-level HIL are supported. You can view the runhil.py usage with:
```
runhil.py -h
```

A call to runhil.py might look like this:
```
./runhil.py --waypoints data/sf_waypoints.txt --master /dev/ttyUSB1 --gcs localhost:14550 --mode sensor
```

Explanation:
* load the given waypoints in data/sf_waypoints.txt, NOTE: QGC waypoints are not compatible with pymavlink currently, you simply need to change the version number in the file to 110 if it is 120
* connect to the px4 autopilot on usb port 1
* setup external ground station communication of localhost:14550 udp.
* mode sensor says do sensor-level hardware-in-the-loop (HIL), this can be set to state as well

### GroundControl Interface
Note that the script defaults to opening a mavlink slave port on udp:14550, this is the default udp port for QGroundControl. If you start QGC, it should start communicating with runhil.py automatically.

## Notes

### TODO:

* Magnetometer measurement model doesn't depend on lat/lot yet.
* Add noise.
* Initialization routines for EKF.

### Source

This uses pymavlink and is based off of MAVProxy/ ardupilotemga SITL code.
