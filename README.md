NVIDIA Jetson IMX708 RPI V3 camera driver
This driver has been developed by RidgeRun Engineering as an initiative in order to release the first version of the Sony IMX708 sensor driver for the Raspberry Pi Camera Module 3.

Supports the following Jetson platforms:

Jetson Xavier NX
Jetson Nano
Repository structure
In this repository you will find the following structure:

.
├── li-camera-calibration-files
│   └── camera_overrides.isp
├── patches_nano
│   ├── 4.5_nano_imx477-v0.1.0.patch
│   └── series
├── patches_nx
│   ├── 4.5_nx_imx477-v0.1.0.patch
│   └── series
└── README.md
where:

4.5_*_imx477-v0.1.0.patch is the patch to be applied on the JetPack 4.5 sources in order to add support for the IMX477 camera sensor in the Jetson Xavier NX and Jetson Nano.
series is a file containing the patch name in order to apply it by using the quilt tool.
camera_overrides.isp is a file which contains a set of calibration parameters for the Image Signal Processor of the HQ camera.
JetPack installation instructions
You can download and install the JetPack by following the instructions below:

JetPack download and installation instructions
Driver Installation instructions
There are two options to install the driver:

OPTION A: Installing the kernel and dtb debians (Recommended)
This is the easiest and fastest way to install the driver. In order to install the debian packages you just need to perform the following instructions:

Installing the kernel and dtb debians
OPTION B: Applying the patches on the sources
In order to apply the patch on the JetPack sources with Xavier NX and Nano support, you must perform the following instructions:

JetPack sources download instructions
Patch instructions
Kernel build instructions
Flash the Jetson
ISP camera overrides installation
Visit the Custom ISP Configuration page in order to learn how to use the camera_overrides.isp file to configure the ISP.

Supported features
Resolutions and framerates
1920x1080 @ 60fps
4032x3040 @ 30fps
Controls
Gain
Exposure
Framerate
Group Hold
Example pipelines
Display
1920x1080
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=60 # Framerate can go from 2 to 60 for 1920x1080 mode
gst-launch-1.0 nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=1920,height=1080,framerate=$FRAMERATE/1" ! nvvidconv ! nvoverlaysink
4032x3040
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=30 # Framerate can go from 2 to 30 for 4032x3040 mode
gst-launch-1.0 nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4032,height=3040,framerate=$FRAMERATE/1" ! nvvidconv ! "video/x-raw(memory:NVMM),width=1920,height=1080,framerate=$FRAMERATE/1" ! nvoverlaysink
MP4 Recording
1920x1080
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=60 # Framerate can go from 2 to 60 for 1920x1080 mode
gst-launch-1.0 -e nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=1920,height=1080,framerate=$FRAMERATE/1" ! nvv4l2h264enc ! h264parse ! mp4mux ! filesink location=rpi_v3_imx477_cam$SENSOR_ID.mp4
4032x3040
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=30 # Framerate can go from 2 to 30 for 4032x3040 mode
gst-launch-1.0 -e nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4032,height=3040,framerate=$FRAMERATE/1" ! nvv4l2h264enc ! h264parse ! mp4mux ! filesink location=rpi_v3_imx477_cam$SENSOR_ID.mp4
JPEG snapshots
1920x1080
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=60 # Framerate can go from 2 to 60 for 1920x1080 mode
NUMBER_OF_SNAPSHOTS=20
gst-launch-1.0 -e nvarguscamerasrc num-buffers=$NUMBER_OF_SNAPSHOTS sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=1920,height=1080,framerate=$FRAMERATE/1" ! nvjpegenc ! multifilesink location=%03d_rpi_v3_imx477_cam$SENSOR_ID.jpeg
4032x3040
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=30 # Framerate can go from 2 to 30 for 4032x3040 mode
NUMBER_OF_SNAPSHOTS=20
gst-launch-1.0 -e nvarguscamerasrc num-buffers=$NUMBER_OF_SNAPSHOTS sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4032,height=3040,framerate=$FRAMERATE/1" ! nvjpegenc ! multifilesink location=%03d_rpi_v3_imx477_cam$SENSOR_ID.jpeg