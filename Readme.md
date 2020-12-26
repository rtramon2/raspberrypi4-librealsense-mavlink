Raspberry Pi 4 mavlink with librealsense install proceadure. This uses the 2.36 version of librealsene, newer versions will give a pipeline error.

# Initial Setup & Mavlink Install
1. download and install Debian Buster https://www.raspberrypi.org/software/
2. update the os:
	- Sudo apt update
	- Sudo apt upgrade
3. install dependencies
	- Sudo apt install python-dev
	- Sudo apt install python3-dev python3-opencv python3-wxgtk4.0 libxml2-dev python3-pip python3-matplotlib python3-lxml
4. install mavlink:
```bash
sudo apt-get install python3-dev python3-opencv python3-wxgtk4.0 python3-pip python3-matplotlib python3-lxml python3-pygame
pip3 install PyYAML mavproxy --user
echo "export PATH=$PATH:$HOME/.local/bin" >> ~/.bashrc
```

	- https://ardupilot.org/mavproxy/docs/getting_started/download_and_installation.html#mavproxy-downloadinstalllinux
# Librealsense Install Procedure:
# Install dependencies
* sudo apt install git libssl-dev libusb-1.0-0-dev pkg-config -y
* sudo apt install cmake python3-dev raspberrypi-kernel-headers -y
# Clone Librealsense 2.36
#it is important to clone the 2.36 branch any other version will not compile #pyrealsense correctly
* git clone https://github.com/IntelRealSense/librealsense.git --branch v2.36.0
* cd librealsense

# Install udev rules
* sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/
* sudo udevadm control --reload-rules && udevadm trigger

# Create the destination directory
* mkdir build
* cd build

# Remove extra files if this is not your first run
```bash
xarg sudo rm < install_manifest.txt
rm CMakeCache.txt
export CC=/usr/bin/gcc-6
export CXX=/usr/bin/g++-6
```
# Make and build librealsense
#note to build with python2 change PYTHON_EXECUTABLE to /usr/bin/python
```bash
cmake -D CMAKE_BUILD_TYPE="Release"\
-D FORCE_LIBUVC=ON \
-D BUILD_PYTHON_BINDINGS=ON -D BUILD_PYTHON_BINDINGS=TRUE \
-D BUILD_EXAMPLES=ON -D PYTHON_EXECUTABLE=/usr/bin/python3 ..

make -j4
sudo make install
sudo ldconfig

# Finally, reboot the pi:
sudo reboot
```	
# download test scripts to the librealsense/build/wrappers/python folder. All python scripts using pyrealsense 2 must be located in the librealsense/build/wrappers/python folder otherwise they will not run. 
* https://raw.githubusercontent.com/thien94/vision_to_mavros/master/scripts/t265_to_mavlink.py
* https://github.com/IntelRealSense/librealsense/tree/master/wrappers/python/examples

# setup mavlink-router 
* https://github.com/mavlink-router/mavlink-router
