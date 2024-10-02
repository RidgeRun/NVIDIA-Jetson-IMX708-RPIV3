# NVIDIA Jetson IMX708 RPI V3 camera driver
This driver has been developed by RidgeRun Engineering as an initiative in order to release the first version of the Sony IMX708 sensor driver for the Raspberry Pi Camera Module 3.

Supports the following Jetson platforms:
* Jetson Orin Nano
* Jetson Nano

## Repository structure

In this repository you will find the following structure:
```
.
├── patches_orin_nano
|   └── patches
│       ├── 5.1.1_orin_nano_imx708-v0.1.0.patch
│       └── series
├── patches_nano
|   └── patches
│       ├── 4.6.4_nano_imx708-v0.1.0.patch
│       └── series
└── README.md
```
where:

* `5.1.1_*_imx708-v0.1.0.patch` is the patch to be applied on the JetPack 5.1.1 sources in order to add support for the IMX708 camera sensor in the Jetson Orin Nano.
* `series` is a file containing the patch name in order to apply it by using the quilt tool. The same applies to '4.6.4_nano_imx708-v0.1.0.patch'.

## JetPack installation instructions

You can download and install the JetPack by following the instructions below:

* [JetPack download and installation instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Download_JetPack)

## Driver Installation instructions

There are two options to install the driver:

### OPTION A: Installing the kernel and dtb debians (Recommended)

**Note:** JetPack 6.0 is not supported using this method.

This is the easiest and fastest way to install the driver. In order to install the debian packages you just need to perform the following instructions:

* [Installing debians](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Installing_the_Driver_-_Option_A:_Debian_Packages_.28Recommended.29)

### OPTION B: Applying the patches on the sources

In order to apply the patch on the JetPack sources with Orin Nano and Nano support, you must perform the following instructions:
#### For Jetson Orin Nano ####
* [Install required software](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Install_dependencies)
* [JetPack sources download instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Get_the_source_code_from_NVIDIA_oficial_repository)
* [Patch instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Get_the_driver_patches)
* [Setup toolchain](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Set_up_the_toolchain)
* [Kernel build instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Build)
* [Flash the Jetson](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Installation_options)
#### For Jetson Nano ####
* [Install required software](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Install_dependencies)
* [JetPack sources download instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Get_the_source_code_from_NVIDIA_oficial_repository)
* [Patch instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Get_the_driver_patches_2)
* [Setup toolchain](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Set_up_the_toolchain_2)
* [Kernel build instructions](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Build_2)
* [Flash the Jetson](https://developer.ridgerun.com/wiki/index.php/Raspberry_Pi_Camera_Module_3_IMX708_Linux_driver_for_Jetson#Installation_options_2)

## Supported features

### Resolutions and framerates

* 4608x2592 @ 14fps

### Controls

* Gain
* Exposure
* Framerate
* Group Hold

## Example pipelines

### Display

* 4608x2592

```
SENSOR_ID=0 # 0 for CAM0 port
FRAMERATE=14 # Framerate can go from 2 to 14 for 4608x2592 mode
gst-launch-1.0 nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4608,height=2592,framerate=$FRAMERATE/1" ! queue ! nvegltransform ! nveglglessink 
```


### MP4 Recording

* 4608x2592

```
SENSOR_ID=0 # 0 for CAM0 port
FRAMERATE=14 # Framerate can go from 2 to 14 for 4608x2592 mode
gst-launch-1.0 -e nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4608,height=2592,framerate=$FRAMERATE/1" ! nvv4l2h264enc ! h264parse ! mp4mux ! filesink location=rpi_v3_imx708_cam$SENSOR_ID.mp4
```

### JPEG snapshots

* 4608x2592

```
SENSOR_ID=0 # 0 for CAM0 port
FRAMERATE=60 # Framerate can go from 2 to 14 for 4608x2592 mode
NUMBER_OF_SNAPSHOTS=20
gst-launch-1.0 -e nvarguscamerasrc num-buffers=$NUMBER_OF_SNAPSHOTS sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4608,height=2592,framerate=$FRAMERATE/1" ! nvjpegenc ! multifilesink location=%03d_rpi_v3_imx708_cam$SENSOR_ID.jpeg
```


## RidgeRun

Check our other products in [ridgerun.com](https://www.ridgerun.com) and don't hesitate to contact us if you have any kind of problem with the instructions given by reaching us at https://www.ridgerun.com/contact




